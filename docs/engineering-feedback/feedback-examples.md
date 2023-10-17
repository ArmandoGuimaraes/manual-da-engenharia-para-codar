# Exemplos de Feedback de Engenharia

A seguir, apresento exemplos do mundo real de Feedback de Engenharia que resultaram em melhorias nos produtos e na remoção de bloqueios para os clientes.

## Suporte a Contêineres do Windows Server no Azure Kubernetes Service

O Azure Kubernetes Service deve ter suporte de primeira classe para contêineres do Windows para que soluções que requerem cargas de trabalho do Windows possam ser implantadas em uma plataforma de orquestração de contêineres amplamente popular. A necessidade era ser capaz de implantar contêineres do Windows Server no AKS, o Serviço Gerenciado de Kubernetes da Azure. De acordo com [esta FAQ](https://learn.microsoft.com/en-us/azure/aks/faq#can-i-run-windows-server-containers-on-aks) (e confirmação paralela), isso ainda não está disponível.

Tentamos implantar mesmo assim como teste, e não funcionou - a implantação ficava pendente sem sucesso.

Mais de uma dúzia de grandes parceiros/clientes estão impedidos de implantar cargas de trabalho do Windows no AKS devido à falta de suporte para contêineres do Windows Server. Eles precisam desse recurso para que soluções que requerem cargas de trabalho do Windows possam ser implantadas nesta popular plataforma de orquestração de contêineres.

Estamos vendo o surgimento de empresas começando a experimentar contêineres do Windows como uma opção para mover suas cargas de trabalho do Windows para a nuvem. A Gartner afirma que 80% das aplicações empresariais rodam no Windows. Contêineres se tornaram o mecanismo de implantação padrão na indústria, e consistência e velocidade de implantação são alguns dos fatores importantes que as empresas estão buscando. Permitir aplicativos do Windows e garantir que os desenvolvedores tenham uma boa experiência ao mover suas cargas de trabalho para o Azure por meio de contêineres do Windows é fundamental para manter os clientes existentes do Windows no ecossistema Azure e impulsionar a adoção do Azure para novas cargas de trabalho.

Também estamos vendo um aumento de interesse, especialmente entre clientes empresariais, em usar um único plano de controle de orquestração para gerenciar tanto cargas de trabalho do Linux quanto do Windows.

> Este feedback foi criado como um feedback de alta prioridade e seguiu internamente até ser abordado. Aqui está [o anúncio](https://azure.microsoft.com/en-in/blog/announcing-the-preview-of-windows-server-containers-support-in-azure-kubernetes-service/).

## Suporte ao Recebimento em Lote com Sessões no Azure Functions Service Bus Trigger

O cenário do cliente era receber um total de 250 mensagens por segundo de 50 produtores com requisitos de ordenação e latência mínima, usando um tópico Service Bus com sessões habilitadas para ordenação. De acordo com a [documentação da Microsoft](https://learn.microsoft.com/en-us/azure/service-bus-messaging/service-bus-performance-improvements#prefetching-and-receivebatch), o recebimento em lote é recomendado para melhor desempenho, mas atualmente não é suportado no Azure Functions Service Bus Trigger. O impacto (e a solução alternativa) foi escolher contêineres em vez de Funções. O Critério de Aceitação é que o Azure Functions suporte sessões do Service Bus com processamento em lote e não em lote para todas as linguagens GA do Azure Functions.

> Este feedback foi [criado como um feedback](https://github.com/Azure/azure-functions-servicebus-extension/issues/15) com o grupo de produtos Azure Functions e também seguiu internamente até ser abordado.

## Stream Analytics - Sem suporte para dimensionamento para baixo sem tempo de inatividade

Para atualizar o número de Unidades de Streaming no Stream Analytics, é necessário interromper o serviço e esperar minutos para que ele reinicie. Isso é inaceitável para clientes que precisam de análise quase em tempo real. Para reiniciar um trabalho, são necessários até 2 minutos, o que não é aceitável para uma solução de streaming em tempo real. Também seria ideal se o dimensionamento para cima e para baixo pudesse ser feito automaticamente, definindo valores de limite que, quando alcançados, aumentam ou diminuem automaticamente a quantidade de UDs disponíveis. Esse feedback é para solicitar aos clientes a capacidade de dimensionar para baixo sem tempo de inatividade no Stream Analytics.

Declaração do Problema: Para atualizar o número de "Unidades de Streaming", os parceiros devem interromper o serviço e esperar até que ele reinicie. O parceiro deve ser capaz de atualizar o número sem interromper o serviço.

Experiência Desejada: Os parceiros devem poder atualizar o número de Unidades de Streaming sem interromper o serviço associado.

> Este feedback foi criado como um feedback de alta prioridade e seguiu até ser abordado em dezembro de 2019.

## Suporte ao Python para o Azure Functions

Vários clientes já usam o Python como parte de seu fluxo de trabalho e gostariam de poder usar o Python para o Azure Functions. Isso é especialmente verdadeiro, pois muitos deles já têm scripts em execução em outras nuvens e serviços.

Além disso, o suporte ao Python estava em Preview por muito tempo e está faltando muita funcionalidade.

Essa solicitação de recurso é uma das mais pedidas e tem um grande potencial para impulsionar as cargas de trabalho baseadas em Machine Learning (ML).

> Este feedback foi criado como um feedback com o grupo de produtos Azure Functions e também seguiu internamente até ser abordado. Aqui está [o anúncio](https://azure.microsoft.com/en-us/blog/announcing-the-general-availability-of-python-support-in-azure-functions/).
