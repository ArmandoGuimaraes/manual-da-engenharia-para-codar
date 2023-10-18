# Logs vs. Metrics vs. Traces

![Sinais](./images/signals.png)

## Visão Geral

### Métricas

O objetivo das métricas é informar observadores sobre a saúde e as operações de um componente ou sistema. Uma métrica representa uma medida pontual em um determinado momento de uma fonte específica e, em termos de dados, tende a ser muito pequena. O tamanho compacto permite a coleta eficiente, mesmo em sistemas grandes em escala. As métricas também se prestam muito bem à pré-agregação dentro do componente antes da coleta, reduzindo o custo computacional para o processamento e armazenamento de um grande número de séries temporais de métricas em um sistema central. Devido à eficiência no processamento e armazenamento de métricas, elas são uma excelente fonte de dados de saúde para todos os componentes no sistema e são muito adequadas para uso em alertas automatizados.

### Registros (Logs)

Os dados de registro informam os observadores sobre os eventos discretos que ocorreram dentro de um componente ou conjunto de componentes. Quase todos os componentes de software registram informações sobre suas atividades ao longo do tempo. Esses dados ricos costumam ser muito maiores do que os dados métricos e podem causar problemas de processamento, especialmente se os componentes estiverem registrando informações em excesso. Portanto, o uso de dados de registro para entender a saúde de um sistema extenso tende a ser evitado e depende das métricas para esses dados. Uma vez que a telemetria de métricas destaca possíveis fontes de problemas, os dados de registro filtrados para essas fontes podem ser usados para entender o que ocorreu.

### Rastreamentos (Traces)

Onde o registro fornece uma visão geral de um registro discreto acionado por eventos, o rastreamento abrange uma visão muito mais ampla e contínua de uma aplicação. O objetivo do rastreamento é seguir o fluxo de um programa e a progressão dos dados.

Em muitas instâncias, o rastreamento representa a jornada de um único usuário por todo o conjunto de aplicativos. Seu objetivo não é reativo, mas sim focado na otimização. Ao rastrear um conjunto, os desenvolvedores podem identificar gargalos e focar na melhoria do desempenho.

Um rastreamento distribuído é definido como uma coleção de spans (trechos). Um span é a menor unidade em um rastreamento e representa uma parte do fluxo de trabalho em um cenário distribuído. Pode ser uma solicitação HTTP, uma chamada a um banco de dados ou a execução de uma mensagem de uma fila.

Quando ocorre um problema, o rastreamento permite ver como você chegou lá:

* Qual função.
* A duração da função.
* Parâmetros passados.
* Até que ponto o usuário pode entrar na função.

## Orientações de Uso

Quando usar dados métricos ou de registro para rastrear uma telemetria específica pode ser resumido nos seguintes pontos:

* Use métricas para rastrear a ocorrência de um evento, contagem de itens, o tempo necessário para realizar uma ação ou para relatar o valor atual de um recurso (CPU, memória, etc.).
* Use registros para rastrear informações detalhadas sobre um evento também monitorado por uma métrica, especialmente erros, avisos ou outras situações excepcionais.
* Um rastreamento fornece visibilidade sobre como uma solicitação é processada por vários serviços em um ambiente de microsserviços. Cada rastreamento precisa ter um identificador único associado a ele.
