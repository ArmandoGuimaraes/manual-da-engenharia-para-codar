# Observabilidade para Azure Databricks

## Visão Geral

O Azure Databricks é um serviço de análise baseado no Apache Spark que facilita o desenvolvimento e a implantação rápida de análises de big data. Monitorar e solucionar problemas de desempenho é fundamental ao operar cargas de trabalho de produção no Azure Databricks. É importante registrar informações adequadas do Azure Databricks para monitorar e solucionar problemas de desempenho de forma eficaz.

O Spark foi projetado para ser executado em um cluster - um cluster é um conjunto de Máquinas Virtuais (VMs). O Spark pode escalar horizontalmente com cargas de trabalho maiores, exigindo mais VMs. O Azure Databricks pode escalar conforme necessário, aumentando ou diminuindo o número de VMs.

## Abordagens para Observabilidade

### Logs de Diagnóstico do Azure

[Logs de Diagnóstico do Azure](https://learn.microsoft.com/en-us/azure/databricks/administration-guide/account-settings/azure-diagnostic-logs) são fornecidos prontos para uso no Azure Databricks, fornecendo visibilidade sobre ações executadas em relação ao DBFS, Clusters, Contas, Trabalhos, Notebooks, SSH, Espaço de Trabalho, Segredos, Permissões SQL e Pools de Instâncias.

Esses logs são habilitados usando o Azure Portal ou CLI e podem ser configurados para serem entregues a um destes recursos do Azure.

- Log Analytics Workspace
- Blob Storage
- Event Hub

### Logs de Eventos do Cluster

[Logs de Eventos do Cluster](https://learn.microsoft.com/en-us/azure/databricks/clusters/configure#cluster-log-delivery) fornecem uma visão rápida sobre eventos importantes do ciclo de vida do Cluster. Os
logs são estruturados - Timestamp, Tipo de Evento e Detalhes. Infelizmente, não há uma maneira nativa de exportar logs para o Log Analytics. Os logs terão que ser entregues ao Log Analytics usando [REST API](https://learn.microsoft.com/en-us/azure/databricks/dev-tools/api/latest/examples#cluster-log-example) ou serem consultados no dbfs usando o Azure Functions.

### Métricas de Desempenho das VMs (OMS)

O [Agente do Log Analytics](https://learn.microsoft.com/en-us/azure/virtual-machines/extensions/oms-linux) fornece insights sobre os contadores de desempenho das VMs do Cluster e ajuda a entender os padrões de utilização do Cluster. Aproveitando o Agente do Linux OMX para integrar as VMs ao Log Analytics, ajuda a fornecer insights sobre as métricas das VMs, desempenho, inventário e métricas do syslog. É importante
observar que o Agente OMS do Linux não é específico para o Azure Databricks.

### Logs de Aplicação

De todos os logs coletados, este é talvez o mais importante. A [biblioteca de monitoramento do Spark](https://github.com/mspnp/spark-monitoring) coleta métricas sobre o driver, executores, JVM, HDFS, cache
embaralhamento, DAGs e muito mais. Esta biblioteca fornece insights úteis para ajustar os trabalhos do Spark. Ela permite monitorar e rastrear cada camada dentro das cargas de trabalho do Spark, incluindo desempenho e uso de recursos no host e JVM, bem como métricas do Spark e logs de nível de aplicação. A biblioteca também inclui painéis do Grafana prontos para uso, que são um ótimo ponto de partida para criar painéis do Azure Databricks.

### Logs via REST API

O Azure Databricks também oferece suporte [REST API](https://learn.microsoft.com/en-us/azure/databricks/dev-tools/api/latest/). Se houver algum dado de log específico necessário, esses dados podem ser coletados usando chamadas da REST API.

### Logs de Fluxo NSG

[Logs de fluxo do grupo de segurança de rede (NSG)](https://learn.microsoft.com/en-us/azure/network-watcher/network-watcher-nsg-flow-logging-overview) é um recurso do Azure Network Watcher que permite registrar
informações sobre o tráfego IP que passa por um NSG. Os dados de fluxo são enviados para contas de armazenamento do Azure, de onde você pode acessá-los e exportá-los para qualquer ferramenta de visualização, SIEM ou IDS de sua escolha. Essas informações de log não são específicas para logs de fluxo do NSG. Esses dados podem ser usados para identificar tráfego desconhecido ou indesejado e monitorar níveis de tráfego e/ou consumo de largura de banda. Isso é possível apenas com workspaces injetados na VNET.

### Logs da Plataforma

Os logs da plataforma podem ser usados para revisar operações de provisionamento/desprovisionamento. Isso pode ser usado para revisar a atividade no grupo de recursos gerenciados do Databricks. Ajuda a descobrir operações executadas no
nível da assinatura (como provisionamento de VM, disco, etc.).

Esses logs podem ser habilitados por meio de Azure Monitor > Logs de Atividade e enviados para o Log Analytics.
