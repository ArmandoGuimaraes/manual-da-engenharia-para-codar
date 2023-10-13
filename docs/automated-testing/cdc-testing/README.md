# Teste de Contrato Orientado pelo Consumidor (CDC)

O Teste de Contrato Orientado pelo Consumidor (ou CDC, na sigla em inglês) é uma metodologia de teste de software usada para testar componentes de um sistema isoladamente, garantindo que os componentes provedores sejam compatíveis com as expectativas que os componentes consumidores têm deles.

## Por que Teste de Contrato Orientado pelo Consumidor

O CDC tenta superar as [várias desvantagens dolorosas](https://pactflow.io/blog/proving-e2e-tests-are-a-scam) dos testes E2E automatizados com componentes interagindo juntos:

* Testes E2E são lentos
* Testes E2E quebram facilmente
* Testes E2E são caros e difíceis de manter
* Testes E2E de sistemas maiores podem ser difíceis ou impossíveis de serem executados fora de um ambiente de teste dedicado

Embora as melhores práticas de teste sugiram escrever apenas alguns testes E2E em comparação com os testes de integração e unitários mais baratos, rápidos e estáveis, como ilustrado na pirâmide de testes abaixo, a experiência mostra que [muitas equipes acabam escrevendo muitos testes E2E](https://testing.googleblog.com/2015/04/just-say-no-to-more-end-to-end-tests.html). Uma razão para isso é que os testes E2E dão aos desenvolvedores a maior confiança para lançar, pois estão testando o "sistema real".

![Pirâmide de Teste E2E](./images/testing-pyramid.png)

O CDC aborda essas questões testando interações entre componentes isoladamente usando mocks que estão de acordo com um entendimento compartilhado documentado em um "contrato". Contratos são acordados entre consumidor e provedor e são regularmente verificados contra uma instância real do componente provedor. Isso efetivamente divide um sistema maior em partes menores que podem ser testadas individualmente isoladas umas das outras, levando a testes mais simples, rápidos e estáveis que também dão confiança para lançar.

Alguns testes E2E ainda são necessários para verificar o sistema como um todo quando implantado no ambiente real, mas a maioria das interações funcionais entre componentes pode ser coberta com testes CDC.

O teste CDC foi inicialmente desenvolvido para testar APIs RESTful, mas o padrão se aplica a todos os sistemas consumidor-provedor e existem ferramentas para outros protocolos de mensagens além do HTTP.

## Blocos de Design de Teste de Contrato Orientado pelo Consumidor

Em uma [abordagem orientada pelo consumidor](https://martinfowler.com/articles/consumerDrivenContracts.html), o consumidor direciona as mudanças nos contratos entre um consumidor (o cliente) e um provedor (o servidor). Isso pode parecer contraintuitivo, mas ajuda os provedores a criar APIs que se ajustem às necessidades reais dos consumidores, em vez de tentar adivinhá-las antecipadamente. A seguir, descrevemos os blocos de construção do CDC ordenados por sua ocorrência no ciclo de desenvolvimento.

![Teste CDC](./images/cdc-testing.png)

### Testes do Consumidor com Mock do Provedor

Os consumidores começam criando testes de integração contra um mock do provedor e executando-os como parte de seu pipeline de CI. Respostas esperadas são definidas no mock do provedor para solicitações disparadas a partir dos testes. Por meio disso, o consumidor essencialmente define o contrato que espera que o provedor cumpra.

### Contrato

Contratos são gerados a partir das expectativas definidas no mock do provedor como resultado de uma execução bem-sucedida do teste. Frameworks de CDC como o [Pact](https://docs.pact.io/) fornecem uma [especificação para contratos](https://github.com/pact-foundation/pact-specification) no formato json, consistindo na lista de solicitações/respostas geradas a partir dos testes do consumidor, além de alguns metadados adicionais.

Contratos não são um substituto para uma discussão entre a equipe do consumidor e do provedor. Este é o momento em que essa discussão deve ocorrer (se já não tiver ocorrido antes). Os testes do consumidor e o contrato gerado são refinados com o feedback e a cooperação da equipe do provedor. Por último, o contrato finalizado é versionado e armazenado em um local central acessível por ambos, consumidor e provedor.

Contratos complementam documentos de especificação de API como o OpenAPI. Especificações de API descrevem a estrutura e o formato da API. Um contrato, por outro lado, especifica que para uma determinada solicitação, uma determinada resposta é esperada. Um documento de especificações de API é útil para escrever um contrato de API e pode ser usado para validar que o contrato está em conformidade com a especificação da API.

### Verificação de Contrato do Provedor

Do lado do provedor, testes também são executados como parte de um pipeline separado que verifica contratos contra respostas reais do provedor. A verificação do contrato falha se as respostas reais diferirem das respostas esperadas, conforme especificado no contrato. A causa disso pode ser:

1. Expectativas inválidas do lado do consumidor, levando à incompatibilidade com a implementação atual do provedor.
2. Implementação defeituosa do provedor devido a alguma funcionalidade ausente ou a uma regressão.

De qualquer forma, graças ao CDC, é fácil identificar problemas de integração até o consumidor/provedor da interação afetada. Isso é uma grande vantagem em comparação com a dor de depuração que isso poderia ter sido com uma abordagem de teste E2E.

## Frameworks e Ferramentas de Teste CDC

[Pact](https://docs.pact.io/) é uma implementação de teste CDC que permite a simulação de respostas no código-base do consumidor e a verificação das interações no código-base do provedor, enquanto define uma [especificação para contratos](https://github.com/pact-foundation/pact-specification). Foi originalmente escrito em Ruby, mas tem wrappers disponíveis para várias linguagens. Pact é o padrão de facto a ser usado ao trabalhar com CDC.

[Spring Cloud Contract](https://cloud.spring.io/spring-cloud-contract/reference/html) é uma implementação de teste CDC da Spring e oferece fácil integração no ecossistema Spring. Suporte para provedores e consumidores não-Spring e não-JVM também existe.

## Conclusão

O CDC tem vários benefícios que o tornam uma abordagem a ser considerada ao lidar com sistemas compostos por múltiplos componentes interagindo juntos.

Os esforços de manutenção podem ser reduzidos testando interações entre consumidor e provedor isoladamente, sem a necessidade de um ambiente integrado complexo, especialmente à medida que as interações entre componentes aumentam em número e se tornam mais complexas.

![CDC VS Testes E2E](./images/cdc-vs-e2e.png)

Além disso, uma colaboração próxima entre as equipes de consumidores e provedores é fortemente incentivada através do processo de desenvolvimento do CDC, o que pode trazer muitos outros benefícios. Contratos oferecem uma forma formal de documentar o entendimento compartilhado de como os componentes interagem entre si e servem como base para a comunicação entre as equipes. De certa forma, o repositório de contratos serve como uma documentação ao vivo de todas as interações entre consumidor e provedor de um sistema.

O CDC tem algumas desvantagens também. Uma camada extra de teste é adicionada, exigindo um investimento adequado em educação para que os membros da equipe entendam e usem o CDC corretamente.

Além disso, [o escopo do teste CDC](https://docs.pact.io/getting_started/testing-scope) deve ser considerado cuidadosamente para evitar confundir o CDC com outras camadas de teste funcional de nível superior. Testes de contrato não são o lugar para verificar a lógica de negócios interna e a correção do consumidor.

## Recursos

* Pirâmide de teste do [blog de Kent C. Dodd](https://blog.kentcdodds.com/write-tests-not-too-many-mostly-integration-5e8c7fff591c)
* [Pact](https://docs.pact.io/), uma ferramenta de teste de contrato orientada pelo consumidor baseada em código com suporte para várias linguagens de programação diferentes
* [Contratos orientados pelo consumidor](https://martinfowler.com/articles/consumerDrivenContracts.html) de Ian Robinson
* [Teste de contrato](https://martinfowler.com/bliki/ContractTest.html) de Martin Fowler
* Um exemplo simples de uso do [teste de contrato orientado pelo consumidor Pact em uma aplicação cliente-servidor Java](https://github.com/oottka/pact-spring)
* [Workshop Pact dotnet](https://github.com/pact-foundation/pact-workshop-dotnet-core-v1)
