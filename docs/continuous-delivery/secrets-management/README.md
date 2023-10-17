# Gerenciamento de Segredos

O Gerenciamento de Segredos se refere à maneira como protegemos configurações e outros dados sensíveis que, se tornados públicos, permitiriam o acesso não autorizado a recursos. Exemplos de segredos incluem nomes de usuário, senhas, chaves de API, tokens SAS, etc.

Devemos assumir que qualquer repositório em que trabalhamos pode se tornar público a qualquer momento e proteger nossos segredos, mesmo que o repositório seja inicialmente privado.

## Abordagem Geral

A abordagem geral é manter segredos em arquivos de configuração separados que não são incluídos no repositório. Adicione os arquivos ao [.gitignore](https://git-scm.com/docs/gitignore) para evitar que eles sejam incluídos.

Cada desenvolvedor mantém sua própria versão local do arquivo ou, se necessário, os distribui por canais privados, como um chat no Teams.

Em um sistema de produção, assumindo o uso do Azure, crie os segredos no ambiente em que o processo está em execução. Isso pode ser feito editando manualmente a seção 'Configurações de Aplicativos' do recurso, mas um script usando o Azure CLI para fazer o mesmo é uma ferramenta útil que economiza tempo. Consulte [az webapp config appsettings](https://learn.microsoft.com/en-us/cli/azure/webapp/config/appsettings?view=azure-cli-latest) para obter mais detalhes.

É uma boa prática manter configurações de segredos separadas para cada ambiente que você utiliza, como desenvolvimento, teste, produção, local, etc.

A [receita de segredos por ramificação](./../azure-devops/secret-management-per-branch.md) descreve uma maneira simples de gerenciar configurações de segredos separadas para cada ambiente.

> Observação: mesmo que o segredo tenha sido apenas enviado para uma ramificação de recurso e nunca mesclado, ele ainda faz parte do histórico do git. Siga [essas instruções](https://help.github.com/en/github/authenticating-to-github/removing-sensitive-data-from-a-repository) para remover quaisquer dados sensíveis e/ou regenerar quaisquer chaves e outras informações sensíveis adicionadas ao repositório. Se uma chave ou segredo foi incluído no código-base, faça a rotação da chave/segredo para que ele não esteja mais ativo.

## Mantendo os Segredos em Sigilo

O cuidado em proteger nossos segredos se aplica tanto à forma como os obtemos e armazenamos quanto à forma como os utilizamos.

- **Não registre segredos**
- Não os inclua em relatórios
- Não os envie para outras aplicações, como parte de URLs, formulários ou de qualquer outra forma, exceto para fazer uma solicitação ao serviço que requer esse segredo

## Aplicações com Segurança Reforçada

As técnicas descritas abaixo fornecem *boa* segurança e um padrão comum para uma ampla gama de linguagens. Elas dependem do fato de que o Azure mantém as configurações de aplicativos (o ambiente) criptografadas até que seu aplicativo seja executado.

Elas *não* impedem que os segredos existam em texto simples na memória em tempo de execução. Em particular, para linguagens com coleta de lixo, esses valores podem existir por mais tempo do que a vida útil da variável e podem ser visíveis durante a depuração de um despejo de memória do processo.

> Se você estiver trabalhando em um aplicativo com requisitos de segurança mais rigorosos, deve considerar o uso de técnicas adicionais para manter a criptografia dos segredos durante toda a vida útil do aplicativo.

Sempre faça a rotação das chaves de criptografia regularmente.

## Técnicas para o Gerenciamento de Segredos

Essas técnicas tornam o carregamento de segredos transparente para o desenvolvedor.

### C#/.NET

#### Solução Moderna .NET

Para o SDK .NET (versão 2.0 ou superior), temos o `dotnet secrets`, uma ferramenta fornecida pelo SDK .NET que permite gerenciar e proteger informações sensíveis, como chaves de API, strings de conexão e outros segredos, durante o desenvolvimento. Os segredos são armazenados com segurança em sua máquina e podem ser acessados por suas aplicações .NET.

```shell
# Inicialize o dotnet secret 
dotnet user-secrets init

# Adicione um segredo
# dotnet user-secrets set <CHAVE> <VALOR>
dotnet user-secrets set ExternalServiceApiKey minha-api-key-12345

# Atualize o segredo
dotnet user-secrets set ExternalServiceApiKey api-key-atualizada-67890

```

Para acessar os segredos:

```csharp
using Microsoft.Extensions.Configuration;

var builder = new ConfigurationBuilder()
    .AddUserSecrets<Startup>();

var configuration = builder.Build();
var externalServiceApiKey = configuration["ExternalServiceApiKey"];

```

##### Considerações de Implantação

Ao implantar sua aplicação em produção, é essencial garantir que seus segredos sejam gerenciados com segurança. Aqui estão algumas implicações relacionadas à implantação:

- Remova Segredos de Desenvolvimento: Antes de implantar em produção, remova quaisquer segredos de desenvolvimento da configuração de sua aplicação. Você pode usar variáveis de ambiente ou uma solução de gerenciamento de segredos mais segura, como o Azure Key Vault ou o AWS Secrets Manager em produção.

- Implantação Segura: Garanta que seu servidor de produção seja seguro e que o acesso aos segredos seja controlado. Nunca armazene segredos diretamente no código-fonte ou em arquivos de configuração.

- Rotação de Chaves: Considere implementar uma política de rotação de segredos para atualizar regularmente seus segredos em produção.

#### Solução .NET Framework

Use o atributo [`file`](https://learn.microsoft.com/en-us/dotnet/framework/configure-apps/file-schema/appsettings/appsettings-element-for-configuration) do elemento appSettings para carregar segredos de um arquivo local.

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <appSettings file="..\..\secrets.config">
  …
  </appSettings>
  <startup>
      <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.6.1" />
  </startup>
  …
</configuration>
```

Acesso

 aos segredos:

```C#
static void Main(string[] args)
{
    String mySecret = System.Configuration.ConfigurationManager.AppSettings["mySecret"];
}
```

Quando em execução no Azure, o ConfigurationManager carregará essas configurações do ambiente do processo. Não é necessário fazer upload de arquivos de segredos para o servidor nem alterar o código.

### Node

Armazene segredos em variáveis de ambiente ou em um arquivo `.env`

```bash
$ cat .env
MY_SECRET=mySecret
```

Use o pacote [dotenv](https://www.npmjs.com/package/dotenv) para carregar e acessar variáveis de ambiente

```node
require('dotenv').config()
let mySecret = process.env.MY_SECRET
```

### Python

Armazene segredos em variáveis de ambiente ou em um arquivo `.env`

```bash
$ cat .env
MY_SECRET=mySecret
```

Use o pacote [dotenv](https://pypi.org/project/python-dotenv/) para carregar e acessar variáveis de ambiente

```Python
import os
from dotenv import load_dotenv

load_dotenv()
my_secret = os.getenv('MY_SECRET')
```

Outra boa biblioteca para ler variáveis de ambiente é `environs`

```Python
from environs import Env

env = Env()
env.read_env()
my_secret = os.environ["MY_SECRET"]
```

### Databricks

O Databricks oferece a opção de usar o `dbutils` como uma maneira segura de recuperar credenciais e não revelá-las nos notebooks em execução no Databricks

Os seguintes passos descrevem claramente como criar novos segredos e utilizá-los em um notebook no Databricks:

1. [Instale e configure o Databricks CLI](https://docs.databricks.com/user-guide/dev-tools/databricks-cli.html#set-up-the-cli) em sua máquina local
2. [Obtenha o token de acesso pessoal do Databricks](https://docs.databricks.com/api/latest/authentication.html#token-management)
3. [Crie um escopo para os segredos](https://learn.microsoft.com/azure/databricks/security/secrets/secret-scopes)
4. [Crie segredos](https://learn.microsoft.com/azure/databricks/security/secrets/)

### Validação

A verificação automatizada de credenciais pode ser realizada no código, independentemente da linguagem de programação utilizada. Saiba mais sobre isso [aqui](../../continuous-integration/dev-sec-ops/secret-management/credential_scanning.md)
