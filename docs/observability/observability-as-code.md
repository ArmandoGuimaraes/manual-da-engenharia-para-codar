# Observabilidade como Código

Na medida do possível, a configuração e o gerenciamento de ativos de observabilidade, como provisionamento de recursos em nuvem, alertas de monitoramento e painéis, devem ser gerenciados como código. A Observabilidade como Código é alcançada usando qualquer um dos seguintes: Terraform, Ansible ou Modelos ARM.

## Exemplos de Observabilidade como Código

1. Painéis como Código - Painéis de Monitoramento podem ser criados como modelos JSON ou XML. Este modelo é mantido no controle de origem e qualquer alteração nos painéis pode ser revisada. A automação pode ser criada para habilitar o painel. [Saiba mais sobre como fazer isso no Azure](https://learn.microsoft.com/en-us/azure/azure-portal/azure-portal-dashboards-create-programmatically). Painéis do Grafana também podem ser [configurados como código](https://grafana.com/blog/2020/02/26/how-to-configure-grafana-as-code/), o que eventualmente pode ser controlado no controle de origem para ser usado na automação e em pipelines.

2. Alertas como Código - Alertas podem ser criados no Azure usando Terraform ou modelos ARM. Tais alertas podem ser controlados no controle de origem e implantados como parte de pipelines (pipelines do Azure DevOps, Jenkins, GitHub Actions, etc.). Algumas referências de como fazer isso são: [Alerta de Métrica de Monitoramento com Terraform](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/monitor_metric_alert). Alertas também podem ser criados com base em consultas de log analytics e podem ser definidos como código usando [Regras de Alerta de Consulta Programada do Terraform Monitor](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/monitor_scheduled_query_rules_alert#example-usage).

3. Automação de Consultas do Log Analytics - Existem vários casos de uso em que a automação de consultas do log analytics pode ser necessária. Por exemplo, geração automática de relatórios, execução de consultas personalizadas de forma programática para análise, depuração, etc. Para que esses casos de uso funcionem, as consultas de log devem ser controladas no controle de origem e a automação pode ser criada usando [log analytics REST](https://learn.microsoft.com/en-us/rest/api/loganalytics/) ou [azure cli](https://learn.microsoft.com/en-us/cli/azure/ext/log-analytics/monitor/log-analytics?view=azure-cli-latest).

## Por que

- Torna a configuração repetível e automatizável. Também evita a configuração manual de alertas de monitoramento e painéis do zero em todos os ambientes.

- Painéis configurados ajudam a solucionar erros durante a integração e implantação (CI/CD).

- Podemos auditar alterações e revertê-las se houver problemas.

- Identificar insights acionáveis a partir dos dados de métricas gerados em todos os ambientes, não apenas na produção.

- A configuração e o gerenciamento de ativos de observabilidade, como limiar de alerta, duração, valores de configuração, usando IAC, ajudam a evitar erros de configuração, erros ou omissões durante a implantação.

- Ao praticar a observabilidade como código, as alterações podem ser revisadas pela equipe, assim como outras contribuições de código.
