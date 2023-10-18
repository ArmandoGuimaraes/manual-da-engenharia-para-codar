# Tracing

## Visão Geral

Produz as informações necessárias para observar uma série de operações correlacionadas em um sistema distribuído. Uma vez coletadas, elas mostram o caminho, as medidas e as falhas em uma transação de ponta a ponta.

## Melhores Práticas

- Certifique-se de que pelo menos transações comerciais-chave sejam rastreadas.
- Inclua em cada rastreamento informações necessárias para identificar lançamentos de software (ou seja, nome do serviço, versão). Isso é importante para correlacionar implantações e degradação do sistema.
- Certifique-se de que as dependências estejam incluídas no rastreamento (bancos de dados, I/O).
- Se os custos forem uma preocupação, use amostragem, evitando descartar erros, comportamentos inesperados e informações críticas.
- Não reinvente a roda, use ferramentas existentes para coletar e analisar os dados.
- Garanta que as políticas e restrições de informações pessoais sejam seguidas.

## Ferramentas Recomendadas

- [Azure Monitor](https://learn.microsoft.com/en-us/azure/azure-monitor/overview) - Conjunto de serviços, incluindo métricas do sistema, análise de registros e muito mais.
- [Jaeger Tracing](https://www.jaegertracing.io) - Rastreamento distribuído de ponta a ponta de código aberto.
- [Grafana](https://grafana.com) - Ferramenta de painel e visualização de código aberto. Suporta fontes de dados de log, métricas e rastreamento distribuído.

> Considere o uso do [OpenTelemetry](../tools/OpenTelemetry.md), pois ele implementa a propagação de contexto de código aberto multiplataforma para transações distribuídas de ponta a ponta em componentes heterogêneos. Ele cuida automaticamente da criação e gerenciamento do objeto de contexto de rastreamento em uma pilha completa de microsserviços implementados em diferentes pilhas técnicas.
