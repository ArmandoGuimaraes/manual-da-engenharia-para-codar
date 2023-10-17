# Feedback de Engenharia da Microsoft

## Por que é importante enviar Feedback de Engenharia da Microsoft

O Feedback de Engenharia captura a "voz do cliente" e é um mecanismo importante para fornecer insights acionáveis e ajudar os grupos de produtos da Microsoft a melhorar continuamente a plataforma e os serviços em nuvem para permitir que todos os clientes sejam o mais produtivos possível.

> Observe que o Feedback de Engenharia é um método assíncrono (ou seja, não em tempo real) para capturar e agregar pontos de atrito entre vários clientes e interações com código. Portanto, se você precisa relatar uma interrupção no serviço ou um bug que esteja bloqueando imediatamente, você deve abrir um ticket de suporte oficial da [Azure](https://azure.microsoft.com/support/create-ticket/) e, se possível, fazer referência ao ID do ticket no feedback que você enviar posteriormente.

Mesmo que o feedback já tenha sido apresentado diretamente a um grupo de produtos ou por meio de canais online como GitHub ou Stack Overflow, ainda é importante apresentá-lo via Feedback de Engenharia da Microsoft, para que possa ser consolidado com outros projetos de clientes que tenham o mesmo feedback, a fim de auxiliar na priorização.

## Quando enviar Feedback de Engenharia

Capturar e fornecer Feedback de Engenharia acionável de alta qualidade é uma parte **contínua** e integral de todas as interações com código. Recomenda-se enviar feedback de forma contínua, em vez de agrupá-lo para envio no final da interação.

Você deve anotar os detalhes do feedback perto do momento em que encontrar os bloqueios, desafios e atritos específicos, pois é quando eles estão mais frescos em sua mente. A equipe do projeto pode então decidir como priorizar e quando enviar o feedback para o sistema oficial de Feedback da CSE (acessível aos membros da equipe ISE) durante cada sprint.

## O que é Feedback de Engenharia bom e de alta qualidade

Um feedback de engenharia bom fornece informações suficientes para aqueles que não fazem parte da interação com código entenderem a dor do cliente, os problemas do produto associados, o impacto e a prioridade desses problemas, e quaisquer soluções alternativas potenciais para minimizar esse impacto.

### Feedback de Engenharia de Alta Qualidade é

* Orientado para Objetivos - indica o que o cliente está tentando realizar
* Específico - detalha o cenário, observação ou desafio enfrentado pelo cliente
* Acionável - inclui as informações necessárias para permitir uma decisão

### Exemplos de Feedback de Engenharia Bom

Por exemplo, aqui está uma evolução transformando um feedback fictício com as orientações de feedback de engenharia de alta qualidade mencionadas acima:

| Estágio                     | Evolução do Feedback                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
|-----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Feedback inicial            | Azure Functions Service Bus Trigger é lento para cenários em ordem                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| Tornando-o **Orientado para Objetivos** | **O cliente solicita o recebimento em lote para o desencadeador de Azure Functions Service Bus com sessões habilitadas para melhor suportar mensagens de maior throughput. Eles desejam usar o Azure Functions para processar o máximo de mensagens por segundo possível com latência mínima e em uma ordem específica.**                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| Adicionando **Detalhes Específicos**        | O cenário do cliente era receber **um total de 250 mensagens/segundo de 50 produtores com requisitos de ordenação por produtor e latência mínima, usando um tópico Service Bus com sessões habilitadas para ordenação. O recebimento em lote não é suportado no Azure Functions Service Bus Trigger.**                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| Tornando-o **Acionável**    | O cenário do cliente era receber um total de 250 mensagens/segundo de 50 produtores com requisitos de ordenação por produtor e latência mínima, usando um tópico Service Bus com sessões habilitadas para ordenação. **De acordo com a [documentação da Microsoft](https://learn.microsoft.com/azure/service-bus-messaging/service-bus-performance-improvements#prefetching-and-receivebatch), o recebimento em lote é recomendado para melhor desempenho, mas atualmente não é suportado no Azure Functions Service Bus Trigger. O impacto e a solução alternativa foram escolher contêineres em vez de Funções. O resultado desejado é que o Azure Functions suporte sessões do Service Bus com processamento em lote e não em lote para todas as linguagens GA do Azure Functions.** |

Para exemplos do mundo real, siga [Exemplos de Feedback](feedback-examples.md).

## Como enviar Feedback de Engenharia

Siga o [Guia de Feedback de Engenharia](feedback-guidance.md) para garantir que você forneça feedback que possa ser triado e processado de forma mais eficiente.

Revise a página de [Perguntas Frequentes](feedback-faq.md) para obter informações adicionais sobre o processo de feedback de engenharia.
