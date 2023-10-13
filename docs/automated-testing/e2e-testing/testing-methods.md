# Métodos de Teste E2E

## Teste Horizontal

Este método é muito comumente usado. Ele ocorre horizontalmente no contexto de várias aplicações. Pegue, por exemplo, um sistema de gerenciamento de ingestão de dados.

![Teste Horizontal](./images/horizontal-e2e-testing.png)

Os dados de entrada podem ser injetados de várias fontes, mas depois são "achatados" em um pipeline de processamento horizontal que pode incluir vários componentes, como uma API de gateway, transformação de dados, validação de dados, armazenamento, etc. Ao longo de todo o processamento de Extração-Transformação-Carga (ETL), o fluxo de dados pode ser rastreado e monitorado sob o espectro horizontal com pequenos toques opcionais, e portanto não importantes para o caso de teste E2E geral, serviços como registro, auditoria, autenticação.

## Teste Vertical

Neste método, todas as transações mais críticas de qualquer aplicação são verificadas e avaliadas desde o início até o fim. Cada camada individual da aplicação é testada começando de cima para baixo. Pegue, por exemplo, uma aplicação baseada na web que usa serviços de middleware para acessar recursos de back-end.

![Teste Vertical](./images/vertical-e2e-testing.png)

Nesse caso, cada camada (nível) precisa ser totalmente testada em conjunto com as camadas "conectadas" acima e abaixo, nas quais os serviços "conversam" entre si durante o fluxo de dados de ponta a ponta. Todos esses cenários de teste complexos exigirão validação adequada e testes automatizados dedicados. Portanto, este método é muito mais difícil.

## Diretrizes para o Design de Casos de Teste E2E

A seguir estão listadas algumas **diretrizes** que devem ser levadas em consideração ao projetar os casos de teste para a realização de testes E2E:

- Os casos de teste devem ser projetados a partir da perspectiva do usuário final.
- Deve-se focar em testar alguns recursos existentes do sistema.
- Vários cenários devem ser considerados para criar múltiplos casos de teste.
- Diferentes conjuntos de casos de teste devem ser criados para focar em múltiplos cenários do sistema.
