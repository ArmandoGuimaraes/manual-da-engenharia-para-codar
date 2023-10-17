# Itens de Trabalho

Embora muitas equipes possam trabalhar com uma lista plana de itens, às vezes é útil agrupar itens relacionados em uma estrutura hierárquica. Você pode usar portfólios de backlog para trazer mais ordem ao seu backlog.

**Processo Ágil** hierarquia de itens de backlog de trabalho:

![artefatos-ageis](./images/agile-artifacts.png)

Hierarquia de itens de backlog de trabalho do **Scrum**:

![artefatos-scrum](./images/scrum-artifacts.png)

Os bugs podem ser definidos no mesmo nível das Histórias de Usuário / Itens de Backlog do Produto ou Tarefas.

## Épicos e Features

As histórias de usuário / itens de backlog do produto são agrupados em **Features**, que normalmente representam um entregável despachável que aborda uma necessidade do cliente, por exemplo, "Adicionar carrinho de compras". E as Features são agrupadas em **Épicos**, que representam uma iniciativa de negócios a ser realizada, por exemplo, "Aumentar o engajamento do cliente". Leve isso em consideração ao nomeá-los.

Cada Feature ou Épico deve incluir o máximo de detalhes de que a equipe precisa para:

- Compreender o escopo.
- Estimar o trabalho necessário.
- Desenvolver testes.
- Garantir que o produto final atenda aos critérios de aceitação.

Detalhes que devem ser adicionados:

- *Área de Valor*: Negócios (fornece valor diretamente ao cliente) vs. Arquitetural (serviços técnicos para implementar recursos de negócios).
- *Esforço / Pontos de História / Tamanho*: Estimativa relativa da quantidade de trabalho necessária para concluir o item.
- *Valor de Negócio*: Prioridade do item em comparação com outros itens do mesmo tipo.
- *Criticidade em Tempo*: Valores mais altos indicam que um item é mais crítico em termos de tempo do que itens com valores mais baixos.
- *Data-Alvo* pela qual a feature deve ser implementada.

Você pode usar tags de itens de trabalho para suportar consultas e filtragem.

## Histórias de Usuário / Itens de Backlog do Produto

Cada História de Usuário / Item de Backlog do Produto deve ter um tamanho de modo que possa ser concluída dentro de um sprint.

Você deve adicionar os seguintes detalhes aos itens:

- *Título*: Normalmente expresso como "Como uma [persona], eu quero [realizar uma ação], para que [possa alcançar um resultado final]".
- *Descrição*: Fornecer detalhes suficientes para criar uma compreensão compartilhada do escopo e apoiar os esforços de estimativa. Concentre-se no usuário, no que eles desejam realizar e por quê. Não descreva como desenvolver o produto. Forneça detalhes suficientes para que a equipe possa escrever tarefas e casos de teste para implementar o item.
  - Inclua Avaliações de Design.
- *Critérios de Aceitação*: Defina o que significa "Concluído".
- *Atividade*: Implantação, Design, Desenvolvimento, Documentação, Requisitos, Testes.
- *Esforço / Pontos de História / Tamanho*: Estimativa relativa da quantidade de trabalho necessária para concluir o item.
- *Valor de Negócio*: Prioridade do item em comparação com outros itens do mesmo tipo.
- *Estimativa Original*: A quantidade de trabalho estimada necessária para concluir uma tarefa.

Lembre-se de usar a seção de *Discussão* dos itens para acompanhar comentários relacionados e mencionar indivíduos, grupos, itens de trabalho ou pull requests quando necessário.

## Tarefas

Cada Tarefa deve ter um tamanho para que possa ser concluída em um dia.

Pelo menos, você deve adicionar os seguintes detalhes aos itens:

- *Título*.
- *Descrição*: Fornecer detalhes suficientes para criar uma compreensão compartilhada do escopo. Qualquer desenvolvedor deve ser capaz de pegar o item e saber o que precisa ser implementado.
  - Inclua Avaliações de Design.
- Referência para o *branch* de trabalho no repositório de código relacionado.

Lembre-se de usar a seção de Discussão das tarefas para acompanhar comentários relacionados.

## Bugs

Você deve usar bugs para capturar tanto o problema inicial quanto as descobertas em curso.

Pelo menos, você deve adicionar os seguintes detalhes aos itens de bug:

- *Título*.
- *Descrição*.
- *Passos para Reproduzir*.
- *Informações do Sistema* / *Encontrado na Build*: Configuração de software e sistema relevante para o bug e testes a serem aplicados.
- *Critérios de Aceitação*: Critérios a serem atendidos para que o bug possa ser fechado.
- *Integrado na Build*: Nome da build que incorpora o código que corrige o bug.
- *Prioridade*:
  - 1: O produto não deve ser lançado sem a resolução bem-sucedida do item de trabalho. O bug deve ser tratado o mais rápido possível.
  - 2: O produto não deve ser lançado sem a resolução bem-sucedida do item de trabalho, mas não precisa ser tratado imediatamente.
  - 3: A resolução do item de trabalho é opcional com base em recursos, tempo e risco.
- *Gravidade*:
  - 1 - Crítico: Deve ser corrigido. Não há métodos alternativos aceitáveis.
  - 2 - Alto: Considere a correção. Existe um método alternativo aceitável.
  - 3 - Médio: (Padrão).
  - 4 - Baixo.

## Problemas / Impedimentos

Não confunda com bugs. Eles representam atividades não planejadas que podem bloquear o trabalho de ser concluído. Por exemplo: ambiguidade de funcionalidade, problemas de pessoal ou recursos, problemas com ambientes ou outros riscos que afetam o escopo, qualidade ou cronograma.

Em geral, você vincula esses itens a histórias de usuário ou outros itens de trabalho.

## Ações de Retrospectivas

Após uma retrospectiva, todas as ações que exigem trabalho devem ser rastreadas com sua própria Tarefa ou Problema / Impedimento. Esses itens podem ser desvinculados (sem vínculo com o item de backlog pai

 ou história de usuário).

## Informações Relacionadas

- [Melhores práticas para gerenciamento de projetos ágeis - Azure Boards | Microsoft Docs](https://learn.microsoft.com/azure/devops/boards/best-practices-agile-project-management?view=azure-devops&tabs=agile-process).
- [Definir features e épicos, organizar itens de backlog - Azure Boards | Microsoft Docs](https://learn.microsoft.com/azure/devops/boards/backlogs/define-features-epics?view=azure-devops&tabs=scrum-process).
- [Crie seu backlog de produto - Azure Boards | Microsoft Docs](https://learn.microsoft.com/azure/devops/boards/backlogs/create-your-backlog?view=azure-devops&tabs=agile-process).
- [Adicionar tarefas para apoiar o planejamento de sprint - Azure Boards | Microsoft Docs](https://learn.microsoft.com/azure/devops/boards/sprints/add-tasks?view=azure-devops).
- [Definir, capturar, triar e gerenciar bugs ou defeitos de código - Azure Boards | Microsoft Docs](https://learn.microsoft.com/azure/devops/boards/backlogs/manage-bugs?view=azure-devops).
- [Adicionar e gerenciar problemas ou impedimentos - Azure Boards | Microsoft Docs](https://learn.microsoft.com/azure/devops/boards/backlogs/manage-issues-impediments?view=azure-devops).
