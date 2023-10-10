# Expectativas Centrais do Agile

Esta seção contém as expectativas centrais para as práticas ágeis na ISE:

- Deve permanecer concisa e vincular à documentação externa para detalhes.
- Cada seção contém uma lista de expectativas centrais e sugestões:
  - **Expectativas centrais** são o que cada equipe de desenvolvimento deve almejar.
  - **Sugestões** *não são* expectativas. São nossa experiência aprendida para atender a essas expectativas e podem ser adotadas e modificadas para se adequar ao projeto.

**Notas:**

- Preferimos o uso de "líder de processo" em vez de "scrum master". Descreve o mesmo papel.
- **"Equipe"**, neste documento, refere-se a toda a equipe trabalhando em um projeto (equipe de desenvolvimento, líder de desenvolvimento, gerente de projeto, etc.).
- Seguimos os princípios ágeis e geralmente [Scrum](https://www.scrum.org/resources/what-is-scrum)

## Expectativas Gerais para um Projeto

- A equipe é previsível em sua entrega.
- A equipe faz ajustes relevantes e os compartilha de forma transparente.
- Papéis e responsabilidades são esclarecidos e acordados antes do início do projeto.
- A equipe está impulsionando a melhoria contínua de seu próprio processo para atender às expectativas centrais e aumentar o sucesso do projeto.

## Expectativas e Sugestões Centrais

### Sprints

___

**Expectativas:**

- A estrutura do sprint oferece oportunidades frequentes para feedback e ajuste no contexto de projetos relativamente pequenos.
- As cerimônias do sprint devem ser planejadas para acomodar os horários de trabalho da equipe e levar em consideração restrições de tempo rígidas e flexíveis.

**Sugestões:**

- O sprint começa no dia 1: a criação do plano de jogo, a revisão do plano de jogo e o compartilhamento estão incluídos nos sprints e devem ser refletidos no backlog.
- Defina um objetivo de sprint que será usado para determinar o sucesso do sprint.
- Nota: Os sprints geralmente duram 1 semana para aumentar o número de oportunidades para ajustes. E minimizar o risco de perder o objetivo do sprint.

### Estimativa

___

**Expectativas:**

- A estimativa suporta a previsibilidade do trabalho e entrega da equipe.
- A estimativa reforça o valor da responsabilidade para a equipe.
- O processo de estimativa é aprimorado ao longo do tempo e discutido regularmente.
- A estimativa é inclusiva dos diferentes indivíduos na equipe.

**Sugestões:**
A estimativa aproximada geralmente é feita para um SE 2 genérico de desenvolvimento.

- Exemplo 1
  - A equipe usa tamanhos de camiseta (P, M, G, GG) e concorda antecipadamente qual tamanho se encaixa em um sprint.
  - Neste exemplo: P, M se encaixam em um sprint, G, GG são muito grandes para um sprint e precisam ser divididos / refinados
  - O líder de desenvolvimento, com o apoio da equipe, estima aproximadamente quantas histórias P e M podem ser feitas nos primeiros sprints
  - Essa estimativa aproximada é refinada ao longo do tempo e usada como uma entrada para o planejamento futuro do sprint e para ajustar a previsão da data de término do projeto
- Exemplo 2
  - A equipe usa um único indicador: "essa história cabe em um sprint?", se não, a história precisa ser dividida
  - O líder de desenvolvimento, com o apoio da equipe, estima aproximadamente quantas histórias podem ser feitas nos primeiros sprints
  - Quantas histórias são feitas em cada sprint, em média, é usado como uma entrada para o planejamento futuro do sprint e como um indicador para ajustar a previsão da data de término do projeto
- Exemplo 3
  - A equipe faz o planejamento do poker e estima em pontos de história
  - Os pontos de história são usados aproximadamente para estimar quanto pode ser feito no próximo sprint
  - O líder de desenvolvimento e o TPM usam os sprints passados e a velocidade observada para ajustar a previsão da data de término do projeto

- Outras considerações
  - Estimar histórias usando pontos de história em projetos menores nem sempre fornece o valor que forneceria em maiores.
  - Evite converter pontos de história ou tamanhos de camiseta para dias.
  - Medir a precisão da estimativa:
    - Colete dados para monitorar a precisão da estimativa e a conclusão do sprint ao longo do tempo para impulsionar melhorias.
    - Use o objetivo do sprint para entender se a estimativa estava correta. Se o objetivo do sprint for atingido: mais alguma coisa importa?
  - Práticas de Scrum: Embora o Scrum não prescreva como dimensionar o trabalho, o Scrum Profissional é tendencioso contra a estimativa absoluta (horas, pontos de função, dias ideais, etc.) e em direção ao dimensionamento relativo.
    - Planejamento do Poker: é uma técnica colaborativa para atribuir tamanho relativo. Os desenvolvedores podem escolher quaisquer unidades que desejarem - pontos de história e tamanhos de camiseta são exemplos de unidades.
    - PBIs do 'Mesmo Tamanho' é uma abordagem de estimativa relativa que envolve dividir os itens em partes pequenas o suficiente para que sejam mais ou menos do mesmo tamanho. A velocidade pode ser entendida como uma contagem de PBIs; isso às vezes é usado por equipes que fazem entrega contínua.
    - PBIs do 'Tamanho Certo' é uma abordagem de estimativa relativa que envolve dividir as coisas em partes pequenas o suficiente para entregar valor em um determinado período de tempo (ou seja, chegar a Feito até o final de um Sprint). Isso às vezes está associado a equipes que utilizam fluxo para previsão. As equipes usam dados históricos para determinar se acham que podem concluir o PBI dentro do nível de confiança que seus dados históricos dizem que normalmente concluem um PBI.

**Links:**

- [A coisa mais importante que você está perdendo sobre estimativa](https://www.scrum.org/resources/blog/most-important-thing-you-are-missing-about-estimation)

### Planejamento do Sprint

___

**Expectativas:**

- O planejamento su

porta princípios de Diversidade e Inclusão e oferece oportunidades iguais.
- O Planejamento define como o trabalho será concluído no sprint.
- As histórias se encaixam em um sprint e são [projetadas](https://github.com/microsoft/code-with-engineering-playbook/tree/main/docs/design/design-reviews) e [prontas](../advanced-topics/team-agreements/definition-of-ready.md) antes do planejamento.

**Sugestões:**

**Objetivo do Sprint:**

Considere definir um objetivo de sprint ou uma lista de objetivos para cada sprint. Objetivos de sprint eficazes são uma lista concisa de itens em forma de tópicos. Um objetivo de sprint pode ser criado primeiro e usado como uma entrada para escolher as histórias para o sprint. Um objetivo de sprint também pode ser criado a partir da lista de histórias que foram escolhidas para o Sprint.

O objetivo do sprint pode ser usado:

- No final de cada reunião de stand-up, para lembrar a estrela do norte para o Sprint e ajudar todos a dar um passo atrás
- *Durante a revisão do sprint ("o objetivo foi alcançado?", "Se não, por quê?")

*Nota: Uma maneira simples de definir um objetivo de sprint é criar uma história de usuário em cada backlog de sprint e nomeá-la "Objetivo do Sprint XX". Você pode adicionar os tópicos na descrição.*

**Histórias:**

- Exemplo 1 - Preparando com antecedência:
  - O líder de desenvolvimento e o proprietário do produto planejam tempo para preparar o backlog do sprint antes do planejamento do sprint.
  - O líder de desenvolvimento usa sua experiência (passada e no projeto atual) e a estimativa feita para essas histórias para avaliar quantas devem estar no sprint.
  - O líder de desenvolvimento pede à equipe inteira para olhar o backlog provisório do sprint antes do planejamento do sprint.
  - O líder de desenvolvimento atribui histórias a desenvolvedores específicos após confirmar com eles que faz sentido
  - Durante a reunião de planejamento do sprint, a equipe revisa o objetivo do sprint e as histórias. Todos confirmam que entendem o plano e acham que é razoável.
- Exemplo 2 - Construindo durante a reunião de planejamento:
  - O proprietário do produto garante que os itens de maior prioridade do backlog do produto sejam refinados e estimados seguindo o processo de estimativa da equipe.
  - Durante a reunião de planejamento do Sprint, o proprietário do produto descreve cada história, uma a uma, começando pela de maior prioridade.
  - Para cada história, o líder de desenvolvimento e a equipe confirmam que entendem o que precisa ser feito e adicionam a história ao backlog do sprint.
  - A equipe continua considerando mais histórias até um ponto em que concordam que o backlog do sprint está cheio. Isso deve ser informado pela estimativa, experiência passada do desenvolvedor e experiência passada neste projeto específico.
  - As histórias são atribuídas durante a reunião de planejamento:
    - Opção 1: O líder de desenvolvimento faz sugestões sobre quem poderia trabalhar em cada história. Cada engenheiro concorda ou discute, se necessário.
    - Opção 2: A equipe revisa cada história e os engenheiros voluntários selecionam a que desejam ser atribuídos. (*Nota*: esta opção pode causar problemas com as primeiras expectativas centrais. Quem trabalha no quê? Em última análise, é responsabilidade do líder de desenvolvimento garantir que cada engenheiro tenha a oportunidade de trabalhar no que faz sentido para seu crescimento.)

**Tarefas:**

- Exemplos de abordagens para criação e atribuição de tarefas:
  - As histórias são divididas em tarefas com antecedência pelo líder de desenvolvimento e atribuídas antes/durante o planejamento do sprint aos engenheiros.
  - As histórias são atribuídas a engenheiros mais experientes, que são responsáveis por dividi-las em tarefas.
  - As histórias são divididas em tarefas durante a reunião de planejamento do Sprint pela equipe inteira.
  - *Nota*: Dependendo da senioridade da equipe, considere dividir em tarefas antes do planejamento do sprint. Isso pode ajudar a sair do planejamento do sprint com todo o trabalho atribuído. Também aumenta a clareza para engenheiros júnior.

**Links:**

- [Definição de Pronto](../advanced-topics/team-agreements/definition-of-ready.md)
- [Modelo de Objetivo de Sprint](https://www.scrum.org/resources/blog/five-questions-sprint-goal)

*Notas: A autoatribuição por membros da equipe pode dar uma sensação de justiça na forma como o trabalho é dividido na equipe. Às vezes, isso acaba não sendo o caso, pois pode dar vantagem às vozes mais altas ou mais experientes na equipe. Os indivíduos também tendem a permanecer em sua zona de conforto,

 o que pode não ser a abordagem certa para o próprio crescimento.*

### Backlog

___

**Expectativas:**

- As histórias de usuário têm critérios de aceitação claros e definição de pronto.
- As atividades de design são planejadas como parte do backlog (um design para uma história que precisa dele deve ser feito antes de ser adicionado em um Sprint).

**Sugestões:**

- Considere o refinamento do backlog como uma atividade contínua, que se expande fora da típica "Reunião de Refinamento".
- A dívida técnica é principalmente devida a atalhos feitos na implementação, bem como ao custo de manutenção futuro como resultado natural da melhoria contínua. Atalhos devem geralmente ser evitados. Em alguns casos raros em que ocorrem, priorizar e planejar atividades de melhoria para reduzir essa dívida em um momento posterior é a abordagem recomendada.

### Retrospectivas

___

**Expectativas:**

- As retrospectivas levam a itens acionáveis que ajudam a crescer as práticas de engenharia da equipe. Esses itens estão no backlog, atribuídos e priorizados para serem corrigidos até uma data acordada (o padrão sendo a próxima retrospectiva).
- É usado para fazer as perguntas difíceis ("geralmente não terminamos o que planejamos, vamos falar sobre isso") quando necessário.

**Sugestões:**

- Considere [outros formatos de retro](https://www.goodreads.com/book/show/721338.Agile_Retrospectives) disponíveis fora de Mad Sad Glad.
  - Coletar dados: Triple Nickels, Linha do Tempo, Mad Sad Glad, Radar da Equipe
  - Gerar Insights: 5 Porquês, Espinha de Peixe, Padrões e Mudanças
- Considere definir uma área de foco para a retro.
- Agende tempo suficiente para garantir que você possa ter a conversa de que precisa para obter o plano correto de ação e melhorar a forma como você trabalha.
- Traga um facilitador neutro para retrospectivas de projeto ou retrospectivas que introspectam após um período difícil.

**Use as seguintes técnicas de retrospectivas para abordar tendências específicas que possam estar surgindo em um projeto:**

**5 porquês:**

Se uma equipe está enfrentando um problema e não tem certeza da causa raiz exata, o exercício dos 5 porquês, retirado do setor de análise de negócios, pode ajudar a chegar ao fundo dele. Por exemplo, se uma equipe não consegue chegar a *Pronto* a cada Sprint, isso iria para o topo do quadro branco. A equipe então pergunta por que esse problema existe, escrevendo essa resposta na caixa abaixo. Em seguida, a equipe pergunta por que novamente, mas desta vez em resposta ao *porquê* que acabaram de identificar. Continue esse processo até que a equipe identifique uma causa raiz real, que geralmente se torna aparente em cinco etapas.

**Processos, ferramentas, indivíduos, interações e a Definição de Pronto:**

Esta abordagem incentiva os membros da equipe a pensar de forma mais ampla. Peça aos membros da equipe para identificar o que está indo bem e ideias para melhoria nas categorias de processos, ferramentas, indivíduos/interações e a Definição de Pronto. Em seguida, peça aos membros da equipe para votar em quais ideias de melhoria focar durante o próximo Sprint.

**Foco:**

Esta técnica de retrospectiva incorpora o conceito de visão. Usando esta técnica, você pergunta aos membros da equipe para onde gostariam de ir? Decida como a equipe deve ser em 4 semanas e, em seguida, pergunte o que está impedindo-os de chegar lá e como podem resolver o impedimento. Se você está focando em melhorias específicas, você pode usar esta técnica para uma ou duas retrospectivas seguidas, para que a equipe possa ver o progresso ao longo do tempo.

### Demonstração do Sprint

___

**Expectativas:**

- Cada sprint tem demonstrações que ilustram o objetivo do sprint e como ele se encaixa no objetivo do projeto.

**Sugestões:**

- Considere não pré-gravar demonstrações de sprint com antecedência. Você pode gravar a reunião de demonstração e arquivá-la.
- Uma demonstração não precisa ser sobre código em execução. Pode ser mostrando documentação que foi escrita.

## Stand-up

___

**Expectativas:**

- O stand-up é executado de forma eficiente.
- O stand-up ajuda a equipe a entender o que foi feito, o que será feito e quais são os bloqueadores.
- O stand-up ajuda a equipe a entender se eles atenderão ao objetivo do sprint ou não.

**Sugestões:**

- Mantenha o stand-up curto e eficiente. Reserve as conversas mais longas para uma seção de estacionamento ou para uma conversa que será planejada posteriormente.
- Faça stand-ups diários: 15 minutos de stand-up e 15 minutos de estacionamento.
- Se alguém não puder comparecer ao stand-up excepcionalmente: Peça-lhes para fazer um stand-up escrito com antecedência.
- Os stand-ups devem incluir todos os envolvidos no projeto, incluindo o cliente.
- Projetos com fusos horários amplamente divergentes devem ser evitados, se possível, mas se você estiver em um, você deve adaptar os stand-ups para atender às necessidades e restrições de tempo de todos os membros da equipe.

### Documentação

- [O que é Scrum?](https://www.scrum.org/resources/what-is-scrum)
- [Retrospectiva Ágil: Tornando Boas Equipes Ótimas](https://www.goodreads.com/book/show/721338.Agile_Retrospectives)
- [Histórias de Usuários Aplicadas: Para Desenvolvimento de Software](https://www.goodreads.com/book/show/3856.User_Stories_Applied)
- [Scrum Essencial: Um Guia Prático para o Processo Ágil Mais Popular](https://www.goodreads.com/book/show/13663747-essential-scrum)
