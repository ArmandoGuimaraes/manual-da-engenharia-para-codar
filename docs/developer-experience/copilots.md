# Copilots

Existem diversas ferramentas de IA que podem aprimorar a experiência do desenvolvedor. Este artigo discutirá as ferramentas disponíveis, bem como conselhos sobre quando pode ser apropriado usar essas ferramentas.

## GitHub Copilot

A versão atual do GitHub Copilot pode fornecer conclusão de código em muitos IDEs populares. Por exemplo, a [extensão para o VS Code](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot) que pode ser instalada no Marketplace do VS Code. Ela requer uma conta do GitHub para uso. Para obter mais informações sobre quais IDEs são suportados, quais idiomas são suportados, custos, recursos, etc., consulte as informações em [Copilot](https://github.com/features/copilot) e [Copilot for Business](https://resources.github.com/copilot-for-business/).

Alguns exemplos de casos de uso para o GitHub Copilot incluem:

- **Escrever Documentação**. Por exemplo, o parágrafo acima foi escrito usando o Copilot.

- **Escrever Testes Unitários**. Dado que a configuração e asserções muitas vezes são consistentes em testes unitários, o Copilot tende a ser muito preciso.

- **Desbloquear**. Muitas vezes é difícil começar a escrever quando se está olhando para uma página em branco. O Copilot pode preencher o espaço com algo que pode ou não ser exatamente o que você deseja fazer, mas pode ajudar a colocar você na mentalidade certa.

Se você quiser que o Copilot escreva algo útil para você, tente escrever um comentário que descreva o que seu código vai fazer - muitas vezes ele pode continuar a partir daí.

## GitHub Copilot Labs

O Copilot possui uma [extensão GitHub Copilot Labs](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-labs) que oferece recursos adicionais que ainda não estão prontos para o uso principal. Para o VS Code, você pode instalá-lo no Marketplace do VS Code. Esses recursos incluem:

- **Explicar**. O Copilot pode explicar o que o código está fazendo em linguagem natural.

- **Traduzir**. O Copilot pode traduzir código de um idioma para outro.

- **Pincéis**. Você pode selecionar código que o Copilot modificará inline com base em um "pincel" que você selecionar, por exemplo, para tornar o código mais legível, corrigir bugs, melhorar a depuração, documentar, etc.

- **Gerar Testes**. O Copilot pode gerar testes unitários para seu código. No entanto, atualmente isso está limitado ao JavaScript e TypeScript.

## GitHub Copilot X

A próxima versão do Copilot oferece várias novas possibilidades além da conclusão de código. Estas incluem:

- **Chat**. Em vez de apenas fornecer conclusão de código, o Copilot poderá ter uma conversa com você sobre o que você deseja fazer. Ele tem contexto sobre o código em que você está trabalhando e pode fornecer sugestões com base nesse contexto. Além de apenas escrever código, considere usar o chat para:

  - **Construir Índices SQL**. Dada uma consulta, o ChatGPT pode gerar um índice SQL que melhorará o desempenho da consulta.

  - **Escrever Expressões Regulares**. Essas são notoriamente difíceis de escrever, mas o ChatGPT pode gerá-las para você se você fornecer alguma entrada de exemplo e descrever o que deseja extrair.

  - **Melhorar e Validar**. Se você não tiver certeza das implicações de escrever código de uma determinada maneira, pode fazer perguntas a esse respeito. Por exemplo, você pode perguntar se há uma maneira de escrever o código que seja mais eficiente em termos de desempenho ou use menos memória. Uma vez que ele lhe der uma opinião, você pode pedir que ele forneça documentação que valide essa afirmação.

- **Explicar**. O Copilot pode explicar o que o código está fazendo em linguagem natural.

- **Escrever Código**. Dado um estímulo pelo desenvolvedor, ele pode escrever código que você pode implantar com um clique em arquivos existentes ou novos.

- **Depurar**. O Copilot pode analisar seu código e propor soluções para corrigir bugs.

Ele pode fazer a maioria do que o Labs pode fazer com "pincéis" como "tópicos", mas enquanto o Labs altera o código no seu arquivo, a funcionalidade de chat simplesmente mostra o que ele mudaria na janela. No entanto, também existe um "modo inline" para o GitHub Copilot Chat que permite fazer alterações no seu código inline, o que não tem essa mesma limitação.

## ChatGPT / Bing Chat

Para programação, ferramentas genéricas de chat de IA, como ChatGPT e Bing Chat, são menos úteis, mas ainda têm seu lugar. O GitHub Copilot responderá apenas a "perguntas sobre programação" e sua interpretação dessa regra pode ser um pouco restritiva. Alguns casos de uso para usar ChatGPT ou Bing Chat incluem:

- **Escrever Documentação**. O Copilot pode escrever documentação, mas usando o ChatGPT ou o Bing Chat, você pode expandir sua documentação para incluir informações comerciais, casos de uso, contexto adicional, etc.

- **Mudar de Perspectiva**. O ChatGPT pode assumir a identidade de uma persona ou até mesmo de um sistema e responder a perguntas a partir dessa perspectiva. Por exemplo, você pode pedir para explicar o que um determinado trecho de código faz da perspectiva de um usuário. Você pode pedir para o ChatGPT imaginar que é um administrador de banco de dados e perguntar como melhorar uma consulta específica.

Ao usar o Bing Chat, experimente com os modos, às vezes mudar para o Modo Criativo pode fornecer os resultados de que você precisa.

## Engenharia de Estímulo

As ferramentas de IA de chat são tão boas quanto os estímulos que você lhes dá. A qualidade e a adequação da saída podem variar muito dependendo do estímulo. Além disso, muitas dessas ferramentas limitam o número de estímulos que você pode enviar em um determinado período de tempo. Para saber mais sobre engenharia de estímulos, você pode revisar a documentação de código aberto [aqui](https://github.com/brex
