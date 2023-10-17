# Diagramas de Sequência

## Propósito

Este documento tem como objetivo fornecer uma compreensão básica do que são, por que são usados e como incorporar Diagramas de Sequência como parte de um envolvimento. Quanto ao **como**, a seção na parte inferior fornecerá ferramentas e complementos para simplificar o máximo possível a geração de Diagramas de Sequência por meio do VSCode.

A [Wikipedia](https://en.wikipedia.org/wiki/Sequence_diagram) define os Diagramas de Sequência UML como responsáveis por:

 > _representar os objetos envolvidos no cenário e a sequência de mensagens trocadas entre os objetos necessários para realizar a funcionalidade do cenário_

O que é um **cenário**? Pode ser:

- uma persona de usuário real realizando uma ação
- um gatilho específico do sistema (com base no tempo, com base em condições) que resulta em uma ação a ser realizada

O que é uma **mensagem** nesse contexto? Pode ser:

- uma solicitação síncrona ou assíncrona
- uma transferência de qualquer forma de dados entre quaisquer objetos

O que é um **objeto** nesse contexto? Pode ser:

- qualquer persona de usuário específica
- qualquer serviço
- qualquer armazenamento de dados
- um sistema (caixa preta composta de serviços desconhecidos, armazenamentos de dados ou outros componentes)
- um subcenário abstrato (para minimizar a alta complexidade de um cenário)

## Principais Pontos

Um Diagrama de Sequência deve:

- começar com um **cenário**
- indicar qual **objeto** ou "ator" iniciou esse cenário
- ter o cenário indicando claramente qual é o estado "final", mesmo que não termine necessariamente com o objeto que iniciou o cenário

É aceitável que um único Diagrama de Sequência tenha muitos cenários diferentes se tiverem algum contexto relacionado que justifique que eles sejam agrupados.

Outro ponto importante a ter em mente é que os **objetos** envolvidos em um Diagrama de Sequência devem se referir aos Componentes existentes de um [Diagrama de Componentes](./componentDiagrams.md).

Existem duas áreas em que a complexidade pode resultar em um Diagrama de Sequência excessivamente "lotado", tornando-o caro de manter. São elas:

1. Grande número de objetos/componentes envolvidos em um cenário específico.
2. Captura de todas as possíveis situações de "falha" que um cenário pode encontrar.

### Grande Número de Objetos

Um Diagrama de Sequência normalmente começa com uma persona de usuário final realizando uma ação e, em seguida, mostra todos os vários componentes e transferências de solicitações/dados envolvidos nesse cenário. No entanto, na maioria das vezes, o fluxo completo de ponta a ponta para esse cenário pode ser muito complexo para ser capturado em um único Diagrama de Sequência.

Quando esse nível de complexidade ocorrer, considere criar Diagramas de Sequência separados para **subcenários** e usá-los como objetos em um Diagrama de Sequência específico. Exemplos disso são "Autenticação" ou "Autorização". Quase todos os cenários de persona de usuário terão vários objetos/componentes envolvidos em um desses subcenários, mas não é necessário incluí-los em todos os Diagramas de Sequência
quando os subcenários têm um Diagrama de Sequência autônomo criado.

Certifique-se de que, ao usar essa abordagem de subcenários, eles tenham um nome que encapsule o que o subcenário está realizando e determine o "ator" apropriado e a "ação" que inicia o subcenário.

A combinação e narração de histórias entre esses **Diagramas de Sequência de usuários finais** e **Diagramas de Sequência de subcenários** podem melhorar significativamente a legibilidade, distribuindo o nível de complexidade em vários diagramas e aproveitando a reutilização de subcenários comuns.

### Lidando com um Grande Número de Situações de Falha

Outro fator de alta complexidade são as possíveis situações de falha que um cenário específico pode encontrar. Cada objeto/componente envolvido em um cenário pode ter várias situações de "falha" diferentes, o que pode resultar em um Diagrama de Sequência muito confuso.

Para tornar realista a gestão de todas essas situações, tente:

- Identificar as situações de "falha" mais comuns que um "ator" pode enfrentar como parte de um cenário. Capturar essas em um Diagrama de Sequência e documentar os outros cenários sem a necessidade de gerenciá-los em um diagrama alcançará o objetivo de conscientização.
- "Agrupe" e "abstraia" todas as inúmeras situações de falha que podem ocorrer mais abaixo no sistema e represente como o objeto/componente mais próximo do "ator" lida com todas essas falhas e informa o "ator" sobre elas.

## Quando Criar?

Como os Diagramas de Sequência representam uma visão detalhada do **comportamento** do sistema, delineando as várias mensagens/solicitações enviadas dentro do sistema, é recomendável começar a criar esses diagramas desde o início de um envolvimento. Atualize-os à medida que as várias comunicações entre Componentes são introduzidas no sistema. Os riscos de não criar Diagramas de Sequência
no início são:

- a equipe não criará nenhum porque será percebido mais como uma "tarefa" do que como um valor agregado
- a equipe será incapaz de obter insights a tempo, visualizando as várias mensagens e solicitações enviadas entre Componentes, a fim de realizar qualquer refatoração potencial
- a equipe ou outras partes interessadas necessárias não terão uma compreensão completa do fluxo de solicitação/mensagem/dados dentro do sistema

Devido à granularidade inerente do sistema, os Diagramas de Sequência não precisarão ser atualizados com a mesma frequência que os [Diagramas de Classes](./classDiagrams.md), mas podem exigir mais manutenção do que os [Diagramas de Componentes](./componentDiagrams.md). Coisas que podem justificar a atualização de um Diagrama de Sequência incluem:

- Uma nova solicitação/mensagem/dado sendo enviado entre Componentes envolvidos em um cenário
- Uma alteração em um ou vários Componentes envolvidos em um Diagrama

 de Sequência, como dividir um componente em vários ou consolidar muitos Componentes em um único
- A introdução de um novo Caso de Uso ou cenário que o sistema agora suporta

## Exemplos

Cenário de Colocação de Pedido:

- Uma persona de usuário "Membro" realiza um pedido, que pode ser composto por muitos "itens de pedido"
- A persona de usuário "Membro" pode ser do tipo "VIP" ou "Comum"
- Dependendo do "tipo de membro", cada "item de pedido" será enviado por meio de um Correio ou por Correio
- Se a persona de usuário "Membro" selecionou a opção de ser informada assim que todos os "itens de pedido" forem enviados, então o sistema enviará uma notificação

![image](./Images/placeOrderScenario.png)

Cenário de Autenticação de Usuário no Facebook:

- Uma persona de usuário usa um Navegador Web para interagir com um "aplicativo", que tenta acessar um "recurso do Facebook" específico
- O "Servidor de Autorização do Facebook" é envolvido para fazer com que o usuário se autentique no Facebook
- A persona de usuário então recebe um "formulário de permissão" para autorizar o "aplicativo" a acessar o "recurso do Facebook"
- Se o "aplicativo" não for autorizado, ele retorna um erro
- Se o "aplicativo" for autorizado, ele obtém um "token de acesso" do "Servidor de Autorização do Facebook" e o usa para acessar com segurança o "recurso do Facebook" do "Servidor de Conteúdo do Facebook". Uma vez obtido o conteúdo, o "aplicativo" o envia de volta para o Navegador Web

![image](./Images/facebookUserAuthentication.png)

## Versionamento

Como os Diagramas de Sequência são mais caros de manter, é recomendável "publicar" uma imagem do diagrama gerado com frequência, sempre que um novo "caso de uso" ou "cenário" for identificado como parte do comportamento ou requisitos do sistema.

O elemento mais importante desses diagramas é garantir que a versão mais recente seja **precisa**. Se o diagrama mais recente mostrar uma sequência de comunicação entre componentes que não são mais válidos, o diagrama causará mais prejuízo do que benefício.

A abordagem abaixo pode ser usada para ajudar a equipe a determinar com que frequência atualizar a versão publicada do diagrama:

- No início do envolvimento, a publicação de uma versão "imaginada" do Diagrama de Sequência fornecerá uma visão comum a todos os engenheiros ao trabalharem nas diferentes partes da solução (focando no fluxo de dados e fluxo de solicitação)
- Ao longo do envolvimento, atualize o diagrama publicado periodicamente. Idealmente, sempre que um novo "caso de uso" ou "cenário" for identificado, ou quando um Componente for introduzido ou removido no sistema, ou quando uma mudança no fluxo de dados/solicitação for feita no sistema

Dependendo da ferramenta utilizada, o versionamento automático pode ser realizado sempre que uma atualização no Diagrama for feita. Caso contrário, é recomendável capturar versões distintas sempre que houver uma necessidade específica do cliente de ter um instantâneo do projeto em um determinado momento. O requisito fundamental é que o diagrama mais recente seja publicado e todos saibam como acessá-lo quando a entrega ao cliente se aproximar.

## Recursos

- [Wikipedia](https://en.wikipedia.org/wiki/Sequence_diagram)
- [Visual Paradigm](https://www.visual-paradigm.com/guide/uml-unified-modeling-language/what-is-sequence-diagram/)
- Complementos para o VS Code:
  - [PlantUML](https://marketplace.visualstudio.com/items?itemName=jebbs.plantuml) - requer um gerador de código para a sintaxe PlantUML para gerar diagramas
    - [Sintaxe PlantUML](https://plantuml.com/sequence-diagram)
    - [Desenho manual](https://towardsdatascience.com/drawing-a-uml-diagram-in-the-vs-code-53c2e67deffe)
