# Diagramas de Componentes

## Propósito

Este documento tem como objetivo fornecer uma compreensão básica do que são, por que são usados e como incorporar Diagramas de Componentes como parte de seu envolvimento. Em relação ao "como", a seção abaixo fornecerá ferramentas e plugins para otimizar o máximo possível a geração de Diagramas de Componentes por meio do VSCode.

A [Wikipedia](https://en.wikipedia.org/wiki/Component_diagram) define os Diagramas de Componentes UML como:

 > _um diagrama de componentes representa como os componentes são conectados para formar componentes maiores ou sistemas de software._

Os Diagramas de Componentes são um tipo de estrutura estática porque se concentram na responsabilidade e nas relações entre os componentes como parte do sistema ou solução geral.

Eles não têm a finalidade de informar sobre o fluxo de dados, as responsabilidades do chamador ou do chamado, os fluxos de solicitação ou quaisquer outras características relacionadas ao "comportamento".

...Espere um segundo... o que é um Componente?

Um [Componente](https://en.wikipedia.org/wiki/Component_(UML)) é uma solução executável que executa um conjunto de operações e pode ser acessado por meio de uma API específica. Pode-se pensar em Componentes como um pedaço de software "independente" - pense em repositórios de dados, microsserviços, funções serverless, interfaces de usuário, etc...

## Principais Pontos

Os dois principais pontos de um Diagrama de Componentes devem ser:

- Uma visão rápida de todos os vários componentes (Interface de Usuário, Serviço, Armazenamento de Dados) envolvidos no sistema
- Os "pontos de contato" imediatos que um determinado Componente tem com outros Componentes, incluindo como esse "ponto de contato" é realizado (HTTP, FTP, etc...)

Dependendo da complexidade do sistema, uma equipe pode decidir criar vários Diagramas de Componentes. Nesse caso, haveria um diagrama por Componente (representando todos os seus "pontos de contato" imediatos com outros Componentes).

Ou, se o sistema for simples, a equipe pode optar por criar um único Diagrama de Componentes que englobe todos os Componentes do sistema.

## Quando Criar?

Como os Diagramas de Componentes representam uma visão geral de alto nível de todo o sistema com foco nos Componentes, é recomendável começar a criação deste diagrama desde o início de um envolvimento e atualizá-lo à medida que os diferentes Componentes forem identificados, desenvolvidos e introduzidos no sistema. Caso contrário, se isso for deixado para mais tarde, há o risco de:

- a equipe não conseguir identificar áreas de melhoria
- a equipe ou outras partes interessadas necessárias não terem uma compreensão completa de como o sistema funciona à medida que ele é desenvolvido

Devido à granularidade inerente do sistema, os Diagramas de Componentes não precisam ser atualizados com tanta frequência quanto os [Diagramas de Classes](./classDiagrams.md). Coisas que podem justificar a atualização de um Diagrama de Componentes podem incluir:

- Exclusão ou adição de um novo Componente no sistema
- Alteração das APIs de interação de um Componente do sistema
- Alteração dos "pontos de contato" imediatos de um Componente do sistema com outros Componentes

Como os Diagramas de Componentes se concentram em informar os vários "pontos de contato" entre os Componentes, requer algum planejamento antecipado para determinar quais Componentes são necessários e quais mecanismos de interação são mais eficazes de acordo com os requisitos do sistema. Essa quantidade de planejamento antecipado deve ser abordada de maneira **pragmática** - pois o design pode evoluir ao longo do tempo, e isso é perfeitamente aceitável, desde que as alterações sejam influenciadas por requisitos funcionais e requisitos não funcionais.

## Exemplos

Aqui estão alguns exemplos básicos:

![image](./Images/ecommerceSite.png)

![image](./Images/orderingSystem.png)

![image](./Images/withPersistenceAndSecurity.png)

## Versionamento

Como os Diagramas de Componentes mudarão periodicamente, é recomendável "publicar" periodicamente uma imagem do diagrama gerado. A frequência pode variar à medida que o envolvimento avança.

A abordagem abaixo pode ser usada para auxiliar a equipe a determinar com que frequência atualizar a versão publicada do diagrama:

- No início do envolvimento, publicar uma versão "imaginada" do Diagrama de Componentes fornecerá uma visão comum a todos os engenheiros ao trabalharem nas diferentes partes da solução.
- Ao longo do envolvimento, atualize o diagrama publicado periodicamente. Idealmente, sempre que um novo Componente for introduzido no sistema ou sempre que um novo "ponto de contato" ocorrer entre Componentes.

Dependendo da ferramenta utilizada, o versionamento automático pode ser realizado sempre que uma atualização no Diagrama for feita. Caso contrário, é recomendável capturar versões distintas sempre que houver uma necessidade específica do cliente de ter uma imagem do projeto em um ponto específico no tempo. O requisito fundamental é que o diagrama mais recente deve ser publicado e todos devem saber como acessá-lo à medida que a entrega ao cliente se aproxima.

## Recursos

- [Wikipedia](https://en.wikipedia.org/wiki

/Component_diagram)
- [Visual Paradigm](https://www.visual-paradigm.com/guide/uml-unified-modeling-language/what-is-component-diagram/#:~:text=UML%20Component%20diagrams%20are%20used%20in%20modeling%20the,model%20the%20static%20implementation%20view%20of%20a%20system.)
- Plugins para o VS Code:
  - [PlantUML](https://marketplace.visualstudio.com/items?itemName=jebbs.plantuml) - requer um gerador de código para a sintaxe PlantUML para gerar diagramas
    - [Sintaxe PlantUML](https://plantuml.com/component-diagram)
    - [Desenhar manualmente](https://towardsdatascience.com/drawing-a-uml-diagram-in-the-vs-code-53c2e67deffe)
