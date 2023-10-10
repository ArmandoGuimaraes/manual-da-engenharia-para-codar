# Definição de Pronto

Quando a equipe de desenvolvimento escolhe uma história de usuário do topo do backlog, a história precisa ter detalhes suficientes para estimar o trabalho necessário para completá-la dentro do sprint. Se tiver detalhes suficientes para a estimativa, ela está Pronta para ser desenvolvida.

> Se uma história de usuário não estiver Pronta no início do Sprint, aumenta a chance de que a história não será concluída no final deste sprint.

## O que é

*Definição de Pronto* é o acordo feito pela equipe Scrum sobre quão completa uma história de usuário deve estar para ser selecionada como candidata à estimativa no planejamento do sprint. Estes podem ser codificados como uma lista de verificação nas histórias de usuário usando [Modelos de Issue do GitHub](https://help.github.com/en/github/building-a-strong-community/configuring-issue-templates-for-your-repository) ou [Modelos de Item de Trabalho do Azure DevOps](https://learn.microsoft.com/en-us/azure/devops/boards/backlogs/work-item-template?view=azure-devops&tabs=browser).

Pode ser entendido como uma lista de verificação que ajuda o Proprietário do Produto a garantir que a história de usuário que escreveu contém todos os detalhes necessários para a equipe Scrum entender o trabalho a ser feito.

### Exemplos de itens da lista de verificação de pronto

* [ ] A descrição contém os detalhes, incluindo quaisquer valores de entrada necessários para implementar a história de usuário?
* [ ] A história de usuário tem critérios de aceitação claros e completos?
* [ ] A história de usuário aborda a necessidade de negócios?
* [ ] Podemos medir os critérios de aceitação?
* [ ] A história de usuário é pequena o suficiente para ser implementada em um curto período de tempo, mas grande o suficiente para fornecer valor ao cliente?
* [ ] A história de usuário está bloqueada? Por exemplo, ela depende de algum dos seguintes:
  * A conclusão de um trabalho não terminado
  * Um entregável fornecido por outra equipe (artefato de código, dados, etc...)

## Quem escreve

A lista de verificação de pronto pode ser escrita por um Proprietário do Produto em acordo com a equipe de desenvolvimento e o Líder do Processo.

## Quando a Definição de Pronto deve ser atualizada

Atualize ou altere a definição de pronto sempre que a equipe Scrum observar que há informações faltando nas histórias de usuário que impactam recorrentemente o planejamento.

## O que deve ser evitado

A lista de verificação de pronto deve conter itens que se aplicam de forma ampla. Não inclua itens ou detalhes que se aplicam apenas a uma ou duas histórias de usuário. Isso pode se tornar um excesso ao escrever as histórias de usuário.

## Como preparar histórias para estarem prontas

No caso de o trabalho de maior prioridade ainda não estar pronto, ainda pode ser possível fazer progresso. Aqui estão algumas estratégias que podem ajudar:

* Sessões de [Refinamento do Backlog](../backlog-management/README.md) são um bom momento para validar que histórias de usuário de alta prioridade são verificadas para ter uma descrição clara, critérios de aceitação e valor comercial demonstrável. Também é um bom momento para dividir grandes histórias que provavelmente não serão concluídas em um único sprint.
* Sessões de priorização são um bom momento para priorizar histórias de usuário que desbloqueiam outros trabalhos de alta prioridade bloqueados.
* Histórias de usuário bloqueadas muitas vezes podem ser divididas de forma a desbloquear uma parte do escopo original. Esta é uma boa maneira de fazer progresso, mesmo quando algum trabalho está bloqueado.
