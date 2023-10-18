# IDs de Correlação

## A Necessidade

Em uma arquitetura de sistema distribuído (arquitetura de microsserviços), é altamente difícil entender o fluxo de uma única transação de cliente de ponta a ponta por meio dos vários componentes.

Aqui estão alguns dos desafios gerais:

- Torna-se desafiador entender o comportamento de ponta a ponta de uma solicitação de cliente que entra na aplicação.
- Agregação: Consolidar registros de vários componentes e dar sentido a esses registros é difícil, senão impossível.
- Dependências cíclicas em serviços, sequência de eventos e solicitações assíncronas não são facilmente decifradas.
- Ao solucionar uma solicitação, o contexto de diagnóstico dos registros é muito importante para chegar à raiz do problema.

## Solução

Um ID de Correlação é um identificador único que é adicionado à primeira interação (solicitação de entrada) para identificar o contexto e é passado para todos os componentes envolvidos no fluxo de transação. O ID de Correlação se torna a cola que une a transação e ajuda a criar uma imagem geral dos eventos.

>Nota: Antes de implementar seu próprio ID de Correlação, verifique se sua ferramenta de telemetria de escolha fornece um ID de Correlação gerado automaticamente e se ele atende aos propósitos de sua aplicação. Por exemplo, o [Application Insights](https://learn.microsoft.com/en-us/azure/azure-monitor/app/auto-collect-dependencies) oferece a coleta automática de dependências para alguns frameworks de aplicação.

### Práticas Recomendadas

1. Atribua a cada solicitação externa um ID de Correlação que vincule a mensagem a uma transação.
2. O ID de Correlação para uma transação deve ser atribuído o mais cedo possível.
3. Propague o ID de Correlação para todos os componentes/serviços downstream.
4. Todos os componentes/serviços da transação devem usar esse ID de Correlação em seus registros.
5. Para uma solicitação HTTP, o ID de Correlação é geralmente passado no cabeçalho.
6. Adicione-o a uma resposta de saída, sempre que possível.
7. Com base no caso de uso, pode haver IDs de correlação adicionais que podem ser necessários. Por exemplo, pode ser necessário rastrear registros com base tanto no ID da Sessão quanto no ID do Usuário. Ao adicionar vários IDs de correlação, lembre-se de propagá-los pelos componentes.

>Considere o uso do [OpenTelemetry](./tools/OpenTelemetry.md), pois ele implementa a propagação de contexto de código aberto multiplataforma para transações distribuídas de ponta a ponta em componentes heterogêneos pronto para uso. Ele cuida automaticamente da criação e gerenciamento do "ID de Correlação", chamado de TraceId.

## Casos de Uso

### Correlação de Logs

A correlação de logs é a capacidade de rastrear eventos discrepantes em diferentes partes da aplicação. Ter um ID de Correlação fornece mais contexto, facilitando a criação de regras para relatórios e análises.

### Sistemas secundários de relatórios/observadores

Usar o ID de Correlação ajuda os sistemas secundários a correlacionar dados sem contexto de aplicação. Alguns exemplos incluem a geração de métricas com base em dados de rastreamento, a integração de diagnósticos em tempo de execução/sistema etc. Por exemplo, alimentar dados do AppInsights e correlacioná-los com problemas de infraestrutura.

### Solução de Problemas de Erros

Para solucionar erros, o ID de Correlação é um ótimo ponto de partida para rastrear o fluxo de trabalho de uma transação.
