# Entrega Contínua em Soluções de Low-Code e No-Code

As plataformas de low-code e no-code ocuparam um lugar em uma ampla variedade de Soluções de Negócios envolvendo automação de processos, modelos de IA, Bots, Aplicações de Negócios e Inteligência de Negócios. Os cenários habilitados por essas plataformas estão constantemente evoluindo e abrindo espaço para funções produtivas. Este tem sido exatamente o motivo pelo qual a introdução de ferramentas profissionais em seu desenvolvimento se tornou necessária, como entrega controlada e automatizada.

No caso dos produtos da Power Platform, a adoção de um processo de CI/CD pode parecer aumentar a complexidade do desenvolvimento para uma solução orientada a [Desenvolvedores Cidadãos](https://www.gartner.com/en/information-technology/glossary/citizen-developer). No entanto, é mais importante tornar o processo de desenvolvimento mais escalável e capaz de lidar com novos recursos e correções de bugs de forma mais rápida.

## Ambientes em Soluções da Power Platform

Os ambientes são espaços onde as Soluções da Power Platform existem. Eles armazenam, gerenciam e compartilham tudo relacionado à solução, como dados, aplicativos, chat bots, fluxos e modelos. Eles também funcionam como contêineres para separar aplicativos que podem ter diferentes funções, requisitos de segurança ou apenas públicos-alvo diferentes. Eles podem ser usados para criar diferentes estágios do processo de desenvolvimento da solução, e o modelo esperado de trabalho com ambientes em um processo de CI/CD é como a imagem a seguir sugere.

![imagem](../images/environments.png)

### Considerações sobre Ambientes

Sempre que um ambiente é criado, seus recursos só podem ser acessados por usuários dentro do mesmo locatário, que é um locatário do Azure Active Directory, na verdade. Quando você cria um aplicativo em um ambiente, esse aplicativo só pode interagir com fontes de dados que também estão implantadas no mesmo ambiente, o que inclui conexões, fluxos e bancos de dados do Dataverse. Isso é uma consideração importante ao lidar com um processo de Entrega Contínua.

## Estratégia de Implantação

Com três ambientes já criados para representar os estágios da implantação, o objetivo agora é automatizar a implantação de um ambiente para outro. Cada ambiente exigirá a criação de sua própria solução: lógica de negócios e dados.

### Etapa 1

A equipe de desenvolvimento trabalhará em um ambiente **Dev**. Esses ambientes, de acordo com a equipe, podem ser um para a equipe ou um para cada desenvolvedor.

Depois que as alterações forem feitas, o primeiro passo será empacotar a solução e exportá-la para o controle de origem.

### Etapa 2

A segunda etapa envolve a solução. Você precisa ter uma solução gerenciada para implantar em outros ambientes, como **Stage** ou **Production**, então agora você deve usar um ambiente JIT onde importará sua solução não gerenciada e a exportará como gerenciada. Esses arquivos de solução não serão verificados no controle de origem, mas serão armazenados como um artefato de build no pipeline, tornando-os disponíveis para implantação no pipeline de release. É aqui que o segundo ambiente será usado. Este segundo ambiente será responsável por receber a solução gerenciada de saída vinda do artefato.

### Etapa 3

A terceira e última etapa importará a solução no ambiente de produção, o que significa que esta etapa pegará o artefato da última etapa e o exportará. Ao trabalhar neste ambiente, você também pode versionar seu produto para fazer um rastreamento mais eficiente do produto.

## Ferramentas

As ferramentas mais usadas para concluir esse processo são:

* [Ferramentas de Compilação da Power Platform](https://marketplace.visualstudio.com/items?itemName=microsoft-IsvExpTools.PowerPlatform-BuildTools).

* Também existe uma ferramenta não gráfica que pode ser usada para trabalhar com esse processo de Entrega Contínua. A ferramenta [Power CLI](https://aka.ms/PowerAppsCLI).

## Referências

[Gerenciamento de Ciclo de Vida de Aplicativos com a Microsoft Power Platform](https://learn.microsoft.com/en-us/power-platform/alm/)
