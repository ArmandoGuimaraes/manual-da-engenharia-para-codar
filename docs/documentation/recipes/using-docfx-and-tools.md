# Usando o DocFx e Ferramentas Complementares para Gerar um Site de Documentação

Se você deseja uma maneira fácil de ter um site com toda a sua documentação proveniente de arquivos Markdown e comentários do código, você pode usar o [DocFx](https://dotnet.github.io/docfx/). O site gerado pelo DocFx também inclui recursos de pesquisa rápida. Existem algumas lacunas na solução do DocFx, mas fornecemos ferramentas complementares que o ajudarão a preencher essas lacunas. Consulte também o post do blog [Fornecendo documentação de qualidade em seu projeto com o DocFx e Ferramentas Complementares](https://mtirion.medium.com/providing-quality-documentation-in-your-project-with-docfx-and-companion-tools-76aed42b1ddd) para obter mais informações sobre a solução.

## Pré-requisitos

Este documento é melhor seguido clonando o exemplo em <https://github.com/mtirionMSFT/DocFxQuickStart> primeiro. Copie o conteúdo da pasta QuickStart para a raiz do seu próprio repositório para começar em seu próprio ambiente.

## Início Rápido

> **Resumo:**
>
> Se você deseja iniciar rapidamente usando o Azure DevOps e o Azure App Service sem ler o quê e como, siga estas etapas:
>
> 1. **Azure DevOps:** Se você ainda não tem, crie um projeto no Azure DevOps e [crie uma Conexão de Serviço para o seu ambiente Azure](https://learn.microsoft.com/en-us/azure/devops/pipelines/library/connect-to-azure?view=azure-devops). Clone o repositório.
> 2. **Pasta QuickStart:** Copie o conteúdo da pasta QuickStart para o repositório. Você pode encontrá-lo em <https://github.com/mtirionMSFT/DocFxQuickStart>.
> 3. **Azure:** Crie um grupo de recursos no seu ambiente Azure onde os recursos do site de documentação devem ser criados.
> 4. **Criar recursos do Azure:** Preencha os valores padrão em *infrastructure/variables.tf* e execute os comandos da [Etapa 3 - Implante recursos do Azure a partir do seu computador local](deploy-docfx-azure-website.md#3-deploy-azure-resources-from-your-local-machine) para criar os Recursos Azure.
> 5. **Pipeline:** Preencha as variáveis em *.pipelines/documentation.yml*, faça o commit das alterações e envie o conteúdo do repositório para o seu branch (possivelmente através de um PR).
>    Agora você pode criar um pipeline no seu projeto Azure DevOps que usa o *.pipelines/documentation.yml* e executá-lo.
>

## Estrutura de Pastas de Documentos e Projetos

A maneira mais fácil de trabalhar é com um [repositório único](https://mtirion.medium.com/monorepo-for-beginners-45d5059ab40e) onde a documentação e o código coexistem. Se essa não for a situação no seu caso, mas você ainda deseja combinar vários repositórios em um único site de documentação, você terá que clonar todos os repositórios primeiro para poder combinar as informações. Nesta receita, assumiremos que um repositório único é usado.

Nas etapas a seguir, consideraremos a geração do site de documentação a partir desta estrutura de conteúdo:

```xaml
├── .pipelines             // Azure DevOps pipeline para geração e implantação automática
│
├── docs                     // todos os documentos
│   ├── .attachments  // todas as imagens e outros anexos usados pelos documentos
│
├── infrastructure       // scripts Terraform para criação do site Azure
│
├── src                        // todos os projetos
│   ├── build              // configurações de compilação
│          ├── dotnet     // configurações de compilação do .NET
│   ├── Directory.Build.props   // configurações do projeto para todos os projetos .NET nas subpastas
│   ├── [Pastas de Projeto]
│
├── x-cross
│   ├── toc.yml              // Definição de referência cruzada (opcional)
│
├── .markdownlint.json // Configurações do Markdownlinter
├── docfx.json               // Configuração do DocFx
├── index.md                 // Página inicial do site
├── toc.yml                    // Definição dos links de conteúdo do cabeçalho do site
├── web.config              // web.config para habilitar a pesquisa no site implantado
```

Usaremos a ferramenta `DocLinkChecker` para validar todos os links na documentação e os anexos órfãos. Essa é a razão pela qual todos os anexos estão na pasta `.attachments`.

No site gerado a partir da pasta QuickStart, você verá que as hierarquias de documentação e referências são combinadas na tabela de conteúdos à esquerda. Isso é alcançado pela definição e uso de **x-cross\toc.yml**. Se você não deseja que as hierarquias sejam combinadas, basta remover a pasta e o arquivo da sua estrutura e (re)gerar o site.

Um arquivo `.markdownlint.json` está incluído com o conteúdo abaixo. A configuração [MD013](https://github.com/markdownlint/markdownlint/blob/master/docs/RULES.md#md013---line-length) está definida como false para evitar a verificação do comprimento máximo da linha. Você pode modificar este arquivo de acordo com suas preferências para incluir ou excluir determinados testes.

```json
{
    "MD013": false
}
```

O conteúdo das pastas **.pipelines** e **infrastructure** é explicado na receita [Implantar o site de Documentação DocFx em um Site Azure automaticamente](deploy-docfx-azure-website.md).

## Documentação de referência do código-fonte

O Doc

Fx pode gerar documentação de referência a partir do código, com suporte melhor para C# e TypeScript no momento. Na pasta QuickStart, usamos apenas projetos C#. Para que o DocFx gere uma documentação de referência de qualidade, são necessários comentários de código do tipo "triple-slash". Consulte [Triple-slash (///) Code Comments Support](https://dotnet.github.io/docfx/spec/triple_slash_comments_spec.html). Para garantir isso, é uma boa ideia utilizar o [StyleCop](https://github.com/DotNetAnalyzers/StyleCopAnalyzers). Existem algumas etapas que facilitarão o início com isso.

Primeiro, você pode usar o arquivo **Directory.Build.props** na pasta **/src** em combinação com os arquivos na pasta **build/dotnet**. Com isso, você pode impor o uso do StyleCop em todos os arquivos de projeto do Visual Studio em suas subpastas com uma configuração de quais regras devem ser usadas ou ignoradas. Você pode personalizar isso de acordo com suas necessidades. Para obter mais informações, consulte [Personalize sua compilação](https://learn.microsoft.com/en-us/visualstudio/msbuild/customize-your-build?view=vs-2019) e [Use conjuntos de regras para agrupar regras de análise de código](https://learn.microsoft.com/en-us/visualstudio/code-quality/using-rule-sets-to-group-code-analysis-rules?view=vs-2019).

Para garantir que os desenvolvedores sejam obrigados a adicionar os comentários "triple-slash" gerando erros do compilador e para ter as configurações adequadas para a geração de arquivos XML de documentação, adicione as configurações **TreatWarningsAsErrors** e **GenerateDocumentationFile** em todos os arquivos *.csproj*. Você pode adicionar isso no primeiro conjunto de propriedades *PropertyGroup* da seguinte forma:

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
      ...
    <GenerateDocumentationFile>true</GenerateDocumentationFile>
    <TreatWarningsAsErrors>true</TreatWarningsAsErrors>
  </PropertyGroup>

    ...
</Project>
```

Agora você está pronto para gerar documentação a partir do seu código C#. Para obter mais informações sobre os idiomas suportados pelo DocFx e como configurá-lo, consulte [Introdução ao Suporte a Múltiplos Idiomas](https://dotnet.github.io/docfx/spec/metadata_format_spec.html#26-multiple-language-support).

> **NOTA:** Você também pode adicionar uma definição PropertyGroup com as duas configurações no arquivo *Directory.Build.props* para tê-las em todos os projetos. Mas, nesse caso, também serão herdados nos projetos de teste.

## 1. Instale o DocFx e o markdownlint-cli

Acesse o site do [DocFx](https://dotnet.github.io/docfx/) na seção de Download e baixe a versão mais recente do DocFx. Acesse a página do [markdownlint-cli no GitHub](https://github.com/igorshubovych/markdownlint-cli) para encontrar opções de download e instalação.

Você também pode usar ferramentas como o [Chocolatey](https://chocolatey.org/) para instalar:

```bash
choco install docfx
choco install markdownlint-cli
```

## 2. Configure o DocFx

A configuração para o DocFx é feita em um arquivo `docfx.json`. Armazene este arquivo na raiz do seu repositório.

> **NOTA:** Você pode armazenar o docfx.json em algum lugar na hierarquia, mas então você precisa fornecer o caminho do arquivo como um argumento para o comando docfx para que ele possa ser localizado.

Aqui está uma boa configuração para começar, onde a documentação está na pasta **/docs** e as fontes estão na pasta **/src**:

```json
{
    "metadata": [
    {
          "src": [
          {
              "files": [ "src/**.csproj" ],
              "exclude": [ "_site/**", "**/bin/**", "**/obj/**", "**/[Tt]ests/**" ]
          }
          ],
          "dest": "reference",
          "disableGitFeatures": false
       }
    ],
    "build": {
        "content": [
            { "files": [ "reference/**" ] },
            {
                "files": [ "**.md", "**/toc.yml" ],
                "exclude": [ "_site/**", "**/bin/**", "**/obj/**", "**/[Tt]ests/**" ]
            }
        ],
        "resource": [
            { "files": ["docs/.attachments/**"] },
            { "files": ["web.config"] }
        ],
        "template": [ "templates/cse" ],
        "globalMetadata": {
            "_appTitle": "Documentação CSE",
            "_enableSearch": true
        },
        "markdownEngineName": "markdig",
        "dest": "_site",
        "xrefService": ["https://xref.learn.microsoft.com/query?uid={uid}"]
    }
}
```

## 3. Configure alguns documentos básicos

Sugerimos começar com uma estrutura básica de documentação na pasta **/docs**. Na pasta QuickStart fornecida, temos uma configuração básica:

```xaml
├── docs
│   ├── .attachments                     // Todas as imagens e outros anexos usados pelos documentos
│
│   ├── architecture-decisions
│           └── .order
│           └── decision-log.md       // Exemplo de índice para todas as ADRs
│           └── README.md          // Página de destino das decisões de arquitetura
│
│   ├── getting-started
│           └── .order
│           └── README.md          // Este documento de exemplo. Substitua o conteúdo por algo significativo para o projeto.
│
│   ├── guidelines
│           └── .order
│           └── docs-guidelines.md  // Diretrizes gerais de documentação
│           └── README.md          // Página de destino das diretrizes
│
│   ├── templates                          // todos os modelos, como o modelo ADR e outros
│           └── .order
│           └── README.md          // Página de destino dos modelos
│
│   ├── working-agreements
│           └── .order
│           └── README.md          // Página de destino dos acordos de trabalho
│
│   ├── .order                                // Fornecer uma ordem fixa de arquivos e diretórios
│   ├── index.md                          // Página inicial do site de documentação
```

Você pode usar modelos como acordos de trabalho e outros do [ISE Playbook](https://github.com/microsoft

/code-with-engineering-playbook/).

Para ter uma página de destino adequada para o seu site de documentação, você pode usar um arquivo Markdown chamado INDEX.MD na raiz do seu repositório. O conteúdo pode ser algo assim:

```markdown
# Documentação CSE

Esta é a página de destino do site de Documentação CSE. Esta é a página para introduzir tudo no site.

Você pode adicionar links específicos que são importantes para fornecer acesso direto.

> Tente não duplicar os links no topo da página, a menos que realmente faça sentido.

Para começar com a configuração deste site, leia o documento de início rápido com o título [Usando o DocFx e Ferramentas Complementares](using-docfx-and-tools.md).

```

## 4. Compile as ferramentas complementares e execute-as

> **NOTA:** Para explicar cada etapa, passaremos pelas várias etapas nos próximos parágrafos. Na pasta QuickStart, um arquivo em lote chamado **GenerateDocWebsite.cmd** está incluído. Este script executará todas as etapas necessárias para compilar as ferramentas, executar as verificações, gerar a tabela de conteúdos e executar o DocFx para gerar o site.

Para verificar a formatação adequada do Markdown, usamos a ferramenta **markdownlint-cli**. O comando obtém sua configuração do arquivo `.markdownlint.json` na raiz do projeto. Para verificar todos os arquivos Markdown, basta executar este comando:

```shell
markdownlint **/*.md
```

Na pasta QuickStart, você deverá ter copiado as duas ferramentas complementares **TocDocFxCreation** e **DocLinkChecker**, conforme descrito na introdução deste artigo.

Você pode compilar as ferramentas a partir do Visual Studio, mas também pode executar `dotnet build` em ambas as pastas de ferramentas.

A ferramenta **DocLinkChecker** é usada para validar o que está na pasta de documentos. Ele valida os links entre documentos e anexos na pasta de documentos e verifica se não há anexos órfãos. Um exemplo de execução dessa ferramenta, incluindo a verificação de anexos:

```shell
DocLinkChecker.exe -d ./docs -a
```

A ferramenta **TocDocFxCreation** é necessária para gerar uma tabela de conteúdos para a sua documentação, para que os usuários possam navegar entre pastas e documentos. Se você compilou a ferramenta, use este comando para gerar um arquivo de tabela de conteúdos `toc.yml`. Para gerar uma tabela de conteúdos com o uso dos arquivos .order para determinar a sequência dos artigos e para gerar automaticamente documentos index.md se não houver documento padrão disponível em uma pasta, você pode usar este comando:

```shell
TocDocFxCreation.exe -d ./docs -sri
```

## 5. Execute o DocFx para gerar o site

Execute o comando `docfx` para gerar o site, por padrão na pasta **_site**.

> **DICA:** Se você deseja verificar o site em seu ambiente local, forneça a opção **--serve** para o comando *docfx* ou o script *GenerateDocWebsite*. Um pequeno servidor web é lançado que hospeda o seu site, que é acessível em localhost.

### Estilo do site

Se você começou com a pasta QuickStart, o site é gerado usando um tema personalizado com [design material](https://ovasquez.github.io/docfx-material/) e o logotipo da Microsoft. Você pode alterar isso de acordo com suas preferências. Para obter mais informações, consulte [Como criar um modelo personalizado | Site do DocFX (dotnet.github.io)](https://dotnet.github.io/docfx/tutorial/howto_create_custom_template.html).

## Implante em um Site Azure

Após concluir as etapas, você deve ter um site padrão gerado na pasta *_site*. No entanto, é claro que você deseja que isso seja acessível para todos. Portanto, o próximo passo é criar, por exemplo, um Site Azure e ter um processo para gerar e implantar automaticamente o conteúdo nesse site. Esse processo é descrito na receita [Implantar o site de Documentação DocFx em um Site Azure automaticamente](deploy-docfx-azure-website.md).

## Referências

* [DocFX - gerador de documentação estática](https://dotnet.github.io/docfx/index.html)
* [Implantar o site de Documentação DocFx em um Site Azure automaticamente](deploy-docfx-azure-website.md)
* [Fornecer documentação de qualidade em seu projeto com DocFx e Ferramentas Complementares](https://mtirion.medium.com/providing-quality-documentation-in-your-project-with-docfx-and-companion-tools-76aed42b1ddd)
* [Monorepo para Iniciantes](https://mtirion.medium.com/monorepo-for-beginners-45d5059ab40e)
