# Ferramentas e Padrões

Existem várias ferramentas modernas para tornar sistemas observáveis. Ao identificar e/ou criar ferramentas que funcionam para o seu sistema, aqui estão algumas coisas a serem consideradas para ajudar a orientar as escolhas.

- Deve ser simples de integrar e fácil de usar.
- Deve ser possível agregar e visualizar dados.
- As ferramentas devem fornecer dados em tempo real.
- Deve ser capaz de guiar os usuários para a área do problema com um contexto adequado de ponta a ponta.

## Escolhas

- [Loki](./loki.md)
- [OpenTelemetry](./OpenTelemetry.md)
- [Painéis do Kubernetes](./KubernetesDashboards.md)
- [Prometheus](./Prometheus.md)

## Service Mesh

Alavancar um Service Mesh que segue o [Padrão Sidecar](https://learn.microsoft.com/en-us/azure/architecture/patterns/sidecar) configura rapidamente um conjunto de métricas e traces, embora as traces precisem ser propagadas manualmente a partir das solicitações de entrada para as solicitações de saída.

Um sidecar funciona interceptando todo o tráfego de entrada e saída da sua imagem. Em seguida, ele adiciona cabeçalhos de trace a cada solicitação e emite um conjunto padrão de logs e métricas. Essas métricas são extremamente poderosas para observação, permitindo que cada serviço, seja do lado do cliente ou do lado do servidor, aproveite um conjunto unificado de métricas, incluindo:

- Latência
- Bytes
- Taxa de solicitação
- Taxa de erro

Em uma arquitetura de microserviços, identificar a causa raiz de um aumento nas respostas com código 500 pode ser complicado, mas com a observação adicional de um sidecar, você pode rapidamente determinar qual serviço em sua malha de serviços resultou no aumento nos erros.

Os Service Meshes têm uma grande área de superfície para configuração e podem parecer uma empreitada assustadora para implantar. No entanto, a maioria dos serviços (incluindo o Linkerd) oferece um conjunto sensato de padrões, e pode ser implantada seguindo o caminho feliz para obter rapidamente esses benefícios de observação.
