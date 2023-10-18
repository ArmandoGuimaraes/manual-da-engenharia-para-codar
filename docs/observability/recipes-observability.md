# Receitas

## Application Insights/ASP.NET

[Repositório GitHub](https://github.com/Azure-Samples/application-insights-aspnet-sample-opentelemetry), [Artigo](https://devblogs.microsoft.com/aspnet/observability-asp-net-core-apps/).

## Application Insights/ASP.NET Core com propagação de contexto de rastreamento distribuído para o Kafka

[Repositório GitHub](https://github.com/MagdaPaj/application-insights-aspnet-sample-trace-context-propagation).

## Exemplo: OpenTelemetry em uma arquitetura orientada a mensagens em Java com Jaeger, Prometheus e Azure Monitor

[Repositório GitHub](https://github.com/iamnicoj/OpenTelemetry-Async-Java-with-Jaeger-Prometheus-AzMonitor)

## Exemplo: Configurar painéis e alertas do Azure Monitor com o Terraform

[Repositório GitHub](https://github.com/buzzfrog/azure-alert-dashboard-terraform)

## Application Insights em Ambientes Locais

[Application Insights em Ambientes Locais](https://github.com/c-w/appinsights-on-premises) é um serviço compatível com o Azure App Insights, mas armazena os dados em um banco de dados interno, como o PostgreSQL ou um armazenamento de objetos, como o [Azurite](https://github.com/Azure/Azurite).

O Application Insights em Ambientes Locais é útil como uma substituição direta do Azure Application Insights em cenários em que uma solução deve ser implantável na nuvem, mas também deve oferecer suporte a cenários de implantação desconectada em ambientes locais.

O Application Insights em Ambientes Locais também é útil para testar a integração de telemetria. Problemas relacionados à telemetria podem ser difíceis de identificar, pois muitas vezes essas integrações são excluídas dos fluxos de teste de unidade ou teste de integração, pois não é trivial usar um recurso Azure Application Insights ao vivo para testes, por exemplo, gerenciar a vida útil do recurso, ter que ignorar telemetria antiga para asserções, se um novo recurso for usado, pode levar um tempo para que a telemetria apareça, etc. O serviço Application Insights em Ambientes Locais pode ser usado para facilitar a integração com um ponto de API compatível com Azure Application Insights durante o desenvolvimento local ou integração contínua, sem a necessidade de criar um recurso no Azure. Além disso, o serviço simplifica os testes de integração de fluxos de trabalho assíncronos, como trabalhadores da web, pois os testes de integração agora podem ser escritos para asserções em relação à telemetria registrada no serviço, por exemplo, assegurar que nenhuma exceção foi registrada, assegurar que algum número de eventos de um tipo específico foi registrados dentro de um determinado intervalo de tempo, etc.

## Relatórios de Pipelines do Azure DevOps com o Power BI

O [Relatório de Pipelines do Azure DevOps](https://github.com/Azure-Samples/powerbi-pipeline-report) contém um modelo do [Power BI](https://learn.microsoft.com/pt-br/power-bi/fundamentals/power-bi-overview) para monitorar dados de projetos, pipelines e execuções de pipeline de uma organização do Azure DevOps (AzDO).

Este modelo de painel fornece observabilidade para pipelines do AzDO, exibindo várias métricas (ou seja, tempo médio de execução, estatísticas de resultado de execução, etc.) em uma tabela. Além disso, a segunda página do modelo visualiza as tendências de sucesso e falha do pipeline usando gráficos do Power BI. A documentação e as informações de configuração podem ser encontradas no README do projeto.

## Classe Logger Python para Application Insights usando OpenCensus

Este repositório contém a classe "AppLogger", que é uma classe de logger Python para o Application Insights usando o OpenCensus. Ele também contém código de exemplo que mostra o uso do "AppLogger".

[Repositório GitHub](https://github.com/Azure-Samples/azure-monitor-opencensus-python/tree/master/azure_monitor/python_logger_opencensus_azure)

## Exemplos de OpenTelemetry em Java

Este [Repositório GitHub](https://github.com/open-telemetry/opentelemetry-java-docs) contém um conjunto de exemplos totalmente funcionais do uso das APIs e SDKs do OpenTelemetry Java.
