# Métricas

## Visão Geral

As métricas fornecem um fluxo de dados quase em tempo real, informando operadores e partes interessadas sobre as funções que o sistema está executando, bem como sua saúde. Ao contrário do registro e do rastreamento, os dados métricos tendem a ser mais eficientes para transmitir e armazenar.

## Métodos de Coleta

As abordagens de coleta de métricas se enquadram em duas categorias amplas: métricas de envio e métricas de busca. Métricas de envio significam que o componente de origem envia os dados para um serviço remoto ou agente. [Azure Monitor](https://azure.microsoft.com/pt-br/services/monitor) e [statsd do Etsy](https://github.com/statsd/statsd) são exemplos de métricas de envio. Algumas vantagens das métricas de envio incluem:

- Requer apenas a saída de rede para o destino remoto.
- O componente de origem controla a frequência da medição.
- Configuração simplificada, pois o componente só precisa saber o destino para enviar os dados.

Algumas compensações com essa abordagem são:

- Em escala, é muito mais difícil controlar as taxas de transmissão de dados, o que pode causar limitação de serviço ou a perda de valores.
- Determinar se cada componente, especialmente em um ambiente de escala dinâmica, está saudável e enviando dados é difícil.

No caso das métricas de busca, cada componente de origem publica um ponto de extremidade para o agente de métricas se conectar e coletar medidas. [Prometheus](https://prometheus.io/) e seu ecossistema de ferramentas são um exemplo de métricas no estilo de busca. Benefícios experimentados usando uma configuração de métricas de busca podem incluir:

- Configuração única para determinar o que está sendo medido e a frequência da medição para o ambiente local.
- Cada alvo de medição possui uma métrica meta relacionada a se a coleta foi bem-sucedida ou não, o que pode ser usado como uma verificação geral de saúde.
- Suporte para roteamento, filtragem e processamento de métricas antes de enviá-las para um repositório global central de métricas.

Itens de preocupação para alguns podem incluir:

- Configurar e gerenciar fontes de dados pode levar a uma configuração complexa. O Prometheus possui ferramentas para autodescobrir e configurar fontes de dados em alguns ambientes, como o Kubernetes, mas sempre há exceções a isso, o que leva à complexidade da configuração.
- A configuração de rede pode adicionar mais complexidade se firewalls e outras ACLs precisarem ser gerenciados para permitir a conectividade.

## Melhores Práticas

### Quando devo usar métricas em vez de logs?

[Logs vs Métricas vs Rastreamentos](../log-vs-metric-vs-trace.md) aborda algumas orientações de alto nível sobre quando utilizar dados métricos e quando usar dados de log. Ambos desempenham um papel valioso na criação de sistemas observáveis.

### O que deve ser monitorado?

Medições críticas do sistema relacionadas à saúde da aplicação/máquina, que geralmente são excelentes candidatas a alertas. Trabalhe com seus colegas de engenharia e devops para identificar as métricas, mas elas podem incluir:

- Utilização de CPU e memória.
- Taxa de solicitações.
- Tamanho da fila.
- Contagem de exceções inesperadas.
- Métricas de serviços dependentes, como tempo de resposta para o cache Redis, servidor SQL ou serviço de barramento de mensagens.

Medições importantes relacionadas aos negócios, que impulsionam relatórios para partes interessadas. Consulte as várias partes interessadas do componente, mas alguns exemplos podem incluir:

- Tarefas executadas.
- Duração da sessão do usuário.
- Jogos jogados.
- Visitas ao site.

### Rótulos de Dimensão

Os sistemas de métricas modernos hoje geralmente definem uma única métrica de série temporal como a combinação do nome da métrica e seu dicionário de rótulos de dimensão. Rótulos são uma excelente maneira de distinguir uma instância de uma métrica de outra, permitindo ainda a agregação e outras operações para análise. Alguns rótulos comuns usados em métricas podem incluir:

- Nome do Contêiner
- Nome do Host
- Versão do Código
- Nome do cluster Kubernetes
- Região do Azure

_Observação_: Como os rótulos de dimensão são usados para agregações e operações de agrupamento, não use strings exclusivas ou com alta cardinalidade como o valor de um rótulo. O valor do rótulo é significativamente reduzido para relatórios e, em muitos casos, tem um impacto negativo no desempenho do sistema de métricas usado para rastreá-lo.

## Ferramentas Recomendadas

- [Azure Monitor](https://learn.microsoft.com/en-us/azure/azure-monitor/overview) - Conjunto de serviços, incluindo métricas do sistema, análise de registros e muito mais.
- [Prometheus](https://learn.microsoft.com/en-us/azure/azure-monitor/overview) - Um aplicativo de monitoramento e alerta em tempo real. Seu formato de exposição para exposição de séries temporais é a base para o formato padrão OpenMetrics.
- [Thanos](https://thanos.io) - Configuração do Prometheus de código aberto, altamente disponível, com capacidade de armazenamento de longo prazo.
- [Cortex](https://cortexmetrics.io) - Escalável horizontalmente, altamente disponível, multilocatário, Prometheus de longo prazo.
- [Grafana](https://grafana.com) - Ferramenta de painel e visualização de código aberto. Suporta fontes de dados de log, métricas e rastreamentos distribuídos.
