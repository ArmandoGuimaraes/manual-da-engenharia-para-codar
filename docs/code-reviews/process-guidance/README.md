# Orientação do Processo

## Orientações Gerais

As revisões de código devem fazer parte do processo da equipe de engenharia de software, independentemente do modelo de desenvolvimento. Além disso, a equipe deve aprender a realizar revisões de forma oportuna. [Pull requests (PRs)](../pull-requests.md) deixados pendentes podem causar problemas adicionais de mesclagem e ficarem desatualizados, resultando em trabalho perdido. PRs qualificados devem refletir tarefas bem definidas e concisas, sendo, portanto, compactos em conteúdo. A revisão de uma única tarefa deve levar relativamente pouco tempo para ser concluída.

Para garantir que o processo de revisão de código seja saudável, inclusivo e atenda aos objetivos mencionados acima, considere seguir estas diretrizes:

- Estabeleça um [acordo de nível de serviço (SLA)](https://pt.wikipedia.org/wiki/Acordo_de_n%C3%ADvel_de_servi%C3%A7o) para revisões de código e inclua-o no acordo de trabalho de sua equipe.
- Embora os ambientes modernos de DevOps incorporem ferramentas para gerenciar PRs, pode ser útil rotular tarefas pendentes de revisão ou ter um local dedicado para elas no quadro de tarefas - [Personalize os quadros de tarefas do AzDO](../tools.md#task-boards)
- Na reunião diária de standup, verifique as tarefas pendentes de revisão e certifique-se de que possuam revisores atribuídos.
- Equipes juniores e equipes novas no processo podem considerar criar tarefas separadas para revisões junto com as próprias tarefas.
- Utilize ferramentas para simplificar o processo de revisão - [Ferramentas de revisão de código](../tools.md)
- Promova revisões de código inclusivas - [Inclusão na Revisão de Código](../inclusion-in-code-review.md)

## Medindo o Processo de Revisão de Código

Se a equipe perceber que as revisões de código estão demorando muito para serem mescladas e estão se tornando um obstáculo, considere as seguintes recomendações adicionais:

1. Meça o tempo médio necessário para mesclar um PR por ciclo de sprint.
1. Revise durante a retrospectiva como o tempo para mesclar pode ser melhorado e priorizado.
1. Avalie o tempo para mesclar ao longo dos sprints para ver se o processo está melhorando.
1. Lembre os aprovadores necessários diretamente como lembrete.

## As revisões de código não devem incluir muito código

É fácil dizer que um desenvolvedor pode revisar algumas centenas de linhas de código, mas quando o código ultrapassa uma certa quantidade de linhas, a eficácia na descoberta de defeitos diminuirá, e há menos chances de fazer uma boa revisão. Não se trata de estabelecer um limite de linhas de código, mas sim de usar o bom senso. Quanto mais código houver para revisar, maiores são as chances de deixar um erro passar despercebido. Consulte [Orientação de Tamanho de PR](../pull-requests.md#size-guidance).

## Automatize sempre que for razoável

Use automação (linting, análise de código etc.) para evitar a necessidade de "[nits](https://pt.wikipedia.org/wiki/Nitpicking)" e permita que o revisor se concentre mais nos aspectos funcionais do PR. Configurando compilações automatizadas, testes e verificações (algo alcançável no [processo de CI](../../continuous-integration/README.md)), as equipes podem economizar tempo dos revisores humanos e permitir que eles se concentrem em áreas como design e funcionalidade para uma avaliação adequada. Isso garantirá maiores chances de sucesso, pois a equipe está se concentrando no que realmente importa.

## Orientações Específicas por Função

- [Orientações para o Autor](author-guidance.md)
- [Orientações para o Revisor](reviewer-guidance.md)
