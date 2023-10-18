# Source Control

Existem muitas opções ao trabalhar com Source Control. Na [ISE](../ISE.md), usamos o [AzureDevOps](https://azure.microsoft.com/en-us/services/devops/) para repositórios privados e o [GitHub](https://github.com/) para repositórios públicos.

## Seções dentro do Source Control

* [Estratégias de Mesclagem](merge-strategies.md)
* [Nomenclatura de Ramificação](naming-branches.md)
* [Versionamento](component-versioning.md)
* [Trabalhando com Segredos](secrets-management.md)
* [Orientação do Git](git-guidance/README.md)

## Objetivo

* Seguir as melhores práticas da indústria para trabalhar em equipes geodistribuídas que incentivam contribuições de toda a [ISE](../ISE.md), bem como da comunidade de Código Aberto mais ampla
* Melhorar a qualidade do código, garantindo revisões antes da mesclagem nas branches principais
* Melhorar a rastreabilidade de recursos e correções por meio de um histórico de commits limpo

## Orientações Gerais

A consistência é importante, portanto, concorde com a abordagem como equipe antes de começar a codificar. Trate isso como uma decisão de design, inclua uma proposta de design e revisão, da mesma forma como você documentaria todas as decisões de design (consulte [Acordos de Trabalho](../agile-development/advanced-topics/team-agreements/working-agreements.md) e [Revisões de Design](../design/design-reviews/README.md)).

## Criando um novo repositório

Ao criar um novo repositório, a equipe deve fazer pelo menos o seguinte:

* Concordar com a **branch**, **release** e **estratégia de mesclagem**
* Definir a estratégia de mesclagem ([linear ou não linear](merge-strategies.md))
* Bloquear a branch padrão e mesclar usando [pull requests (PRs)](../code-reviews/pull-requests.md)
* Concordar com a [nomenclatura de branches](naming-branches.md) (por exemplo, `user/your_alias/feature_name`)
* Estabelecer políticas de [branch/PR](../code-reviews/pull-requests.md)
* Para repositórios públicos, a branch padrão deve conter os seguintes arquivos:
  * [LICENSE](../resources/templates/LICENSE)
  * [README.md](../resources/templates/README.md)
  * [CONTRIBUTING.md](../resources/templates/CONTRIBUTING.md)

## Contribuindo para um repositório existente

Ao trabalhar em um projeto existente, faça `git clone` do repositório e certifique-se de entender a estratégia de branch, mesclagem e lançamento da equipe (por exemplo, por meio do arquivo [CONTRIBUTING.md do projeto](https://blog.github.com/2012-09-17-contributing-guidelines/)).

## Ambientes DevOps Mistos

Na maioria dos casos, ter um único ambiente DevOps hospedado (ou seja, Azure DevOps) é o caminho preferido, mas há momentos em que um ambiente DevOps misto (ou seja, Azure DevOps para rastreamento de itens de trabalho ágil e GitHub para Controle de Origem) é necessário devido a requisitos do cliente. Ao trabalhar em um ambiente misto:

* Marque manualmente os PRs nos itens de trabalho
* Certifique-se de que o escopo dos itens de trabalho/tarefas esteja alinhado com os PRs

## Recursos

* [Git](https://git-scm.com/) `--local-branching-on-the-cheap`
* [Azure DevOps](https://azure.microsoft.com/pt-br/services/devops/)
* [Detalhes do Git da ISE](git-guidance/README.md): detalhes sobre como usar o Git como parte de um projeto da [ISE](../ISE.md).
* [GitHub - Removendo dados sensíveis de um repositório](https://help.github.com/articles/removing-sensitive-data-from-a-repository/)
* [Curso Pluralsight sobre Como o Git Funciona](https://www.pluralsight.com/courses/how-git-works)
* [Curso Pluralsight sobre Domínio do Git](https://www.pluralsight.com/courses/master-git)
