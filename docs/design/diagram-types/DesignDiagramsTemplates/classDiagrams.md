# Diagramas de Classes

## Propósito

Este documento tem como objetivo fornecer uma compreensão básica do que são, por que são usados e como incorporar Diagramas de Classes como parte de seu envolvimento. Em relação ao "como", a seção abaixo fornecerá ferramentas e plugins para automatizar o máximo possível a geração de Diagramas de Classes por meio do VSCode.

A [Wikipedia](https://en.wikipedia.org/wiki/Class_diagram) define os Diagramas de Classes UML como:

 > _um tipo de diagrama de estrutura estática que descreve a estrutura de um sistema, mostrando as classes do sistema, seus atributos, operações (ou métodos) e as relações entre objetos._

Os termos-chave a serem observados aqui são:

- estrutura estática
- mostrando as classes do sistema, atributos, operações e relacionamentos

Os Diagramas de Classes são um tipo de estrutura estática porque se concentram nas propriedades e relacionamentos das classes. Eles não têm a finalidade de informar sobre o fluxo de dados, as responsabilidades do chamador ou do chamado, os fluxos de solicitação ou quaisquer outras características relacionadas ao "comportamento".

## Principais Pontos

Cada "Componente" (peça independente de software - pense em repositórios de dados, microsserviços, funções serverless, interfaces de usuário, etc...) de um Produto ou Sistema terá seu próprio Diagrama de Classes.

Os Diagramas de Classes devem contar uma "história", onde cada Diagrama exigirá que os engenheiros pensem profundamente sobre:

- A responsabilidade / operações de cada classe. O que uma classe pode (e deve) fazer?
- Os atributos e propriedades da classe. O que pode ser definido por um implementador desta classe? Quais são todas (se houver) as propriedades universalmente estáticas?
- A visibilidade ou acessibilidade que uma operação de classe pode ter para outras classes
- O [relacionamento](https://en.wikipedia.org/wiki/Class_diagram#Relationships) entre cada classe ou as várias instâncias

## Quando Criar?

Porque os Diagramas de Classes representam uma das representações mais granulares do que um "produto" ou "sistema" é composto, é recomendável começar a criação desses diagramas no início e ao longo das partes de engenharia de um envolvimento.

Isso significa que qualquer mudança de código (nova funcionalidade, melhoria, refatoração de código) pode envolver a atualização de um ou muitos Diagramas de Classes. Embora isso possa parecer uma desvantagem dos Diagramas de Classes, na verdade pode se tornar um grande benefício.

Porque os Diagramas de Classes contam uma "história" para cada Componente de um produto (veja a seção anterior), eles exigem uma quantidade substancial de planejamento antecipado e considerações de design. Essa quantidade de planejamento antecipado, no final das contas, resulta em mudanças de código mais eficazes e pode até minimizar o nível de refatorações em estágios futuros do envolvimento.

Os Diagramas de Classes também fornecem indicadores rápidos de "alerta" quando uma refatoração pode ser necessária. As razões podem ser devido ao fato de que uma classe específica pode estar fazendo muito, ter muitas dependências ou quando a base de código pode produzir um Diagrama de Classes muito "confuso" ou "caótico". **Se o Diagrama de Classes for ilegível, o código provavelmente será ilegível**.

## Exemplos

Pode-se encontrar muitos exemplos online, como em [Diagramas UML](https://www.uml-diagrams.org/class-diagrams-examples.html).

Abaixo estão alguns exemplos básicos:

![image](./Images/generalization-aggregation-association.png)

![image](./Images/realization.png)

## Versionamento

Como os Diagramas de Classes mudarão rapidamente, essencialmente sempre que uma classe for alterada no código, e porque podem ser muito grandes, é recomendável "publicar" periodicamente uma imagem do diagrama gerado. A frequência pode variar à medida que o envolvimento avança.

A abordagem abaixo pode ser usada para auxiliar a equipe a determinar com que frequência atualizar a versão publicada do diagrama:

- Espere até que o envolvimento progrida (talvez 10-20% de conclusão) antes de publicar um Diagrama de Classes. Não vale a pena publicar um Diagrama de Classes desde o início, pois ele estará mudando diariamente.
- Uma vez que as classes mais cruciais forem desenvolvidas, atualize o diagrama publicado periodicamente. Idealmente, sempre que uma grande refatoração ou uma nova classe for introduzida. Se a equipe usar um plugin de IDE para gerar automaticamente o diagrama de seu ambiente de desenvolvimento, isso se tornará mais uma t

arefa de documentação do que uma necessidade.
- À medida que o envolvimento se aproxima do final (90-100% de conclusão), atualize o diagrama publicado sempre que houver uma alteração em uma classe existente como parte dos critérios de aceitação de uma funcionalidade ou história.

Dependendo da ferramenta usada, o versionamento automático pode ser realizado sempre que uma atualização do Diagrama for feita. Se não for o caso, é recomendável capturar versões distintas sempre que houver uma necessidade específica do cliente de ter uma imagem do projeto em um ponto específico no tempo. O requisito fundamental é que o diagrama mais recente deve ser publicado e todos devem saber como acessá-lo quando a entrega ao cliente se aproximar.

## Recursos

- [Wikipedia](https://en.wikipedia.org/wiki/Class_diagram)
- [Visual Paradigm](https://www.visual-paradigm.com/guide/uml-unified-modeling-language/uml-class-diagram-tutorial/#:~:text=A%20class%20diagram%20in%20the%20Unified%20Modeling%20Language,%28or%20methods%29%2C%204%20and%20the%20relationships%20among%20objects.)
- Plugins para o VS Code:
  - C#, Visual Basic, C++ usando [Componente de Designer de Classe](https://marketplace.visualstudio.com/items?itemName=AlexShen.classdiagram-ts&ssr=false#overview)
  - TypeScript [classdiagram-ts](https://marketplace.visualstudio.com/items?itemName=AlexShen.classdiagram-ts&ssr=false#overview)
  - [PlantUML](https://marketplace.visualstudio.com/items?itemName=jebbs.plantuml) - requer um gerador de código para a sintaxe PlantUML para gerar diagramas
    - [Sintaxe PlantUML](https://plantuml.com/class-diagram)
    - [C# para PlantUML](https://marketplace.visualstudio.com/items?itemName=pierre3.csharp-to-plantuml)
    - [Desenhar manualmente](https://towardsdatascience.com/drawing-a-uml-diagram-in-the-vs-code-53c2e67deffe)
