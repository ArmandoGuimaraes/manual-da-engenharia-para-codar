Entendi sua solicitação. Vou traduzir o documento técnico sobre o uso do Azure Website conforme as diretrizes fornecidas no seu prompt inicial.

# Implante o site de documentação DocFx automaticamente em um site do Azure

No artigo [Usando o DocFx e Ferramentas Complementares para Gerar um Site de Documentação](using-docfx-and-tools.md), descreve-se o processo de geração de conteúdo de um site de documentação usando o DocFx. Este documento descreve como configurar um site do Azure para hospedar o conteúdo e automatizar a implantação usando um pipeline no Azure DevOps.

A [amostra QuickStart](https://github.com/mtirionMSFT/DocFxQuickStart) fornecida para uma configuração rápida da geração do DocFx também contém os arquivos explicados neste documento. Especialmente as pastas **.pipelines** e **infrastructure**.

Os seguintes passos podem ser seguidos ao usar a pasta Quick Start. Na pasta **infrastructure**, você pode encontrar os arquivos Terraform para criar o site em um ambiente do Azure. Por padrão, o script criará um site onde o conteúdo da documentação pode ser implantado.

## 1. Instale o Terraform

Você pode usar ferramentas como o [Chocolatey](https://chocolatey.org/) para instalar o Terraform:

```shell
choco install terraform
```

## 2. Configure as variáveis apropriadas

> **IMPORTANTE:** Certifique-se de modificar o valor das variáveis **app_name**, **rg_name** e **rg_location**. O valor de *app_name* é acrescentado por **azurewebsites.net** e deve ser exclusivo. Caso contrário, o script falhará ao criar o site.

Na Quick Start, a autenticação está desativada. Se você deseja ativá-la, certifique-se de criar um *Aplicativo* no Azure AD e obter o *ID do cliente*. Este ID do cliente deve ser definido como o valor da variável **client_id** em *variables.tf*. No *main.tf*, certifique-se de descomentar as configurações de autenticação em *app-service*. Para obter mais informações, consulte [Configurar a autenticação do Azure AD - Azure App Service](https://learn.microsoft.com/en-us/azure/app-service/configure-authentication-provider-aad).

Se você deseja configurar um domínio personalizado para o seu site de documentação com um certificado SSL, precisará seguir algumas etapas adicionais. Você deve criar um *Key Vault* e armazenar o certificado lá. O próximo passo é descomentar e definir os valores em *variables.tf*. Você também deve descomentar as etapas necessárias em *main.tf*. Tudo é indicado por caixas de comentários. Para obter mais informações, consulte [Adicionar um certificado TLS/SSL no Azure App Service](https://learn.microsoft.com/en-us/azure/app-service/configure-ssl-certificate).

Algumas informações adicionais sobre certificado SSL, domínio personalizado e Azure App Service podem ser encontradas nos parágrafos a seguir. Se você estiver familiarizado com isso ou não precisar disso, prossiga com [Passo 3](#3-implante-recursos-do-Azure-a-partir-do-seu-computador-local).

### Certificado SSL

Para proteger um site com um nome de domínio personalizado e um certificado, você pode encontrar as etapas a serem seguidas no artigo [Adicionar um certificado TLS/SSL no Azure App Service](https://learn.microsoft.com/en-us/azure/app-service/configure-ssl-certificate). Esse artigo também contém uma descrição das maneiras de obter um certificado e os requisitos para um certificado. Normalmente, você obterá um certificado do departamento de TI do cliente. Se você deseja começar com um certificado de desenvolvimento para testar o processo, pode criar um por conta própria. Você pode fazer isso no PowerShell com o seguinte script. Substitua:

* **[SEU DOMÍNIO]** pelo domínio que deseja registrar, por exemplo, `docs.somewhere.com`.
* **[SENHA]** por uma senha do certificado. É necessário ter uma senha ao carregar um certificado no Key Vault. Você precisará dessa senha nessa etapa.
* **[NOME DO ARQUIVO]** pelo nome do arquivo de saída do certificado. Você pode até inserir o caminho aqui onde ele deve ser armazenado em sua máquina.

Você pode armazenar este script em um arquivo de script do PowerShell (com extensão ps1).

```powershell
$cert = New-SelfSignedCertificate -CertStoreLocation cert:\currentuser\my -Subject "cn=[SEU DOMÍNIO]" -DnsName "[SEU DOMÍNIO]"
$pwd = ConvertTo-SecureString -String '[SENHA]' -Force -AsPlainText
$path = 'cert:\currentuser\my\' + $cert.thumbprint
Export-PfxCertificate -cert $path -FilePath [NOME DO ARQUIVO].pfx -Password $pwd
```

O certificado precisa ser armazenado no Key Vault comum. Acesse `Configurações > Certificados` no menu esquerdo do Key Vault e clique em `Gerar/Importar`. Forneça esses detalhes:

* Método de criação de certificado: `Importar`
* Nome do certificado: por exemplo, `ssl-certificate`
* Carregar arquivo de certificado: selecione o arquivo no disco.

Senha: esta é a [SENHA] que mencionamos anteriormente.

### Registro de domínio personalizado

Para usar um domínio personalizado, algumas etapas precisam ser executadas. O processo no portal do Azure é descrito no artigo [Tutorial: Mapear um nome DNS personalizado existente para o Azure App Service](https://learn.microsoft.com/pt-br/azure/app-service/app-service-web-tutorial-custom-domain). Uma parte importante é descrita sob o cabeçalho [Obter um ID de verificação de domínio](https://learn.microsoft.com/pt-br/azure/app-service/app-service-web-tutorial-custom-domain#get-a-domain-verification-id). Este ID de verificação de domínio precisa ser registrado com a descrição DNS como um registro TXT.

Importante saber que este "ID de Verificação de Domínio Personalizado" é o mesmo para todos os recursos da Web na mesma assinatura do Azure. Veja [esta questão no StackOverflow](https://stackoverflow.com/questions/64309200/is-the-custom-domain-verification-shared-across-an-azure-subscription). Isso significa que este ID precisa ser registrado apenas uma vez para uma assinatura do Azure. E isso permite a (re)criação de um Serviço de Aplicativo com o domínio personalizado por meio de um script.

### Adicionar Get-permissions para o Microsoft

 Azure App Service

O Azure App Service precisa acessar o Key Vault para obter o certificado. Isso é necessário na primeira execução, mas também quando o certificado é renovado no Key Vault. Para esse fim, o Azure App Service acessa o Key Vault com a identidade de recurso fornecida pelo serviço. Essa identidade pode ser encontrada com o nome principal de serviço **abfa0a7c-a6b6-4736-8310-5855508787cd** ou **Microsoft Azure App Service** e é do tipo **Aplicativo**. Esse ID é o mesmo para todas as assinaturas do Azure. Ele precisa ter permissões Get em segredos e certificados. Para obter mais informações, consulte este artigo [Importar um certificado do Key Vault](https://learn.microsoft.com/pt-br/azure/app-service/configure-ssl-certificate#importar-um-certificado-do-key-vault).

### Adicione o domínio personalizado e o certificado SSL ao Serviço de Aplicativo

Depois de obtermos o certificado SSL e houver um registro DNS completo conforme descrito, podemos descomentar o código no script Terraform da pasta Quick Start para vinculá-lo ao Serviço de Aplicativo. Neste script, você precisa fazer referência ao certificado no Key Vault comum e usá-lo na vinculação de nome de host personalizado. O nome de host personalizado também é atribuído no script. As configurações `ssl_state` devem ser `SniEnabled` se você estiver usando um certificado SSL. Agora a criação do site autenticado com um domínio personalizado está automatizada.

## 3. Implante recursos do Azure a partir do seu computador local

Abra um prompt de comando. Para executar os comandos, você precisa ter uma conexão com sua assinatura do Azure. Isso pode ser feito usando o [Azure CLI](https://learn.microsoft.com/pt-br/cli/azure/install-azure-cli-windows?tabs=azure-cli). Digite este comando:

```shell
az login
```

Isso usará o navegador da web para fazer login na sua conta. Você pode verificar a assinatura conectada com este comando:

```shell
az account show
```

Se você precisar mudar para outra assinatura, use este comando, substituindo *[id]* pelo ID da assinatura a ser selecionada:

```shell
az account set --subscription [id]
```

Depois de fazer isso, execute este comando para inicializar:

```shell
terraform init
```

Agora você pode executar o comando para planejar o que o script fará. Execute este comando sempre que fizer alterações nos scripts do Terraform:

```shell
terraform plan
```

Verifique o resultado mostrado. Se isso for o que você espera, aplique essas alterações com este comando:

```shell
terraform apply
```

Quando for solicitada a aprovação, digite "yes" e pressione ENTER. Você também pode adicionar a opção *-auto-approve* ao comando apply.

A implantação usando o Terraform não está incluída no pipeline da pasta Quick Start, conforme descrito no próximo passo, pois requer mais configuração. Mas é claro que isso pode sempre ser adicionado.

## 4. Implante o site a partir de um pipeline

A melhor maneira de criar os recursos e implantar neles é fazer isso automaticamente em um pipeline. Para esse fim, o pipeline **.pipelines/documentation.yml** é fornecido. Este pipeline é construído para um ambiente Azure DevOps. Crie um pipeline e faça referência a este arquivo YAML.

> **IMPORTANTE:** a pasta Quick Start contém um arquivo web.config necessário para a implantação no IIS ou no Azure App Service. Isso permite o uso do arquivo json para solicitações de pesquisa. Se você não tiver isso, a pesquisa de texto nunca retornará nada e resultará em erros 404 nos bastidores.

Você deve criar uma Conexão de Serviço em seu ambiente do DevOps para se conectar à Assinatura do Azure à qual deseja implantar.

> **IMPORTANTE:** configure as variáveis **AzureConnectionName** como o nome da Conexão de Serviço e **AzureAppServiceName** como o nome que você determinou em *infrastructure/variables.tf*.

Na pasta Quick Start, o pipeline usa `master` como acionador, o que significa que qualquer push feito para o master aciona o pipeline. Você provavelmente mudará isso para outra branch.
