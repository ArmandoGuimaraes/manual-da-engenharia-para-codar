# Prometheus

## Visão Geral

Originalmente desenvolvido na SoundCloud, o [Prometheus](https://prometheus.io/docs/introduction/overview/) é uma ferramenta de monitoramento e alerta de código aberto baseada em dados de métricas de séries temporais. Tornou-se uma solução de métricas padrão no mundo nativo da nuvem e é amplamente utilizado com o Kubernetes.

O núcleo do Prometheus é um servidor que coleta e armazena métricas. Existem vários recursos e componentes opcionais, como o Alert-manager e [bibliotecas de cliente](https://prometheus.io/docs/instrumenting/clientlibs/) para linguagens de programação que estendem as funcionalidades do Prometheus além do básico.
As bibliotecas de cliente oferecem quatro [tipos de métricas](https://prometheus.io/docs/concepts/metric_types/): `Counter`, `Gauge`, `Histogram` e `Summary`.

## Por que o Prometheus?

- O Prometheus é um banco de dados de séries temporais que permite o acompanhamento, monitoramento e agregação de eventos ou medidas ao longo do tempo.
- O Prometheus é uma ferramenta de coleta ativa. Uma das maiores vantagens do Prometheus em relação a outras ferramentas de monitoramento é que ele coleta métricas de forma ativa, raspando alvos para recuperar métricas deles. O Prometheus também suporta o modelo de envio para enviar métricas.
- O Prometheus permite controlar como coletar métricas e com que frequência fazê-lo. Através do servidor Prometheus, podem existir várias configurações de coleta, permitindo várias taxas para diferentes alvos.
- Semelhante ao [Grafana](https://prometheus.io/docs/visualization/grafana/), a visualização das séries temporais pode ser feita diretamente por meio da interface web do Prometheus. A interface web oferece a capacidade de filtrar facilmente e ter uma visão geral do que está acontecendo com seus diferentes alvos.
- O Prometheus fornece uma poderosa linguagem de consulta funcional chamada PromQL (Prometheus Query Language) que permite ao usuário agregar dados de séries temporais em tempo real.

## Integração com Outras Ferramentas

As bibliotecas de cliente do Prometheus permitem adicionar instrumentação ao seu código e expor métricas internas por meio de um ponto de extremidade HTTP. As [bibliotecas de cliente oficiais do Prometheus](https://prometheus.io/docs/instrumenting/clientlibs/) atualmente são `Go`, `Java ou Scala`, `Python` e `Ruby`. Bibliotecas não oficiais de terceiros incluem: `.NET/C#`, `Node.js` e `C++`.

O formato de métricas do Prometheus é suportado por uma ampla variedade de ferramentas e serviços, incluindo:

- [Azure Monitor](https://learn.microsoft.com/en-us/azure/azure-monitor/containers/container-insights-prometheus-integration)
- [Stackdriver](https://cloud.google.com/stackdriver/docs/solutions/gke/prometheus)
- [Datadog](https://docs.datadoghq.com/integrations/prometheus/)
- [CloudWatch](https://aws.amazon.com/blogs/containers/using-prometheus-metrics-in-amazon-cloudwatch/)
- [New Relic](https://docs.newrelic.com/docs/integrations/prometheus-integrations/get-started/send-prometheus-metric-data-new-relic/)
- [Flagger](https://docs.flagger.app/tutorials/prometheus-operator)
- [Grafana](https://grafana.com/docs/grafana/latest/getting-started/getting-started-prometheus/)
- [GitLab](https://docs.gitlab.com/ee/user/project/integrations/prometheus.html)
- [etc...](https://prometheus.io/docs/operating/integrations/)

Existem numerosos [exportadores](https://prometheus.io/docs/instrumenting/exporters/) que são usados para exportar métricas existentes de bancos de dados de terceiros, hardware, ferramentas CI/CD, sistemas de mensagens, APIs e outros sistemas de monitoramento. Além das bibliotecas de cliente e exportadores, há um número significativo de [pontos de integração](https://prometheus.io/docs/operating/integrations/) para descoberta de serviços, armazenamento remoto, alertas e gerenciamento.

## Referências

- [Documentação do Prometheus](https://prometheus.io/docs)
- [Melhores Práticas do Prometheus](https://prometheus.io/docs/practices)
- [Grafana com Prometheus](https://prometheus.io/docs/visualization/grafana/)
