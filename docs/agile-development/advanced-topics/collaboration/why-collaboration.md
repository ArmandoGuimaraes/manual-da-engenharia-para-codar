# Por que Colaboração

## Por que a colaboração é importante

Em engajamentos, visamos ser altamente colaborativos porque, quando codificamos juntos, temos um melhor desempenho, uma maior velocidade de sprint e um maior grau de compartilhamento de conhecimento entre a equipe.

Existem dois padrões comuns que usamos para colaboração: Programação em Par e Enxameamento.

**Programação em Par** ("pairing") - dois engenheiros de software atribuídos a, e trabalhando em, uma única história compartilhada de cada vez durante o sprint. O Líder de Desenvolvimento atribui uma história de usuário a dois engenheiros - um engenheiro principal (dono da história) e um engenheiro secundário (par atribuído).

**Programação em Enxame** ("swarming") - três ou mais engenheiros de software colaborando em um item de alta prioridade para levá-lo à conclusão.

## Como programar em par

Como mencionado, cada história é intencionalmente atribuída a um par. O par atribuído pode estar no processo de aprimoramento de habilidades, no entanto, eles são parceiros iguais no esforço de desenvolvimento.
A seguir estão algumas diretrizes gerais para programação em par:

- Após a atribuição da história/item do backlog do produto (PBI), o par precisa ser deliberado sobre como definir o trabalho a ser concluído. Essa informação deve ser expressa claramente na descrição da história e nos critérios de aceitação. As expectativas sobre isso precisam ser comunicadas e acordadas por ambos os engenheiros e devem ser feitas antes de qualquer sessão de trabalho real.
- O dono da história e o par atribuído não dividem simplesmente o trabalho e sincronizam regularmente - eles trabalham ativamente juntos nas mesmas tarefas e podem compartilhar suas telas por meio de uma sessão online do Teams. Ferramentas colaborativas como [VS Live Share](https://visualstudio.microsoft.com/services/live-share/) podem ser preferíveis ao compartilhamento de telas. Nem toda colaboração precisa ser baseada em compartilhamento de tela.
- Durante as sessões colaborativas, um engenheiro fornece o ambiente de desenvolvimento enquanto o outro visualiza ativamente e comenta verbalmente.
- Os engenheiros trocam de lugar frequentemente de uma sessão para a próxima, para que todos tenham tempo no controle do teclado.
- Os engenheiros utilizam branches de recursos para a colaboração durante o desenvolvimento de cada história, para ter pequenos Pull Requests (PRs) (em oposição a um único PR gigante) no final do sprint.
- O código é enviado ao repositório por ambos os membros do par atribuído onde e quando faz sentido, à medida que as tarefas foram concluídas.
- O par atribuído é a voz que representa o par durante o standup diário, enquanto é apoiado pelo dono da história.
- Ter os nomes de ambos os indivíduos (dono e par atribuído) visíveis no PBI pode ser útil durante as cerimônias de sprint e levar a uma maior responsabilidade por parte do par atribuído. Um exemplo disso usando cartões do Azure DevOps pode ser encontrado [aqui](./add-pairing-field-azure-devops-cards.md).

## Por que a programação em par ajuda na colaboração

A programação em par ajuda na colaboração porque ambos os engenheiros compartilham igual responsabilidade por levar a história à conclusão. Este é um exercício mutuamente benéfico porque, enquanto o dono da história geralmente tem mais experiência para se apoiar, o par atribuído traz uma visão fresca que não é nublada pela repetição.

Alguns outros benefícios incluem:

- Menos defeitos e maior responsabilidade. Ter dois conjuntos de olhos permite que os engenheiros tenham mais oportunidades de detectar erros e de lembrar tarefas frequentemente esquecidas, como escrever testes unitários e de integração.
- A programação em par permite que engenheiros com diferentes experiências e conhecimentos aprendam um com o outro, colaborando e recebendo feedback em tempo real. Em vez de ter um engenheiro trabalhando sozinho em uma tarefa por longas horas e atingindo um ponto de isolamento, a programação em par permite que o par se atualize mutuamente.
- Mesmo algo tão simples quanto descrever o problema em voz alta pode ajudar a descobrir problemas ou bugs no código.
- A programação em par pode ajudar no brainstorming, bem como na validação de detalhes, como tornar os nomes de variáveis consistentes.

## Quando programar em enxame

É importante saber que nem todo PBI precisa usar enxameamento. Alguns sprints podem nem mesmo justificar o enxameamento.
Programar em enxame quando:

- O trabalho é complexo o suficiente para ter mentes coletivas colaborando (não porque a quantidade de trabalho é mais do que o que seria concluído em um sprint).
- A tarefa em que o enxame trabalha se tornou (ou está em perigo iminente de se tornar) um bloqueador para outras histórias.
- Um desconhecido é descoberto que precisa de um esforço colaborativo para formar uma decisão sobre como seguir em frente. O conhecimento

 e a experiência coletivos ajudam a mover a história mais rapidamente e, em última análise, produzem um código de melhor qualidade.
- Um conflito ou diferença de opinião não resolvida surge durante uma sessão de programação em par. Promova o trabalho para se tornar uma sessão de enxameamento para ajudar a resolver o conflito.

## Como programar em enxame

Assim que o par descobre que o PBI vai justificar o enxameamento, o par o traz para o resto da equipe (via estacionamento durante o stand-up ou de forma assíncrona). Os membros da equipe concordam ou se voluntariam para ajudar.

- O dono da história (ou par atribuído) envia um convite de chamada do Teams para as partes interessadas. Isso permite que o enxame tenha um tempo de foco dedicado, bloqueando o tempo nos calendários.
- Durante uma sessão de enxameamento, um engenheiro pode se ramificar se houver algo que precise ser tratado enquanto o enxame aborda o problema principal, depois se reconecta e relata. Isso permite que o enxame se concentre em um aspecto central e esteja todos na mesma página.
- A chamada do Teams é repetida até que uma resolução seja encontrada ou um caminho alternativo para seguir em frente seja formulado.

## Por que a programação em enxame ajuda na colaboração

- O enxameamento permite que o conhecimento e a expertise coletivos da equipe se unam de forma focada e unificada.
- Não só o enxameamento ajuda a fechar o item mais rapidamente, mas também ajuda a equipe a entender os pontos fortes e fracos uns dos outros.
- Permite que a equipe construa um maior nível de confiança e trabalhe como uma unidade coesa.

## Quando decidir enxamear, programar em par e/ou dividir

- Embora muito tempo possa ser gasto na programação em par, faz sentido dividir o trabalho quando as pessoas entendem como o trabalho será realizado, e o trabalho a ser feito é em grande parte prescritivo.
- Uma vez que a história foi conjuntamente distribuída por ambos os engenheiros, os engenheiros podem optar por abordar algumas tarefas separadamente e depois combinar o trabalho no final.
- A programação em par é mais útil quando os engenheiros não têm clareza perfeita sobre o que precisa ser feito ou como pode ser feito.
- O enxameamento é feito quando os dois engenheiros atribuídos à história precisam de uma placa de som adicional ou precisam de expertise que outros membros da equipe poderiam fornecer.

## Benefícios do aumento da colaboração

O compartilhamento de conhecimento e a união de engenheiros de ISE e clientes de uma forma "code-with" é um aspecto importante dos engajamentos de ISE. Isso aumenta tanto a capacidade de nossos clientes quanto de nossa equipe de ISE para construir no Azure. Somos responsáveis por demonstrar fundamentos de engenharia e deixar o cliente em um lugar melhor depois que nos desengajamos. Isso só pode acontecer se colaborarmos e nos envolvermos juntos como equipe. Além de melhorar a qualidade do software, isso também adiciona um aspecto social benéfico aos engajamentos.

## Recursos

- [Como adicionar um campo personalizado de programação em par nas Histórias de Usuário do Azure DevOps](./add-pairing-field-azure-devops-cards.md) - adicionando um campo personalizado do tipo _Identity_ no Azure DevOps para programação em par

- [Sobre Programação em Par - Martin Fowler](https://martinfowler.com/articles/on-pair-programming.html)

- [Lições práticas de Programação em Par](https://github.com/The-V8/pair-programming-sessions) - essas podem ser usadas (e adaptadas) para apoiar a introdução da programação em par em sua equipe (interna da MS ou incluindo clientes)

- [Programação em Par sem Esforço com GitHub Codespaces e VSCode](./pair-programming-tools.md)
