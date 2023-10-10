# Seções de um Acordo de Trabalho

Um acordo de trabalho é um documento ou um conjunto de documentos que descrevem como trabalhamos juntos como uma equipe e quais são nossas expectativas e princípios.

O acordo de trabalho é criado pela equipe no início do projeto e é armazenado no repositório para que esteja prontamente disponível para todos que trabalham no projeto.

A seguir estão exemplos de seções e pontos que podem fazer parte de um acordo de trabalho, mas cada equipe deve compor o seu próprio e ajustar horários, canais de comunicação, políticas de nomeação de branches, etc., para atender às necessidades da equipe.

## Geral

- Trabalhamos como uma única equipe em direção a um objetivo comum e escopo claro
- Certificamo-nos de que a voz de todos seja ouvida e escutada
- Mostramos igual respeito a todos os membros da equipe
- Trabalhamos como uma equipe para ter expectativas comuns para a entrega técnica que são documentadas em um [Manifesto da Equipe](team-manifesto.md).
- Certificamo-nos de disseminar nossa expertise e habilidades na equipe, para que nenhuma única pessoa seja a única responsável por uma habilidade
- Todos os horários abaixo estão listados em CET

## Comunicação

- Comunicamos todas as informações relevantes para a equipe através do canal de Equipes do Projeto
- Adicionamos todos os [spikes técnicos](../../../design/design-reviews/recipes/technical-spike.md), [estudos de trade-off](../../../design/design-reviews/trade-studies/README.md) e outra documentação técnica ao repositório do projeto através de [revisões de design assíncronas em PRs](../../../design/design-reviews/recipes/async-design-reviews.md)

## Equilíbrio entre Trabalho e Vida Pessoal

- Nosso horário de expediente, quando podemos esperar colaborar via Microsoft Teams, telefone ou presencialmente, é de segunda a sexta-feira, das 10h às 17h
- Não somos obrigados a responder e-mails após as 18h, nos fins de semana ou quando estamos de férias ou feriados.
- Trabalhamos em diferentes fusos horários e respeitamos isso, especialmente ao marcar reuniões recorrentes.
- Gravamos reuniões sempre que possível, para que os membros da equipe que não puderam participar ao vivo possam ouvir mais tarde.

## Qualidade e não Quantidade

- Concordamos com uma [Definição de Concluído](definition-of-done.md) para nossas histórias de usuário e sprints e vivemos de acordo com ela.
- Seguimos as melhores práticas de engenharia, como o [Code With Engineering Playbook](https://github.com/microsoft/code-with-engineering-playbook)

## Ritmo do Scrum

| Atividade                                             | Quando                 | Duração  | Quem         | Responsável | Objetivo                                                                  |
|-------------------------------------------------------|------------------------|----------|--------------|-------------|---------------------------------------------------------------------------|
| [Reunião Diária do Projeto](../../core-expectations/README.md) | Ter-Sex 9h              | 15 min   | Todos        | Líder de Processo | O que foi realizado, próximos passos, bloqueios                            |
| Demonstração do Sprint                                | Segunda 9h             | 1 hora   | Todos        | Líder de Desenvolvimento | Apresentar o trabalho realizado e aprovar a conclusão da história de usuário |
| [Retrospectiva do Sprint](../../core-expectations/README.md)  | Segunda 10h            | 1 hora   | Todos        | Líder de Processo | A equipe de desenvolvimento compartilha aprendizados e o que pode ser melhorado |
| [Planejamento do Sprint](../../core-expectations/README.md)   | Segunda 11h            | 1 hora   | Todos        | PO          | Dimensionar e planejar histórias de usuário para o sprint                   |
| Criação de Tarefas                                    | Após o Planejamento do Sprint | -     | Equipe de Desenvolvimento | Líder de Desenvolvimento | Criar tarefas para esclarecer e determinar a velocidade                    |
| [Refinamento do Backlog](../backlog-management/README.md)     | Quarta 14h             | 1 hora   | Líder de Desenvolvimento, PO | PO  | Preparar para o próximo sprint e garantir que as histórias estejam prontas para o próximo sprint. |

## Líder de Processo

O Líder de Processo é responsável por liderar quaisquer práticas de scrum ou ágeis para permitir que o projeto avance.

- Facilitar reuniões diárias e responsabilizar a equipe pela presença e participação.
- Manter a reunião em movimento conforme descrito na página [Reunião Diária do Projeto](../../core-expectations/README.md).
- Certificar-se de que todas as ações estão documentadas e garantir que cada uma tenha um responsável e uma data de vencimento e rastrear as questões em aberto.
- Anotações conforme necessário após o planejamento/reuniões diárias.
- Certificar-se de que os itens são movidos para o estacionamento e garantir o acompanhamento posterior.
- Manter um local mostrando o trabalho e o status da equipe e remover impedimentos que estão bloqueando a equipe.
- Responsabilizar a equipe pelos

 resultados de forma solidária.
- Certificar-se de que a documentação do projeto e do programa estão atualizadas.
- Garantir o rastreamento/seguimento de ações da retrospectiva (planejamento de iteração e liberação) e das reuniões diárias.
- Facilitar a retrospectiva do sprint.
- Treinar o Product Owner e a equipe no processo, conforme necessário.

## Gerenciamento de Backlog

- Trabalhamos juntos em uma [Definição de Pronto](definition-of-ready.md) e todas as histórias de usuário atribuídas a um sprint precisam seguir isso
- Comunicamos o que estamos trabalhando através do quadro
- Atribuímos a nós mesmos uma tarefa quando estamos prontos para trabalhar nela (não antes) e a movemos para ativo
- Capturamos qualquer trabalho que fazemos relacionado ao projeto em uma história de usuário/tarefa
- Fechamos nossas tarefas/histórias de usuário apenas quando estão concluídas (conforme descrito na [Definição de Concluído](definition-of-done.md))
- Trabalhamos com o PM se quisermos adicionar uma nova história de usuário ao sprint
- Se adicionarmos novas tarefas ao quadro, certificamo-nos de que ela corresponde aos critérios de aceitação da história de usuário (para evitar o aumento do escopo).
  Se não corresponder aos critérios de aceitação, devemos discutir com o PM para ver se precisamos de uma nova história de usuário para a tarefa ou se devemos ajustar os critérios de aceitação.

## Gerenciamento de Código

- Seguimos a convenção de nomenclatura de branch do git flow para branches e identificamos o número da tarefa, por exemplo, `feature/123-add-working-agreement`
- Mesclamos todo o código em branches principais através de PRs
- Todos os PRs são revisados por uma pessoa de [Nome do Cliente/Parceiro] e uma da Microsoft (para transferência de conhecimento e para garantir que os padrões de código e segurança sejam atendidos)
- Sempre revisamos os PRs existentes antes de começar a trabalhar em uma nova tarefa
- Olhamos os PRs abertos no final da reunião diária para garantir que todos os PRs tenham revisores.
- Tratamos a documentação como código e aplicamos os mesmos [padrões ao Markdown](../../../code-reviews/recipes/markdown.md) como código.
