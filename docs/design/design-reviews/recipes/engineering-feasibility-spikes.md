# Engenharia de Investigação de Viabilidade: Identificação e Mitigação de Riscos

## Introdução

Alguns compromissos requerem mais redução de riscos do que outros. Mesmo após Sessões de Design Arquitetônico (ADS), um compromisso pode ainda ter incertezas técnicas substanciais. Esses tipos de compromissos justificam uma fase exploratória/validação em que as Investigações de Viabilidade de Engenharia podem ser conduzidas imediatamente após a visão/ADS e antes dos sprints de engenharia.

### Investigações de Viabilidade de Engenharia

- São atividades investigativas com limite de tempo regulamentadas, mas colaborativas, conduzidas em um ciclo de feedback para aproveitar as aprendizagens individuais para informar a equipe.
- Aumentam o conhecimento e a compreensão da equipe, ao mesmo tempo que minimizam os riscos do compromisso.

As diretrizes a seguir detalham como a Microsoft e o cliente podem incorporar as investigações de viabilidade de engenharia nos processos ágeis do dia a dia.

## Pré-Mortem

Uma boa maneira de avaliar quais investigações de engenharia conduzir é fazer um [pré-mortem](https://www.facebook.com/business/m/thinkkit/exercises/strong-starts/pre-mortem).

### O que é um pré-mortem?

- Uma reunião de 90 minutos após a visão/ADS que inclui toda a equipe (e também pode incluir o cliente) que responde a "Imagine que o projeto falhou. Quais problemas e desafios causaram esse fracasso?"
- Permite que toda a equipe levante inicialmente preocupações e riscos no início do compromisso.

Essa entrada é usada para decidir quais riscos abordar como investigações de engenharia.

## Compartilhamento de Aprendizados e Progresso Atual

### Ciclo de Feedback

O elemento-chave da condução das investigações de viabilidade de engenharia é compartilhar os resultados em andamento.

- A equipe se reúne e compartilha o aprendizado em uma base semanal (ou com mais frequência, se necessário).
- O compartilhamento é feito por meio de uma chamada de 30 minutos.
- Todos na Equipe de Desenvolvimento participam da chamada (mesmo que nem todos tenham uma história de investigação de viabilidade de engenharia atribuída ou mesmo se o trabalho de investigação estiver em andamento e não estiver totalmente concluído).

O ciclo de feedback é significativamente mais curto do que no processo ágil baseado em sprint. Em vez de usar o Sprint como a função forçadora para ajustar/pivotar/repriorizar, as sessões de compartilhamento intermediário eram o gatilho.

### Repriorização das próximas investigações

Depois que a equipe compartilha o progresso atual, outra rodada de planejamento é feita. Isso permite que a equipe:

- Estabeleça um ciclo de feedback muito mais curto.
- Repriorize a(s) próxima(s) investigação(ões) com base no resultado das investigações de viabilidade de engenharia atuais.

### Ajustando com base no contexto

Durante a chamada de compartilhamento e quando a equipe acredita ter informações suficientes, a equipe às vezes percebe que os critérios de aceitação originais da investigação já não são válidos. A equipe muda para outra área que fornece mais valor.

Um [registro de decisões](../registro-de-decisoes/README.md) pode ser usado para rastrear resultados.

### Diagrama de Sprints de Investigação de Viabilidade de Engenharia

O processo é representado no diagrama abaixo.

![Ciclo de feedback de investigação de viabilidade de engenharia](images/engineering-feasibility-spike-feedback-loop.png)

## Benefícios

### Criação de exemplos de código para comprovar ideias

É importante observar que é necessário ser intencional sobre as investigações não visarem produzir código de produção.

- Às vezes, a equipe deve escrever código para chegar à aprendizagem técnica.
- A equipe deve estar ciente de que o código escrito para as investigações não servirá como código para a solução final.
- O código escrito é apenas o suficiente para impulsionar a investigação com maior confiança.

Por exemplo, suponha que a equipe estava explorando a coreografia de API na criação de um cliente do Microsoft Graph com vários fluxos de autenticação e permissões do Azure Active Directory (AAD). O código para demonstrar isso é implementado em um aplicativo de console, mas poderia ter sido feito por meio de um servidor Express, etc. O fato de ser um aplicativo de console não era importante, mas sim a capacidade do cliente do Microsoft Graph de realizar operações no endpoint da API do Microsoft Graph com o número mínimo de permissões é o principal objetivo de aprendizado.

### Conversas direcionadas

Ao compartilhar o progresso da investigação, o conhecimento coletivo da equipe aumenta.

- As investigações permitem que a equipe conduza conversas concisas com vários Grupos de Produtos (PGs) e outros especialistas em assuntos (SMEs).
- Em vez de falar em um nível hipotético, a equipe apresenta preocupações de projeto/arquitetura concretas e aponta de forma concreta por que algo é um obstáculo ou não é uma maneira viável de avançar.

### Aumento da confiança do cliente

Esse processo leva ao aumento da confiança do cliente.

Usando esse processo, a equipe:

- Leva o cliente junto no processo de tomada de decisão e o orienta sobre como prosseguir.
- Fornece respostas com confiança e sugere designs arquitetônicos sólidos.

A condução das investigações de viabilidade de engenharia prepara a equipe e o cliente para o sucesso, especialmente se destacar aprendizados tecnológicos que ajudem o cliente a entender completamente a viabilidade de uma solução de engenharia.

## Resumo dos principais pontos

- Um pré-mortem pode envolver toda a equipe na identificação de riscos de negócios e técnicos.
- O objetivo principal da investigação de viabilidade de engenharia é a aprendizagem.
- A aprendizagem ocorre tanto durante a condução quanto no compartilhamento de insights das investigações.
- Use novas aprendizagens obtidas por meio das investigações para revisar, refinar, repriorizar ou criar o próximo conjunto de investigações.
- Quando as investigações são concluídas, procure novos ritmos sem

anais, como adicionar uma coluna de 'risco' ao quadro de retrospectiva ou levantar tópicos na [standup diária](../../../agile-development/core-expectations/README.md) para identificar riscos emergentes.
