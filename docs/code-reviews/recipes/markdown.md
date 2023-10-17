# Revisões de Código em Markdown

## Guia de Estilo

Os desenvolvedores devem tratar a documentação como qualquer outro código fonte e seguir as mesmas regras e listas de verificação ao revisar a documentação como código.

A documentação deve utilizar uma boa sintaxe Markdown para garantir que seja interpretada corretamente e seguir boas [diretrizes de estilo de escrita](#diretrizes-de-estilo-de-escrita) para garantir que o documento seja fácil de ler e entender.

## Markdown

Markdown é uma linguagem de marcação leve que você pode usar para adicionar elementos de formatação a documentos de texto simples. Criado por John Gruber em 2004, o Markdown agora é uma das linguagens de marcação mais populares do mundo.

Usar o Markdown é diferente de usar um editor WYSIWYG. Em um aplicativo como o Microsoft Word, você clica em botões para formatar palavras e frases, e as alterações são visíveis imediatamente. O Markdown não é assim. Quando você cria um arquivo formatado em Markdown, você adiciona a sintaxe do Markdown ao texto para indicar quais palavras e frases devem parecer diferentes.

Você pode encontrar mais informações e documentação completa [aqui](https://www.markdownguide.org/).

## Linters

O Markdown tem uma maneira específica de ser formatado. É importante respeitar essa formatação, caso contrário, alguns interpretadores que são rigorosos podem não exibir corretamente o documento. Os linters são frequentemente usados para ajudar os desenvolvedores a criar documentos adequadamente, verificando tanto a sintaxe correta do Markdown quanto a gramática e o uso adequado do idioma inglês.

Uma boa configuração inclui um linter de Markdown usado durante a edição e verificação de build de PR, e um linter de gramática usado durante a edição do documento. A seguir, estão listados alguns linters que podem ser usados nessa configuração.

### markdownlint

[`markdownlint`](https://github.com/markdownlint/markdownlint) é um linter para Markdown que verifica a sintaxe do Markdown e também impõe regras que tornam o texto mais legível. [Markdownlint-cli](https://github.com/igorshubovych/markdownlint-cli) é uma CLI fácil de usar baseada no Markdownlint.

Está disponível como um [gem Ruby](https://github.com/markdownlint/markdownlint), um [pacote npm](https://github.com/DavidAnson/markdownlint), uma [CLI Node.js](https://github.com/igorshubovych/markdownlint-cli) e uma [extensão do VS Code](https://github.com/DavidAnson/vscode-markdownlint). A extensão do VS Code [Prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode) também captura todos os erros do markdownlint.

Instalando a CLI Node.js

```bash
npm install -g markdownlint-cli
```

Executando o markdownlint em um projeto Node.js

```bash
markdownlint **/*.md --ignore node_modules
```

Corrigindo erros automaticamente

```bash
markdownlint **/*.md --ignore node_modules --fix
```

Uma lista completa de regras do markdownlint está disponível [aqui](https://github.com/DavidAnson/markdownlint/blob/main/doc/Rules.md).

### proselint

[`proselint`](http://proselint.com/) é um utilitário de linha de comando que verifica o conteúdo de texto do documento. Ele verifica jargões, erros de ortografia, redundância, linguagem corporativa e outros problemas relacionados ao idioma.

Está disponível como um [pacote Python](https://github.com/amperser/proselint/#checks) e um [pacote Node](https://www.npmjs.com/package/proselint).

```bash
pip install proselint
npm install -g proselint
```

Execute o proselint

```bash
proselint document.md
```

### write-good

[`write-good`](https://github.com/btford/write-good) é um linter para texto em inglês que ajuda a escrever uma documentação melhor.

```bash
npm install -g write-good
```

Execute o write-good

```bash
write-good *.md
```

Execute o write-good sem instalá-lo

```bash
npx write-good *.md
```

O Write Good também está disponível como uma [extensão para o VS Code](https://marketplace.visualstudio.com/items?itemName=travisthetechie.write-good-linter)

## Extensões do VS Code

### Extensão Write Good Linter

A [`Extensão Write Good Linter`](https://marketplace.visualstudio.com/items?itemName=travisthetechie.write-good-linter) integra-se ao VS Code para fornecer dicas de gramática e linguagem durante a edição do documento.

### Extensão markdownlint

A [`Extensão markdownlint`](https://marketplace.visualstudio.com/items?itemName=DavidAnson.vscode-markdownlint) examina os documentos Markdown, exibindo avisos para violações de regras durante a edição.

## Validação de Build

### Lintagem

Para automatizar a lintagem com o `markdownlint` para validação de PR em ações do GitHub,
você pode usar um agregador de linters, como fazemos com o [MegaLinter neste repositório](https://github.com/microsoft/code-with-engineering-playbook/blob/main/.github/workflows/mega-linter.yml), ou usar o seguinte YAML.

```yaml
name: Markdownlint

on:
  push:
    paths:
      - "**/*.md"
  pull_request:
    paths:
      - "**/*.md"

jobs:
  lint:

    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v2
    - name: Use o Node.js
      uses: actions/setup-node@v1
      with:
        node-version: 12.x
    - name: Execute o Markdownlint
      run: |
        npm i -g markdownlint-cli
        markdownlint "**/*.md" --ignore node_modules
```

### Verificação de Links

Para automatizar a verificação de links em seus arquivos Markdown, adicione a ação `markdown-link-check` ao seu pipeline de validação:

```yaml
  markdown-link-check:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@master
    - uses: gaurav-nelson/github-action-markdown-link-check@v1
```

Mais informações sobre as opções da ação `markdown-link-check` podem ser encontradas na [página principal do `markdown-link-check`](https://github.com/gaurav-nelson/github-action-markdown-link-check).

## Checklist de Revisão de Código

Além do [Checklist de

 Revisão de Código](../process-guidance/reviewer-guidance.md), você também deve procurar por itens específicos de revisão de código de documentação:

- [ ] O documento é fácil de ler e entender e segue as [boas diretrizes de escrita](#diretrizes-de-estilo-de-escrita)?
- [ ] Existe uma única fonte de verdade ou o conteúdo é repetido em mais de um documento?
- [ ] A documentação está atualizada com o código?
- [ ] A documentação é tecnicamente e eticamente correta?

## Diretrizes de Estilo de Escrita

A seguir, alguns exemplos de diretrizes de estilo de escrita.

Concordem em sua equipe quais diretrizes vocês devem aplicar à documentação do projeto.
Salve suas diretrizes juntamente com sua documentação, para que seja fácil consultá-las.

### Redação

- Use linguagem inclusiva e evite jargões e palavras incomuns. A documentação deve ser fácil de entender.
- Seja claro e conciso, siga o objetivo do documento.
- Use voz ativa.
- Verifique a ortografia e a gramática do texto.
- Siga sempre a ordem cronológica.
- Visite [Plain English](https://plainenglish.co.uk/how-to-write-in-plain-english.html) para dicas sobre como escrever documentação fácil de entender.

### Organização do Documento

- Organize os documentos por tópico em vez de tipo, isso facilita a localização da documentação.
- Cada pasta deve ter um README.md de nível superior e qualquer outro documento dentro dessa pasta deve ter links diretos ou indiretos a partir desse README.md.
- Nomes de documentos com mais de uma palavra devem usar sublinhados em vez de espaços, por exemplo, `machine_learning_pipeline_design.md`. O mesmo se aplica às imagens.

### Cabeçalhos

- Comece com um H1 (um único # em Markdown) e respeite a ordem H1 > H2 > H3 etc.
- Siga cada cabeçalho com texto antes de prosseguir para o próximo cabeçalho.
- Evite colocar números nos cabeçalhos. Números mudam e podem criar títulos desatualizados.
- Evite usar símbolos e caracteres especiais em cabeçalhos, isso causa problemas com links de âncora.
- Evite links em cabeçalhos.

### Links

- Evite duplicação de conteúdo, em vez disso, faça um link para a `única fonte de verdade`.
- Faça link, mas não resuma. Resumir o conteúdo em outra página faz com que o conteúdo exista em dois lugares.
- Use textos de âncora significativos, por exemplo, em vez de escrever `Siga as instruções [aqui](../recipes/markdown.md)`, escreva `Siga as [diretrizes do Markdown](../recipes/markdown.md)`.
- Certifique-se de que os links para a documentação da Microsoft não contenham o marcador de idioma `/en-us/` ou `/fr-fr/`, pois isso é determinado automaticamente pelo próprio site.

### Listas

- Itens de lista devem começar com letras maiúsculas, se possível.
- Use listas ordenadas quando os itens descrevem uma sequência a seguir, caso contrário, use listas não ordenadas.
- Para listas ordenadas, prefixe cada item com `1.`. Quando renderizado, os itens da lista aparecerão com numeração sequencial. Isso evita lacunas de números na lista.
- Não adicione vírgulas `,` ou ponto e vírgulas `;` no final dos itens da lista e evite pontos `.` a menos que o item da lista represente uma frase completa.

### Imagens

- Coloque as imagens em um diretório separado chamado `img`.
- Nomeie as imagens apropriadamente, evitando nomes genéricos como `screenshot.png`.
- Evite adicionar imagens ou vídeos grandes ao controle de origem, faça um link para um local externo.

### Ênfase e seções especiais

- Use **negrito** ou _itálico_ para enfatizar.
  > Para seções que todos que leem este documento precisam estar cientes, use blocos.
- Use `crases` para código, uma crase simples para código inline como `pip install flake8` e 3 crases para blocos de código seguidos pelo idioma para destacar a sintaxe.

  ```python
  def add(num1: int, num2: int):
    return num1 + num2
  ```

- Use caixas de seleção para listas de tarefas.
  - [ ] Item 1
  - [ ] Item 2
  - [x] Item 3
- Adicione uma seção de Referências ao final do documento com links para referências externas.
- Prefira tabelas a listas para comparações e relatórios para tornar a pesquisa e os resultados mais legíveis.

  | Opção    | Prós      | Contras   |
  |----------|-----------|-----------|
  | Opção 1  | Alguns prós | Alguns contras |
  | Opção 2  | Alguns prós | Alguns contras |

### Geral

- Sempre use a sintaxe Markdown, não misture com HTML.
- Certifique-se de que a extensão dos arquivos seja `.md`. Se a extensão estiver faltando, um linter pode ignorar os arquivos.
