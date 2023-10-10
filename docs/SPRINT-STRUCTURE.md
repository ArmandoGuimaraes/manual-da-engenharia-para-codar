# Estrutura de um Sprint

O objetivo deste documento é:

- Organizar o conteúdo no playbook para rápida referência e descobrimento
- Fornecer conteúdo em uma estrutura lógica que reflita o processo de engenharia
- Hierarquia extensível para permitir que as equipes compartilhem conhecimento profundo em assuntos específicos

## A primeira semana de um Projeto ISE

### Antes de iniciar o projeto

- [ ] [Discutir e começar a escrever os Acordos da Equipe](agile-development/advanced-topics/team-agreements/README.md). Atualize esses documentos com quaisquer decisões de processo feitas ao longo do projeto
  - [Acordo de Trabalho](agile-development/advanced-topics/team-agreements/working-agreements.md)
  - [Definição de Pronto](agile-development/advanced-topics/team-agreements/definition-of-ready.md)
  - [Definição de Concluído](agile-development/advanced-topics/team-agreements/definition-of-done.md)
  - [Estimativa](agile-development/core-expectations/README.md)
- [ ] [Configurar o(s) repositório(s)](source-control/README.md#creating-a-new-repository)
  - Decidir sobre a estrutura(s) do repositório
  - Adicionar [README.md](resources/templates/README.md), [LICENSE](resources/templates/LICENSE), [CONTRIBUTING.md](resources/templates/CONTRIBUTING.md), .gitignore, etc
- [ ] [Construir um Backlog de Produto](agile-development/advanced-topics/backlog-management/README.md)
  - Configurar um projeto na sua ferramenta de gerenciamento de projeto escolhida (ex. Azure DevOps)
  - [INVEST](https://en.wikipedia.org/wiki/INVEST_(mnemonic)) em boas Histórias de Usuário e Critérios de Aceitação
  - [Orientação para Requisitos Não Funcionais](design/design-patterns/non-functional-requirements-capture-guide.md)

### Dia 1

- [ ] [Planejar o primeiro sprint](agile-development/core-expectations/README.md)
  - Concordar com um objetivo de sprint e como medir o progresso do sprint
  - Determinar a capacidade da equipe
  - Atribuir histórias de usuário ao sprint e dividir histórias de usuário em tarefas
  - Configurar limites de Trabalho em Andamento (WIP)
- [ ] [Decidir sobre frameworks de teste e discutir estratégias de teste](automated-testing/README.md)
  - Discutir o propósito e os objetivos dos testes e como medir a cobertura de teste
  - Concordar em como separar testes unitários de testes de integração, carga e fumaça
  - Projetar os primeiros casos de teste
- [ ] [Decidir sobre a nomenclatura das branches](source-control/naming-branches.md)
- [ ] [Discutir necessidades de segurança e verificar que segredos são mantidos fora do controle de versão](continuous-delivery/azure-devops/secret-management-per-branch.md)

### Dia 2

- [ ] [Configurar Controle de Fonte](source-control/README.md)
  - Concordar com [melhores práticas para commits](source-control/git-guidance/README.md#commit-best-practices)
- [ ] [Configurar Integração Contínua básica com linters e testes automatizados](continuous-integration/README.md)
- [ ] [Configurar reuniões para Stand-ups Diários e decidir sobre um Líder de Processo](agile-development/core-expectations/README.md)
  - Discutir propósito, objetivos, participantes e orientações para facilitação
  - Discutir o momento e como executar um stand-up eficiente
- [ ] [Se o projeto tiver subequipes, configurar um Scrum de Scrums](agile-development/advanced-topics/effective-organization/scrum-of-scrums.md)

### Dia 3

- [ ] [Concordar com o estilo de código](code-reviews/README.md) e sobre [como atribuir Pull Requests](code-reviews/pull-requests.md)
- [ ] [Configurar Validação de Build para Pull Requests (2 revisores, linters, testes automatizados)](code-reviews/README.md) e concordar com [Definição de Concluído](agile-development/advanced-topics/team-agreements/definition-of-done.md)
- [ ] [Concordar com uma estratégia de Mesclagem de Código](source-control/merge-strategies.md) e atualizar o [CONTRIBUTING.md](resources/templates/CONTRIBUTING.md)
- [ ] [Concordar com frameworks e estratégias de registro e observabilidade](observability/README.md)

### Dia 4

- [ ] [Configurar Implantação Contínua](continuous-delivery/README.md)
  - Determinar quais ambientes são apropriados para esta solução
  - Para cada ambiente, discutir o propósito, quando a implantação deve ser acionada, aprovadores pré-implantação, aprovação para promoção.
- [ ] [Decidir sobre uma estratégia de versionamento](source-control/component-versioning.md)
- [ ] Concordar em como [Projetar um recurso e conduzir uma Revisão de Design](design/design-reviews/README.md)

### Dia 5

- [ ] Realizar uma Demonstração de Sprint
- [ ] [Conduzir uma Retrospectiva](agile-development/core-expectations/README.md)
  - Determinar participantes necessários, como capturar entrada (ferramentas) e resultado
  - Definir um cronograma e discutir facilitação, estrutura da reunião, etc.
- [ ] [Refinar o Backlog](agile-development/advanced-topics/backlog-management/README.md)
  - Determinar participantes necessários
  - Atualizar a [Definição de Pronto](agile-development/advanced-topics/team-agreements/definition-of-ready.md)
  - Atualizar estimativas e o documento [Estimativa](agile-development/core-expectations/README.md)
- [ ] [Enviar Feedback de Engenharia para problemas encontrados](engineering-feedback/README.md)
