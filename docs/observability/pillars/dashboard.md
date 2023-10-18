# Painel de Controle

## Visão Geral

Um painel de controle é uma forma de visualização de dados que fornece uma visão "de relance" dos Indicadores-Chave de Desempenho (KPIs) de um sistema observável. Um painel de controle conecta várias fontes de dados, permitindo a criação de representações visuais de insights de dados que, de outra forma, seriam difíceis de entender. O painel de controle pode ser usado para:

- Mostrar tendências
- Identificar padrões (usuário, uso, pesquisa etc.)
- Medir eficiência facilmente
- Identificar valores atípicos de dados e correlações
- Visualizar o estado de saúde ou desempenho do sistema
- Dar uma visão dos KPIs que são importantes para um negócio/processo

## Melhores Práticas

Perguntas comuns a se fazer ao criar um painel de controle seriam:

- Onde meu usuário passou a maior parte do tempo?
- O que meu usuário está procurando?
- Como posso ajudar melhor minha equipe com alertas e resolução de problemas?
- Meu sistema está saudável nos últimos dias/semanas/meses/trimestres?

Aqui estão princípios a serem considerados ao criar painéis de controle:

1. Separe um painel de controle em várias seções para simplificá-lo. Adicionar um salto de página ou um link âncora (#seção) também é uma vantagem, se aplicável.
2. Adicione gráficos simples e múltiplos. Construa gráficos simples e tenha mais deles em vez de um único gráfico complicado.
3. Identifique metas ou medições de KPI. Identificar metas ou KPIs ajuda a definir o que precisa ser alcançado. Aqui estão alguns exemplos - tempo de inatividade do servidor, tempo médio para resolver erros, acordo de nível de serviço.
4. Faça perguntas que possam ajudar a alcançar o objetivo ou KPI definido. Isso pode parecer contra-intuitivo, mas quanto mais perguntas forem feitas ao construir um painel de controle, melhor será o resultado. Perguntas como localização, provedor de serviços de Internet, horário do dia em que os usuários fazem solicitações ao servidor seriam um bom começo.
5. Valide as perguntas. Isso é frequentemente feito com partes interessadas, patrocinadores, líderes ou gerentes de projeto.
6. Observe o painel de controle que foi construído. Os dados estão refletindo o que as partes interessadas se propuseram a responder?
7. Lembre-se sempre de que esse processo leva tempo. Construir um painel de controle é fácil, mas construir um painel de controle observável para mostrar padrões é difícil.

## Ferramentas Recomendadas

- [Azure Monitor Workbooks](https://learn.microsoft.com/pt-br/azure/azure-monitor/platform/workbooks-overview) - Com suporte a markdown, o Azure Workbooks é intimamente integrado aos serviços Azure, tornando-o altamente personalizável sem a necessidade de ferramentas extras.
- [Criar painel de controle usando consulta de log](https://learn.microsoft.com/pt-br/azure/azure-monitor/learn/tutorial-logs-dashboards) - Painéis de controle podem ser criados usando consulta de log em dados do Log Analytics.
- [Construção de painéis de controle usando o Application Insights](https://learn.microsoft.com/pt-br/azure/azure-monitor/learn/tutorial-app-dashboards) - Painéis de controle podem ser criados usando o Application Insights também.
- [Power Bi](https://learn.microsoft.com/pt-br/power-bi/create-reports/service-dashboard-create) - O Power Bi é uma das ferramentas mais fáceis para criar painéis de controle a partir de fontes de dados e relatórios.
- [Grafana](https://grafana.com/tutorials/) - Introdução ao Grafana. O Grafana é uma ferramenta de código aberto popular para criação de painéis e visualização.
- [Azure Monitor como fonte de dados do Grafana](https://grafana.com/grafana/plugins/grafana-azure-monitor-datasource) - Isso fornece uma integração passo a passo do Azure Monitor com o Grafana.
- [Comparação breve de várias ferramentas](https://learn.microsoft.com/pt-br/azure/azure-monitor/visualizations)

## Amostras de Painéis de Controle e Receitas

### Azure Workbooks

- Análise de desempenho - Uma medição de como o sistema se comporta. Modelo de workbook disponível na galeria.
- Análise de falhas - Um relatório sobre falhas do sistema com detalhes. Modelo de workbook disponível na galeria.
- Índice de Desempenho de Aplicativo ([Apdex](https://en.wikipedia.org/wiki/Apdex)) - Esta é uma maneira de medir a satisfação do usuário. Classifica o desempenho em três zonas com base em um limite de desempenho de referência T. O modelo para Apdex também está disponível na galeria de Azure Workbooks.

### Application Insights

- [Análise de retenção de usuários](https://learn.microsoft.com/azure/azure-monitor/app/usage-retention)
- [Análise de padrões de navegação do usuário](https://learn.microsoft.com/azure/azure-monitor/app/usage-flows)
- [Análise de sessões de usuário](https://learn.microsoft.com/azure/azure-monitor/learn/tutorial-users)

Para outras ferramentas, essas amostras podem ser usadas como referência para recriar painéis de controle se um modelo não estiver prontamente disponível.

### Grafana com Azure Monitor como Fonte de Dados

- [Serviço de Kubernetes do Azure - Métricas de Cluster e Namespace](https://grafana.com/grafana/dashboards/10956) - Métricas do Container Insights para clusters Kubernetes. Utilização do cluster, utilização do namespace, CPU e memória do nó, uso de disco e E/S de disco do nó, métricas de rede do nó e operações do Docker kubelet.
- [Serviço de Kubernetes do Azure - Métricas de Nível de Contêiner e Pod](https://grafana.com/grafana/dashboards/14891) - Este painel contém métricas de nível de contêiner e de pod, como CPU e memória, que estão ausentes no painel acima.

## Resumo

Para construir um painel de controle observável, o objetivo é aproveitar métricas, logs, rastreamentos coletados para fornecer uma visão de como o sistema se comporta, como o usuário se comporta e identificar padrões.
