# Revisões de Código em Go

## Guia de Estilo

Os desenvolvedores devem seguir o [Guia de Estilo Effective Go](https://golang.org/doc/effective_go.html).

## Análise de Código / Verificação de Estilo

### Configuração do Projeto

Abaixo está a configuração do projeto que você gostaria de ter em seu Visual Studio Code.

#### Extensão vscode-go

Usando a extensão Go para o Visual Studio Code, você obtém recursos de linguagem como IntelliSense, navegação de código, pesquisa de símbolos, correspondência de colchetes, snippets, etc. Esta extensão inclui suporte de linguagem avançado para Go no VS Code.

#### go vet

`go vet` é uma ferramenta de análise estática que verifica erros comuns em Go, como uso incorreto de variáveis de loop range ou argumentos printf desalinhados. O código Go deve ser capaz de ser compilado sem erros do `go vet`. Isso fará parte da extensão vscode-go.

#### golint

**:exclamation: AVISO: A biblioteca golint está descontinuada e arquivada.**

O linter revive (abaixo) pode ser uma substituição adequada.

[golint](https://github.com/golang/lint) pode ser uma ferramenta eficaz para encontrar muitos problemas, mas pode gerar falsos positivos. É melhor usado por desenvolvedores ao trabalhar no código, não como parte de um processo de build automatizado. Este é o linter padrão que está configurado como parte da extensão vscode-go.

#### revive

[Revive](https://revive.run/) é um linter para Go, que fornece um framework para o desenvolvimento de regras personalizadas e permite que você defina um conjunto rígido para melhorar seus processos de desenvolvimento e revisão de código.

## Formatação Automática de Código

### gofmt

`gofmt` é o guia de estilo de formatação de código automatizado para Go. Isso faz parte da extensão vs-code e é ativado por padrão para rodar sempre que um arquivo é salvo.

## Agregador

### golangci-lint

[golangci-lint](https://github.com/golangci/golangci-lint/) é a substituição do agora descontinuado `gometalinter`. Ele é de 2 a 7 vezes mais rápido que `gometalinter` [juntamente com uma série de outros benefícios](https://github.com/golangci/golangci-lint/#comparison).

golangci-lint é um agregador poderoso e personalizável de linters. Por padrão, vários estão habilitados, mas nem todos. Uma lista completa de linters e seus usos pode ser encontrada [aqui](https://github.com/golangci/awesome-go-linters).

Ele permitirá que você configure cada linter e escolha quais deseja habilitar em seu projeto.

Uma característica incrível do `golangci-lint` é que ele pode ser facilmente introduzido em um código legado existente usando `--new-from-rev COMMITID`. Com essa configuração, apenas novos problemas são sinalizados, permitindo que uma equipe melhore o novo código sem precisar corrigir todos os problemas históricos em um código legado grande. O golangci-lint também pode ser configurado como o linter padrão no VS Code.

Opções de instalação para golangci-lint estão presentes em [golangci-lint](https://github.com/golangci/golangci-lint#binary).

Para usar golangci-lint com o VS Code, use as configurações recomendadas abaixo:

```json
"go.lintTool":"golangci-lint",
   "go.lintFlags": [
     "--fast"
   ]
```

## Hooks Pré-Commit

Todos os desenvolvedores devem executar `gofmt` em um hook pré-commit para garantir a formatação padrão.

### Passo 1 - Instalar pre-commit

Execute `pip install pre-commit` para instalar o pre-commit.
Alternativamente, você pode executar `brew install pre-commit` se estiver usando o Homebrew.

### Passo 2 - Adicionar go-fmt no pré-commit

Adicione o arquivo .pre-commit-config.yaml à raiz do projeto Go. Execute go-fmt no pré-commit adicionando-o ao arquivo .pre-commit-config.yaml como abaixo.

```yaml
- repo: git://github.com/dnephin/pre-commit-golang
  rev: master
  hooks:
    - id: go-fmt
```

### Passo 3

Execute `$ pre-commit install` para configurar os scripts de hook do Git.

## Validação de Build

`gofmt` deve ser executado como parte de cada build para impor o padrão comum.

Para automatizar esse processo no Azure DevOps, você pode adicionar o trecho a seguir ao seu arquivo `azure-pipelines.yaml`. Isso formatará os scripts na pasta `./scripts/`.

```yaml
- script: go fmt
  workingDirectory: $(System.DefaultWorkingDirectory)/scripts
  displayName: "Executar formatação de código"
```

`govet` deve ser executado como parte de cada build para verificar a verificação de código.

Para automatizar esse processo no Azure DevOps, você pode adicionar o trecho a seguir ao seu arquivo `azure-pipelines.yaml`. Isso verificará a verificação de linting de qualquer script na pasta `./scripts/`.

```yaml
- script: go vet
  workingDirectory: $(System.DefaultWorkingDirectory)/scripts
  displayName: "Executar verificação de código"
```

Alternativamente, você pode usar o golangci-lint como uma etapa no pipeline para realizar várias validações habilitadas (incluindo go vet e go fmt) do golangci-lint.

```yaml
- script: golangci-lint run --enable gofmt --fix
  workingDirectory: $(System.DefaultWorkingDirectory)/scripts
  displayName: "Executar verificação de código"
```

## Exemplo de Pipeline de Validação de Build no Azure DevOps

```yaml
trigger: master

pool:
   vmImage: 'ubuntu-latest'

steps:

- task: GoTool@0
  inputs:
    version: '1.13.5'

- task: Go@0
  inputs:
    command: 'get'
    arguments: '-d'
    workingDirectory: '$(System.DefaultWorkingDirectory)/scripts'


- script: go fmt
  workingDirectory: $(System.DefaultWorkingDirectory)/scripts
  displayName: "Executar formatação de código"

- script: go vet
  workingDirectory: $(System.DefaultWorkingDirectory)/scripts
  displayName: 'Executar go vet'

- task: Go@0
  inputs:
    command: 'build'
    workingDirectory: '$(System.DefaultWorking

Directory)'

- task: CopyFiles@2
  inputs:
    TargetFolder: '$(Build.ArtifactStagingDirectory)'
- task: PublishBuildArtifacts@1
  inputs:
     artifactName: drop
```

## Checklist de Revisão de Código

A equipe de linguagem Go mantém uma lista de [Comentários de Revisão de Código Comuns](https://github.com/golang/go/wiki/CodeReviewComments) para Go que formam a base para um sólido checklist para uma equipe que trabalha em Go, que deve ser seguido além do [Checklist de Revisão de Código ISE](../process-guidance/reviewer-guidance.md)

* [ ] Este código [trata erros](https://golang.org/doc/effective_go.html#errors) corretamente? Isso inclui não descartar erros com atribuições `_` e retornar erros, em vez de [valores de erro no próprio canal](https://github.com/golang/go/wiki/CodeReviewComments#in-band-errors)?
* [ ] Este código segue os padrões Go para tipos de receptor de método [receiver types](https://github.com/golang/go/wiki/CodeReviewComments#receiver-type)?
* [ ] Este código [passa valores](https://github.com/golang/go/wiki/CodeReviewComments#pass-values) quando necessário?
* [ ] As interfaces neste código são definidas nos [pacotes corretos](https://github.com/golang/go/wiki/CodeReviewComments#interfaces)?
* [ ] As go-routines neste código têm [lifetimes claros](https://github.com/golang/go/wiki/CodeReviewComments#goroutine-lifetimes)?
* [ ] O paralelismo neste código é tratado por meio de go-routines e canais com [métodos síncronos](https://github.com/golang/go/wiki/CodeReviewComments#synchronous-functions)?
* [ ] Este código possui [Comentários de Documentação](https://github.com/golang/go/wiki/CodeReviewComments#doc-comments) significativos?
* [ ] Este código possui [Comentários de Pacote](https://github.com/golang/go/wiki/CodeReviewComments#package-comments) significativos?
* [ ] Este código usa [Contextos](https://github.com/golang/go/wiki/CodeReviewComments#contexts) corretamente?
* [ ] Os testes unitários falham com [mensagens significativas](https://github.com/golang/go/wiki/CodeReviewComments#useful-test-failures)?
