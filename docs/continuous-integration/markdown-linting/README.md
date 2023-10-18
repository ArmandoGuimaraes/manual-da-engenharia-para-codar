# Pipeline de CI para Melhor Documentação

## Introdução

A maioria dos projetos começa com experimentações, onde desenvolvedores e analistas produzem muita documentação.

Às vezes, esses documentos não seguem um padrão e cada membro da equipe os escreve de acordo com suas preferências. Acrescente a isso
o tempo que um revisor gasta verificando gramática, procurando por erros ortográficos ou linguagem não inclusiva.

Este pipeline ajuda a resolver esse problema!

## O Pipeline

O pipeline utiliza os seguintes módulos `npm`:

- [markdownlint](https://github.com/DavidAnson/markdownlint): adiciona padronização usando [regras](https://github.com/DavidAnson/markdownlint#rules--aliases)
- [markdown-link-check](https://github.com/tcort/markdown-link-check): verifica os links na documentação e relata os quebrados
- [write-good](https://github.com/btford/write-good): linter para prosa em inglês

Utilizamos este pipeline por mais de um ano em diferentes projetos e sempre recebemos ótimos feedbacks dos clientes!

## Como Funciona

Para começar a usar este pipeline:

1. Baixe os arquivos deste [repositório](https://github.com/squassina/doc-pipeline/tree/main/repo-root)
1. Descompacte as pastas e arquivos na raiz do seu repositório, se o repositório estiver vazio
    - se não for totalmente novo, copie os arquivos e faça os ajustes necessários:
        - verifique o `.azdo` para que ele corresponda ao padrão do seu repositório
        - verifique o `package.json` para não sobrescrever um que você já tenha no processo. Atualize o arquivo se você alterou
          o nome da pasta `.azdo`.
1. Crie o pipeline no Azure DevOps ou GitHub

## Referências

[Revisões de Código em Markdown no Code With Engineering Playbook](https://microsoft.github.io/code-with-engineering-playbook/code-reviews/recipes/markdown/#code-review-checklist)
