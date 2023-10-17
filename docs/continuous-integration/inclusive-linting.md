# Inclusão de Linting

Como profissionais de software, devemos nos esforçar para promover um ambiente de trabalho inclusivo, o que naturalmente se estende ao código e à documentação que escrevemos. É importante manter o uso de linguagem inclusiva consistente em todo o projeto ou repositório.

Para alcançar isso, recomendamos o uso de uma ferramenta de análise de texto, como um linter inclusivo, e incluí-lo como uma etapa em seus pipelines de Integração Contínua (CI).

### O que verificar com o Linter

O objetivo principal de um inclusive linter é identificar qualquer ocorrência de linguagem não inclusiva no código-fonte (e opcionalmente sugerir algumas alternativas). Palavras ou frases não inclusivas em um projeto podem ser encontradas em qualquer lugar, desde comentários e documentação até nomes de variáveis.

Um inclusive linter pode incluir seu próprio dicionário de palavras e frases não inclusivas "padrão" para verificar como um bom ponto de partida. Essas ferramentas também podem ser personalizáveis, oferecendo frequentemente a capacidade de omitir alguns termos e/ou adicionar os seus próprios.

A capacidade de adicionar termos adicionais ao seu linter tem o benefício adicional de permitir a verificação de linguagem sensível, além da verificação inclusiva. Isso pode evitar que coisas como nomes de clientes ou outras informações não públicas entrem em seu histórico do Git, por exemplo.

### Como Começar com um Inclusive Linter

**[`woke`](https://github.com/get-woke/woke)**

Um inclusive linter que recomendamos é o **`woke`**. É uma ferramenta CLI agnóstica a linguagens que detecta linguagem não inclusiva em seu código-fonte e sugere alternativas. Embora o **`woke`** aplique automaticamente um [conjunto de regras padrão](https://github.com/get-woke/woke/blob/main/pkg/rule/default.yaml) com termos não inclusivos para verificar, você também pode aplicar uma configuração de regra personalizada (por meio de um arquivo YAML) com termos adicionais para verificar. Veja [`example.yaml`](https://github.com/get-woke/woke/blob/main/example.yaml) para um exemplo de adição de regras personalizadas.

Executar a ferramenta localmente em um arquivo ou diretório é relativamente simples:

```sh
$ woke test.txt

test.txt:2:2-6: `guys` pode ser insensível, use `pessoas`, `indivíduos` em vez disso (aviso)
* guys
  ^
```

**`woke`** pode ser executado localmente em sua máquina ou sistema CI/CD por meio da CLI e também está disponível como duas ações no GitHub:

- [Executar woke](https://github.com/marketplace/actions/run-woke)
- [Executar woke com o Reviewdog](https://github.com/marketplace/actions/run-woke-with-reviewdog)

Para usar a ação padrão "Run woke" do GitHub com o conjunto de regras padrão em um pipeline de CI:

1. Adicione a ação **`woke`** como uma etapa no arquivo YAML do pipeline de CI do seu projeto:

    ```yaml
    name: ci
    on:
      - pull_request
    jobs:
      woke:
        name: woke
        runs-on: ubuntu-latest
        steps:
          - name: Checkout
            uses: actions/checkout@v2

          - name: woke
            uses: get-woke/woke-action@v0
            with:
              # Faz com que a verificação falhe em qualquer regra quebrada
              fail-on-error: true
    ```

2. Execute seu pipeline
3. Veja a saída na guia "Ações" na visualização principal do repositório

Para obter mais informações sobre configuração adicional e uso, consulte a [documentação oficial](https://docs.getwoke.tech/).

[`woke`]: https://github.com/get-woke/woke
[default ruleset]: https://github.com/get-woke/woke/blob/main/pkg/rule/default.yaml
[`example.yaml`]: https://github.com/get-woke/woke/blob/main/example.yaml
[Run woke]: https://github.com/marketplace/actions/run-woke
[Run woke with reviewdog]: https://github.com/marketplace/actions/run-woke-with-reviewdog
[docs]: https://docs.getwoke.tech/
