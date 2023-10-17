# Como Automatizar Verificações Simples

Se você deseja automatizar algumas verificações em seus documentos Markdown, existem várias ferramentas que você pode utilizar. Por exemplo:

- Análise de Código / Linting
  - [markdownlint](../../code-reviews/recipes/markdown.md#markdownlint) para verificar a sintaxe do Markdown e aplicar regras que tornem o texto mais legível.
  - [markdown-link-check](https://github.com/tcort/markdown-link-check) para extrair links de textos em Markdown e verificar se cada link está funcionando (200 OK) ou morto.
  - [proselint](../../code-reviews/recipes/markdown.md#proselint) para verificar jargão, erros de ortografia, redundância, linguagem corporativa e outras questões relacionadas à linguagem.
  - [write-good](../../code-reviews/recipes/markdown.md#write-good) para verificar a prosa em inglês.
  - [Imagem Docker para node-markdown-spellcheck](https://github.com/tmaier/docker-markdown-spellcheck), uma imagem Docker leve para verificar a ortografia de arquivos Markdown.
  - [Análise de código estático](../../continuous-integration/dev-sec-ops/static-code-analysis/static_code_analysis.md)

- [Extensões do VS Code](../../code-reviews/recipes/markdown.md#vs-code-extensions)
  - [Write Good Linter](../../code-reviews/recipes/markdown.md#write-good-linter) para obter conselhos gramaticais e de linguagem enquanto edita um documento.
  - [markdownlint](../../code-reviews/recipes/markdown.md#markdownlint-extension) para examinar documentos Markdown e obter avisos sobre violações de regras durante a edição.

- Automação
  - [pre-commit](https://pre-commit.com/) para usar scripts de hook do Git para identificar problemas simples antes de enviar nosso código ou documentação para revisão.
  - Verifique [Validação de Build](../../code-reviews/recipes/markdown.md#build-validation) para automatizar a verificação de PRs.
  - Verifique [Pipeline de CI para melhor documentação](../../continuous-integration/markdown-linting/README.md) para um exemplo de pipeline com `markdownlint`, `markdown-link-check` e `write-good`.

Exemplo de saída:

![verificações-de-documentos](./images/docs-checks.png)

## Sobre as regras de linting

A equipe precisa estar clara sobre quais regras de linting são necessárias e não devem ser substituídas por ferramentas ou comentários. A equipe deve chegar a um consenso sobre quando substituir as regras das ferramentas.
