# Painéis de Controle da UI do Kubernetes

Este documento aborda as opções e benefícios de vários painéis de controle da UI do Kubernetes, que são ferramentas úteis para monitorar e depurar sua aplicação em Clusters Kubernetes. Eles permitem a gestão de aplicativos em execução no cluster, a depuração deles e a gestão do cluster, tudo por meio desses painéis.

## Visão Geral e Contexto

Há momentos em que nem todas as soluções podem ser executadas localmente. Essa limitação pode ser devido a um serviço em nuvem que não oferece uma forma robusta ou eficiente de depurar o ambiente localmente. Nestes casos, é necessário usar outras ferramentas que proporcionem as capacidades de monitorar sua aplicação com o Kubernetes.

## Vantagens e Casos de Uso

- Permite a capacidade de visualizar, gerenciar e monitorar os aspectos operacionais do Cluster Kubernetes.

- Os benefícios de usar um painel de controle de UI incluem o seguinte:
  - ver uma visão geral do cluster
  - implantar aplicativos no cluster
  - depurar aplicativos em execução no cluster
  - visualizar, criar, modificar e excluir recursos do Kubernetes
  - visualizar métricas básicas de recursos, incluindo o uso de recursos para objetos Kubernetes
  - visualizar e acessar logs
  - visualização em tempo real do estado dos pods (por exemplo, iniciados, terminando, etc.)

- Diferentes painéis de controle podem fornecer funcionalidades diferentes, e a escolha de um painel de controle específico dependerá dos requisitos. Por exemplo, muitos painéis de controle oferecem apenas uma maneira de monitorar suas aplicações no Kubernetes, mas não oferecem uma maneira de gerenciá-las.

## Painéis de Controle de Código Aberto

Atualmente, existem vários painéis de controle de UI disponíveis para monitorar suas aplicações ou gerenciá-las com o Kubernetes. Por exemplo:

- [Octant](https://github.com/vmware-tanzu/octant)
- [Prometheus e Grafana](https://prometheus.io/docs/visualization/grafana/)
  - [Gráfico Kube Prometheus Stack](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack): fornece uma maneira fácil de operar o monitoramento de cluster Kubernetes de ponta a ponta com o Prometheus usando o Prometheus Operator.
- [K8Dash](https://github.com/indeedeng/k8dash)
- [kube-ops-view](https://github.com/hjacobs/kube-ops-view): uma ferramenta para visualizar a ocupação e utilização de nós
- [Lens](https://k8slens.dev/): Ferramenta de desktop do lado do cliente
- [Thanos](https://github.com/thanos-io/thanos) e [Cortex](https://cortexmetrics.io/docs/): Implementações multi-cluster

## Referências

- [Alternativas ao Painel de Controle do Kubernetes](https://octopus.com/blog/alternative-kubernetes-dashboards)
- [Prometheus e Grafana com Kubernetes](https://tanzu.vmware.com/developer/guides/kubernetes/observability-prometheus-grafana-p1/)
