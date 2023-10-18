# Observabilidade em Microservices

Microservices é uma arquitetura de software muito popular, onde a aplicação é organizada como uma coleção de serviços fracamente acoplados. Alguns desses serviços podem ser escritos em diferentes linguagens por equipes diferentes.

## Motivações

Precisamos considerar casos especiais ao criar uma arquitetura de microservices do ponto de vista da observabilidade. Queremos capturar as interações ao fazer solicitações entre esses microservices e correlacioná-las.

Imagine que temos um microservice que acessa um banco de dados para recuperar alguns dados como parte de uma solicitação. Este microservice será chamado por outra pessoa como parte de uma solicitação HTTP de entrada ou de um processo interno em execução. O que acontece se ocorrer um problema durante a recuperação dos dados (ou a atualização dos dados)? Como podemos associar, ou correlacionar, que essa chamada específica falhou no microservice de destino?

Este é um problema comum. Ao chamar outros microservices, dependendo da pilha de tecnologia que usamos, podemos inadvertidamente ocultar erros e exceções que podem ocorrer do outro lado. Se estivermos usando uma interface REST simples, o outro microservice pode retornar um código de status HTTP 500 e não temos ideia do que aconteceu dentro desse microservice.

Mais importante, não temos nenhuma maneira de associar nosso Correlation Id ao que acontece dentro desse microservice. Portanto, é tão importante ter um plano em vigor para poder estender seus esforços de rastreamento e monitoramento, especialmente ao usar uma arquitetura de microservices.

## Como estender suas informações de rastreamento entre microservices

O consórcio W3C está trabalhando em uma definição de [Trace Context](https://www.w3.org/TR/trace-context/) que pode ser aplicada ao usar o HTTP como protocolo em uma arquitetura de microservices. Mas vamos explicar como podemos implementar essa funcionalidade em nosso software.

A ideia principal por trás disso é propagar as informações de correlação entre solicitações HTTP para que outros pedaços de software possam ler essas informações e correlacionar corretamente a telemetria entre os microservices.

A maneira de propagar essas informações é usar cabeçalhos HTTP para o Correlation Id, Correlation Id pai, etc.

Quando você está no escopo de uma solicitação HTTP, seu sistema de rastreamento já deve ter criado quatro propriedades que você pode usar para enviar entre seus microservices.

- RequestId:0HLQV2BC3VP2T:00000001,
- SpanId:da13aa3c6fd9c146,
- TraceId:f11a03e3f078414fa7c0a0ce568c8b5c,
- ParentId:5076c17d0a604244

Este é um exemplo das quatro propriedades que você pode encontrar, que identificam a solicitação atual.

- RequestId é o ID único que representa a solicitação HTTP atual.
- SpanId é o span automaticamente gerado por padrão. Você pode ter mais de um Span que abrange funcionalidades diferentes dentro do seu software.
- TraceId representa o ID para o registro de log atual.
- ParentId é o ID do span pai, que em alguns casos pode ser o mesmo ou algo diferente.

## Exemplo

Agora vamos explorar um exemplo com 3 microservices que se chamam em sequência.

![image](./microservices.png)

Esta imagem é um resumo do que é necessário em cada microservice para propagar o trace-id de A para C.

O chamador raiz é A e é por isso que não possui um parent-id, apenas possui um novo trace-id. Em seguida, A chama B usando HTTP. Para propagar as informações de correlação como parte da solicitação, estamos usando dois novos cabeçalhos com base na especificação de Correlação da W3C, trace-id e parent-id. Neste exemplo, porque A é o chamador raiz, A só envia seu próprio trace-id para o microservice B.

Quando o microservice B recebe a solicitação HTTP de entrada, ele verifica o conteúdo desses dois cabeçalhos. Ele lê o conteúdo do cabeçalho trace-id e define seu próprio parent-id para esse trace-id (conforme mostrado no retângulo verde dentro de B). Além disso, ele cria um novo trace-id para sinalizar que é um novo escopo para a telemetria. Durante a execução do microservice B, ele também chama o microservice C e repete o padrão. Como parte da solicitação, ele inclui os dois cabeçalhos e propaga o trace-id e o parent-id também.

Finalmente, o microservice C lê o valor do trace-id de entrada e define como seu próprio parent-id, mas também cria um novo trace-id que usará para enviar telemetria sobre suas próprias operações.

## Resumo

Vários produtos de tecnologia de monitoramento de aplicativos (APM) já suportam a maior parte dessa Propagação de Correlação. O mais popular é o [OpenZipkin/B3-Propagation](https://github.com/openzipkin/b3-propagation). A W3C já propôs uma recomendação para o [W3C Trace Context](https://www.w3.org/blog/2019/12/trace-context-enters-proposed-recommendation/), onde você pode ver quais SDKs e estruturas já suportam essa funcionalidade. É importante implementar corretamente a propagação, especialmente quando diferentes equipes usam pilhas de tecnologia diferentes no mesmo projeto.

> Considere o uso do [OpenTelemetry](./tools/OpenTelemetry.md), pois ele implementa a propagação de contexto de código aberto entre plataformas para transações distribuídas de ponta a ponta em componentes heterogêneos "out of the box". Ele cuida automaticamente da criação e gerenciamento do objeto Trace Context entre uma pilha completa de microservices implementados em diferentes tecnologias.
