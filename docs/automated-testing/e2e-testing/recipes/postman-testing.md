# Testes com Postman

O objetivo deste documento é fornecer orientações sobre como usar o Newman em seu pipeline de CI/CD para executar testes de ponta a ponta (E2E) definidos em Coleções do Postman, seguindo as melhores práticas de segurança.

Primeiramente, apresentaremos o Postman e o Newman e, em seguida, descreveremos vários casos de uso de testes com o Postman que explicam por que você pode querer ir além dos testes locais com Coleções do Postman.

No caso de uso final, pretendemos usar um script shell que faz referência ao caminho do arquivo da Coleção do Postman e ao caminho do arquivo de Ambiente como entradas para o Newman. Abaixo está um diagrama de fluxo representando o resultado do caso de uso final:

![Dependências do Script E2E do Postman](./../images/postman-e2e-script.png)

## Postman e Newman

[Postman](https://www.postman.com/) é uma plataforma API gratuita para testar APIs. As principais funcionalidades destacadas neste guia incluem:

- Coleções do Postman
- Arquivos de Ambiente do Postman
- Scripts do Postman

[Newman](https://github.com/postmanlabs/newman) é um executor de Coleções do Postman via linha de comando. Ele permite que você execute e teste uma Coleção do Postman diretamente da linha de comando. As principais funcionalidades destacadas neste guia incluem:

- Comando Run do Newman

### O que é uma Coleção

Uma Coleção do Postman é um grupo de requisições salvas que podem ser executadas. Uma coleção pode ser exportada como um arquivo json.

### O que é um Arquivo de Ambiente

Um arquivo de Ambiente do Postman contém variáveis de ambiente que podem ser referenciadas por uma Coleção do Postman válida.

### O que é um Script do Postman

Um Script do Postman é um Javascript hospedado dentro de uma Coleção do Postman que pode ser escrito para executar contra sua Coleção do Postman e Arquivo de Ambiente.

### O que é o Comando Run do Newman

Um comando CLI do Newman que permite especificar uma Coleção do Postman a ser executada.

### Instalando o Postman e o Newman

Para instruções específicas sobre como instalar o Postman, visite a [página de Downloads do Postman](https://www.postman.com/downloads/).

Para instruções específicas sobre como instalar o Newman, visite a [página do pacote Newman no NPMJS](https://www.npmjs.com/package/newman).

## Implementando Testes Automatizados de Ponta a Ponta (E2E) com Coleções do Postman

Para fornecer orientações sobre como implementar testes E2E automatizados com o Postman, a seção abaixo começa com um caso de uso que explica os compromissos que um desenvolvedor ou analista de QA pode enfrentar ao pretender usar o Postman para testes iniciais. Cada caso de uso representa cenários que facilitam o objetivo final de testes E2E automatizados.

### Caso de Uso - Teste Funcional Manual de Pontos de Extremidade

Um desenvolvedor ou analista de QA gostaria de testar localmente dados de entrada contra serviços de API que compartilham um token oauth2 comum. Como resultado, eles usam o Postman para criar uma suíte de testes de API de Coleções do Postman que podem ser executadas localmente contra pontos de extremidade individuais em diferentes ambientes. Após validar que sua Coleção do Postman funciona, eles a compartilham com sua equipe.

Os passos podem ser os seguintes:

1. Para cada um dos seus serviços de API existentes, use o recurso de importação do IDE do Postman para importar sua Especificação OpenAPI (Swagger) como uma Coleção do Postman.

    Se um serviço ainda não estiver usando o Swagger, procure orientações específicas de linguagem sobre como usar o Swagger para gerar uma Especificação OpenAPI para o seu serviço. Finalmente, se o seu serviço tiver apenas alguns pontos de extremidade, leia a documentação do Postman para orientações sobre como criar manualmente uma Coleção do Postman.

2. Forneça clareza extra sobre uma requisição em uma Coleção do Postman usando o recurso Exemplo do Postman para salvar suas respostas como exemplos. Você também pode simplesmente adicionar um exemplo manualmente. Por favor, leia a documentação do Postman para orientações sobre como especificar exemplos.
3. Combine cada Coleção do Postman em uma Coleção do Postman centralizada.
4. Construa arquivos de Ambiente do Postman (local, Dev e/ou QA) e parametrize todas as requisições salvas da Coleção do Postman de forma que referencie os arquivos de Ambiente do Postman.
5. Use o recurso de Script do Postman para criar um script de pré-busca compartilhado que atualiza automaticamente os tokens de autenticação expirados por requisição salva. Isso exigiria referenciar segredos de um arquivo de Ambiente do Postman.

    ```javascript
    // Por favor, trate isso como pseudocódigo e ajuste conforme necessário.

    /* A requisição para um ponto de extremidade de autorização oauth2 que emitirá um token 
    com base nas credenciais fornecidas. */
    const oauth2Request = POST {...};
    var getToken = true;
    if (pm.environment.get('ACCESS_TOKEN_EXPIRY') <= (new Date()).getTime()) {
        console.log('Token expirou')
    } else {
        getToken = false;
        console.log('Token e data de expiração estão bons');
    }
    if (getToken === true) {
        pm.sendRequest(oauth2Request, function (_, res) {
                console.log('Salve o token')
                var responseJson = res.json();
                pm.environment.set('token', responseJson.access_token)
                console.log('Salve a data de expiração')
                var expiryDate = new Date();
                expiryDate.setSeconds(expiryDate.getSeconds() + responseJson.expires_in);
                pm.environment.set('ACCESS_TOKEN_EXPIRY', expiryDate.getTime());
        });
    }
    ```

6. Use o IDE do Postman para exercitar pontos de extremidade.
7. Exporte a coleção e os arquivos de ambiente e, em seguida, remova quaisquer segredos antes de fazer o commit no seu repositório.

Começar com essa abordagem tem as seguintes vantagens:

- Você se preparou para as etapas iniciais de uma coleção de postman E2E, agregando as coleções em um único arquivo e usando arquivos de ambiente para facilitar a troca de ambientes.
- O token é atualizado automaticamente em cada chamada na coleção. Isso economiza tempo normalmente perdido ao ter que solicitar manualmente um token que expirou.
- Concede ao QA/Dev controle granular para enviar combinações de dados de entrada por ponto de extremidade.
- Concede aos desenvolvedores uma experiência comum por meio dos recursos do IDE do Postman.

Terminar com essa abordagem tem as seguintes desvantagens:

- Promove o compartilhamento inseguro de segredos. As credenciais necessárias para solicitar o token JWT no script de pré-busca estão sendo compartilhadas manualmente.
- Segredos podem acabar sendo expostos no histórico de commits do git por várias razões (ex. Compartilhando os arquivos de Ambiente do Postman exportados).
- As coleções só podem ser usadas localmente para acessar APIs (locais ou implantadas). Não é baseado em CI.
- Cada desenvolvedor tem que manter atualizados tanto sua Coleção do Postman quanto seus arquivos de Ambiente do Postman para acompanhar as últimas alterações nos serviços implantados.

### Caso de Uso - Teste Funcional Manual de Pontos de Extremidade com Azure Key Vault e Azure App Config

Um desenvolvedor ou analista de QA pode ter uma suíte de testes de API existente de Coleções do Postman; no entanto, agora eles querem desencorajar o compartilhamento inseguro de segredos. Como resultado, eles constroem um script que se conecta tanto ao Key Vault quanto ao Azure App Config para gerar automaticamente arquivos de Ambiente do Postman, em vez de registrá-los em um repositório compartilhado.

Os passos podem ser os seguintes:

1. Crie um Azure Key Vault e armazene segredos de autenticação por ambiente:
    - `"Chave:valor"` (ex. `"dev-auth-password:12345"`)
    - `"Chave:valor"` (ex. `"qa-auth-password:12345"`)
2. Crie uma instância compartilhada do Azure App Configuration e salve todas as suas variáveis de ambiente do Postman. Esta instância será dedicada a manter todas as suas variáveis de ambiente do Postman:
    > NOTA: Use o recurso de Rótulo (Label) para distinguir entre ambientes.
    - `"Chave:valor" -> "apiRoute:url"` (ex. `"servicename:https://servicename.net" & Rótulo = "QA"`)
    - `"Chave:valor" -> "Header:value"`(ex. `"token: " & Rótulo = "QA"`)
    - `"Chave:valor" -> "KeyVaultKey:KeyVaultSecret"` (ex. `"authpassword:qa-auth-password" & Rótulo = "QA"`)

3. Instale o Powershell ou o Bash. O Powershell funciona tanto para o Azure Powershell quanto para o Azure CLI.
4. Baixe o Azure CLI, faça login na assinatura apropriada e certifique-se de ter acesso aos recursos apropriados. Alguns comandos úteis estão abaixo:

    ```powershell
    # fazer login na assinatura apropriada
    az login
    # validar o login
    az account show
    # validar o acesso ao Key Vault
    az keyvault secret list --vault-name "$KeyvaultName"
    # validar o acesso ao App Configuration
    az appconfig kv list --name "$AppConfigName"
    ```

5. Construa um script que gere automaticamente seus arquivos de ambiente.
    > NOTA: O App Configuration faz referência ao Key Vault; no entanto, seu script é responsável por autenticar adequadamente tanto no App Configuration quanto no Key Vault. Os dois serviços não se comunicam diretamente.

    ```powershell (CreatePostmanEnvironmentFiles.ps1)
    # Por favor, trate isso como pseudocódigo e ajuste conforme necessário.
    ############################################################

    env = $arg1
    # 1. listar variáveis de ambiente do app para um ambiente
    envVars = az appconfig kv list --name PostmanAppConfig --label $env | ConvertFrom-Json
    # 2. percorrer o array envVars para obter URIs do Key Vault
    keyvaultURI = ""
    $envVars | % {if($_.key -eq 'password'){keyvaultURI = $_.value}} 
    # 3. analisar URIs para obter o nome do Key Vault e os nomes dos segredos
    # 4. obter segredo do Key Vault
    kvsecret = az keyvault secret show --name $secretName --vault-name $keyvaultName --query "value"
    # 5. definir o valor da senha para o segredo retornado do Key Vault
    $envVars | % {if($_.key -eq 'password'){$_.value=$kvsecret}}  
    # 6. criar arquivo de ambiente
    envFile = @{ "_postman_variable_scope" = "environment", "name" = $env, values = @() }
    foreach($var in $envVars){
            $envFile.values += @{ key = $var.key; value = $var.value; }
    }
    $envFile | ConvertTo-Json -depth 50 | Out-File -encoding ASCII -FilePath .\$env.postman_environment.json
    ```

6. Use o IDE do Postman para importar os arquivos de Ambiente do Postman a serem referenciados pela sua coleção.

Esta abordagem tem as seguintes vantagens:

- Herda todas as vantagens do caso anterior.
- Desencoraja o compartilhamento inseguro de segredos. Os segredos agora são retirados do Key Vault via Azure CLI. A URI do Key Vault também não precisa mais ser compartilhada para acesso aos tokens de autenticação.
- Fonte única de verdade para arquivos de Ambiente do Postman. Não há mais necessidade de compartilhá-los via repositório.
- O desenvolvedor só precisa gerenciar uma única Coleção do Postman.

Terminar com essa abordagem tem as seguintes desvantagens:

- Segredos podem acabar sendo expostos no histórico de commits do git se .gitIgnore não for atualizado para ignorar arquivos de Ambiente do Postman.
- As coleções só podem ser usadas localmente para acessar APIs (locais ou implantadas). Não é baseado em CI.

### Caso de Uso - Testes E2E com Integração Contínua e Newman

Um desenvolvedor ou analista de QA pode ter uma suíte de testes de API existente de Coleções do Postman locais que seguem as melhores práticas de segurança para desenvolvimento; no entanto, agora eles querem que os testes E2E sejam executados como parte de um pipeline de CI automatizado. Com o advento do Newman, você pode agora usar mais prontamente o Postman para criar uma suíte de testes de API executável no seu CI.

Os passos podem ser os seguintes:

1. Atualize sua Coleção do Postman para usar o recurso de Teste do Postman para criar afirmações de teste que cobrirão todas as solicitações salvas de ponta a ponta. Leia a documentação do Postman para orientação sobre como usar o recurso de Teste do Postman.
2. Use localmente o Newman para validar que os testes estão funcionando conforme o esperado.

    ```powershell
    newman run tests\e2e_Postman_collection.json -e qa.postman_environment.json
    ```

3. Construa um script que execute automaticamente as afirmações de teste do Postman via Newman e Azure CLI.
    > NOTA: Um Service Principal do Azure deve ser configurado para continuar usando o azure cli neste exemplo de pipeline de CI.

    ```powershell (RunPostmanE2eTests.ps1)
    # Por favor, trate isso como pseudocódigo e ajuste conforme necessário.
    ############################################################

    # 1. faça login no Azure usando um Service Principal
    az login --service-principal -u $APP_ID -p $AZURE_SECRET --tenant $AZURE_TENANT
    # 2. liste as variáveis de ambiente do app para um ambiente
    envVars = az appconfig kv list --name PostmanAppConfig --label $env | ConvertFrom-Json
    # 3. percorra o array envVars para obter URIs do Key Vault
    keyvaultURI = ""
    @envVars | % {if($_.key -eq 'password'){keyvaultURI = $_.value}} 
    # 4. analise URIs para obter o nome do Key Vault e os nomes dos segredos
    # 5. obtenha o segredo do Key Vault
    kvsecret = az keyvault secret show --name $secretName --vault-name $keyvaultName --query "value"
    # 6. defina o valor da senha para o segredo retornado do Key Vault
    $envVars | % {if($_.key -eq 'password'){$_.value=$kvsecret}}  
    # 7. crie o arquivo de ambiente
    envFile = @{ "_postman_variable_scope" = "environment", "name" = $env, values = @() }
    foreach($var in $envVars){
            $envFile.values += @{ key = $var.key; value = $var.value; }
    }
    $envFile | ConvertTo-Json -depth 50 | Out-File -encoding ASCII $env.postman_environment.json
    # 8. instale o Newman
    npm install --save-dev newman
    # 9. execute testes E2E automatizados via Newman
    node_modules\.bin\newman run tests\e2e_Postman_collection.json -e $env.postman_environment.json
    ```

4. Crie um arquivo yaml e defina um passo que executará seu script de teste. (ex. Um arquivo yaml direcionado para o Azure Devops que executa um script Powershell.)

    ```yaml
    # Por favor, trate isso como pseudocódigo e ajuste conforme necessário.
    ############################################################
    displayName: 'Executar testes E2E do Postman'
    inputs:
        targetType: 'filePath'
        filePath: RunPostmanE2eTests.ps1
    env:
        APP_ID: $(environment.appId) # credenciais para az cli
        AZURE_SECRET: $(environment.secret)
        AZURE_TENANT: $(environment.tenant)
    ```

Esta abordagem tem a seguinte vantagem:

- Os testes E2E agora podem ser executados automaticamente como parte de um pipeline de CI.

Terminar com essa abordagem tem a seguinte desvantagem:

- Os arquivos de Ambiente do Postman não estão mais sendo gerados em um ambiente local para testes manuais práticos. No entanto, isso pode ser resolvido gerenciando dois scripts.
