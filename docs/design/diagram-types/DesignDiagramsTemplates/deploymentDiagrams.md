# Diagramas de Implantação

## Propósito

Este documento tem como objetivo fornecer uma compreensão básica do que são, por que são usados e como incorporar Diagramas de Implantação como parte de seu envolvimento.

A [Wikipedia](https://en.wikipedia.org/wiki/Deployment_diagram) define os Diagramas de Implantação UML como:

 > _modela a implantação física de artefatos em nós_

Os Diagramas de Implantação são um tipo de estrutura estática porque se concentram na infraestrutura e hospedagem onde todos os aspectos do sistema residem.

Eles não têm a finalidade de informar sobre o fluxo de dados, as responsabilidades do chamador ou do chamado, os fluxos de solicitação ou quaisquer outras características relacionadas ao "comportamento".

## Principais Pontos

O diagrama de implantação deve conter todos os Componentes identificados nos [Diagramas de Componentes](./componentDiagrams.md), mas capturados juntamente com os seguintes elementos:

- Firewalls
- VNETs e sub-redes
- Máquinas virtuais
- Serviços em nuvem
- Armazenamento de dados
- Servidores (Web, proxy)
- Balanceadores de carga

Este diagrama deve informar à audiência:

- onde as coisas estão hospedadas/em execução
- quais são os limites de rede envolvidos no sistema

## Quando Criar?

Como os Diagramas de Implantação representam a arquitetura final de "hospedagem", é recomendável criar o diagrama "imaginado final" desde o início de um envolvimento. Isso permite que a equipe tenha uma ideia compartilhada do que a equipe está trabalhando para alcançar. Tenha em mente que isso pode mudar se algum requisito não funcional não foi considerado no início do envolvimento. Isso é aceitável, mas
requer a criação dos itens de backlog necessários e a atualização do diagrama de implantação para capturar essas mudanças.

Também é válido criar e manter um Diagrama de Implantação que represente o estado "atual" do sistema. Às vezes, pode ser benéfico ter um Diagrama de Implantação para cada ambiente (Desenvolvimento, QA, Staging, Produção, etc...). No entanto, isso aumenta a quantidade de manutenção necessária e só deve ser realizado se houver diferenças substanciais entre os ambientes.

O Diagrama de Implantação "atual" deve ser atualizado quando:

- Um novo elemento foi introduzido ou removido no sistema (consulte a seção "Principais Pontos" para uma lista de possíveis elementos)

## Exemplos

Aqui estão alguns exemplos básicos:

![image](./Images/azureDeploymentDiagram.png)

![image](./Images/deploymentDiagram.png)

## Versionamento

Como os Diagramas de Implantação mudarão periodicamente, é recomendável "publicar" periodicamente uma imagem do diagrama gerado. A frequência pode variar à medida que o envolvimento avança.

A abordagem abaixo pode ser usada para auxiliar a equipe a determinar com que frequência atualizar a versão publicada do diagrama:

- No início do envolvimento, publicar uma versão "imaginada" do Diagrama de Componentes fornecerá uma visão comum a todos os engenheiros ao trabalharem nas diferentes partes da solução.
- Ao longo do envolvimento, atualize o diagrama "atual" (estado representado do "ramo principal") periodicamente. Idealmente, sempre que um novo Componente for introduzido no sistema, ou sempre que um novo "ponto de contato" ocorrer entre Componentes.

## Recursos

- [Wikipedia](https://en.wikipedia.org/wiki/Deployment_diagram)
- [Visual Paradigm](https://www.visual-paradigm.com/guide/uml-unified-modeling-language/what-is-deployment-diagram/)
  - [PlantUML](https://marketplace.visualstudio.com/items?itemName=jebbs.plantuml) - requer um gerador de código para a sintaxe PlantUML para gerar diagramas
    - [Sintaxe PlantUML](https://plantuml.com/deployment-diagram)
    - [Desenhar manualmente](https://towardsdatascience.com/drawing-a-uml-diagram-in-the-vs-code-53c2e67deffe)
