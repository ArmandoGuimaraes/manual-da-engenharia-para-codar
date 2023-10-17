# Modelo de Estudo de Análise

Este modelo genérico pode ser usado em qualquer situação em que tenhamos um conjunto de requisitos que podem ser satisfeitos por várias soluções. Eles podem variar em escopo, desde a escolha de qual pacote de código aberto usar até projetos completos de arquitetura.

## Estudo de Análise/Design: {nome do estudo}

- **Conduzido por:** {Nomes das pessoas que podem responder a perguntas de acompanhamento e pelo menos um endereço de e-mail}
- **Item de Trabalho no Backlog:** {Link para o item de trabalho para fornecer mais contexto}
- **Sprint:** {Em qual sprint o estudo foi realizado? Inclua a data de início da sprint}
- **Decisão:** {Solução escolhida para prosseguir}
- **Tomadores de Decisão:**

**IMPORTANTE** Os designs devem ser concluídos dentro de uma sprint. A maioria dos designs se beneficiará da brevidade. Para fazer isso:

1. Limite o escopo do design.
1. Reduza a avaliação para 2 a 3 soluções.
1. Projete experimentos para coletar evidências o mais rápido possível.

## Visão Geral

Descrição do problema que estamos resolvendo. Isso deve incluir:

1. Suposições sobre o restante do sistema.
1. Restrições que se aplicam ao sistema, tanto de negócios quanto técnicas.
1. Requisitos para a funcionalidade que precisa ser implementada, incluindo possíveis entradas e saídas.
1. (opcional) Um diagrama mostrando as diferentes partes.

### Resultados Desejados

A seção a seguir deve estabelecer as capacidades desejadas da solução para que ela seja bem-sucedida. Isso pode ser feito respondendo diretamente às seguintes perguntas ou por meio de um link para um artefato relacionado (ou seja, PBI ou descrição de funcionalidade).

1. Aceitação: Quais capacidades devem ser demonstráveis para que um interessado aceite a solução?
1. Justificação: Como isso contribui para os objetivos gerais do projeto?

> **IMPORTANTE** Isso **não** se destina a definir resultados para a atividade de design em si. Destina-se a definir os resultados para a solução em processo de design.

Como mencionado na seção [Interface do Usuário](../../../user-interface-engineering/README.md), se o estudo de análise estiver analisando uma solução de desenvolvimento de aplicativo, use as "histórias de persona" para derivar resultados desejados. Por exemplo, se uma história de persona exemplifica um certo requisito de acessibilidade, o resultado desejado paralelo pode ser "O aplicativo deve ser acessível para pessoas com deficiência visual".

### Critérios de Avaliação

Os resultados devem ser condensados em um conjunto de "critérios de avaliação" que podemos classificar todas as soluções potenciais. Exemplos de critérios de avaliação:

- Funciona no Windows e Linux - Resposta binária
- Uso de recursos computacionais - Pode ser categorias que classificam efetivamente diferentes opções: Alto, Médio, Baixo
- Custo da solução - Um campo numérico estimado

A seção de resultados contém uma tabela que avalia cada solução em relação aos critérios de avaliação.

#### Métricas-Chave (Opcional)

Se disponível, descreva quais métricas mensuráveis são importantes para o sucesso da solução. Exemplos incluem, mas não se limitam a:

- Metas de desempenho e escala, como Requisições/Segundo, Latência e Tempo de resposta (em um determinado percentil).
- Orçamento de custo de consumo no Azure. Por exemplo, dadas certas utilizações, espera-se que a solução custe X dólares por mês.
- Disponibilidade de tempo de atividade de XX% ao longo de um período de tempo X.
- Consistência. Escritas disponíveis para leitura em X milissegundos.
- Objetivo de ponto de recuperação (RPO) e objetivo de tempo de recuperação (RTO).

#### Restrições (Opcional)

Se aplicável, descreva os limites dentro dos quais devemos projetar a solução. Isso pode ser pensado como a "caixa" na qual a equipe deve trabalhar. Essa caixa pode ser definida como:

- Tecnologias, serviços e linguagens com as quais uma organização se sente confortável operar/gerenciar.
- Dispositivos, sistemas operacionais e/ou navegadores que precisam ser suportados.
- Compatibilidade com vers

ões anteriores. Por exemplo, interfaces públicas consumidas por aplicativos de terceiros ou clientes não podem introduzir alterações incompatíveis.
- Integrações ou dependências com outros sistemas. Por exemplo, notificações push para aplicativos de cliente devem ser feitas por meio de um canal existente de websockets.

#### Acessibilidade

**Acessibilidade nunca é opcional**. A Microsoft fez um compromisso público de sempre produzir aplicativos acessíveis. Para mais informações, visite o [site oficial de acessibilidade da Microsoft](https://www.microsoft.com/accessibility) e leia a página [Acessibilidade](../../../accessibility/README.md).

Considere as seguintes perguntas ao determinar os requisitos de acessibilidade do aplicativo:

- O aplicativo atende aos padrões de acessibilidade da indústria?
- Os recursos de treinamento, suporte e documentação são acessíveis?
- O aplicativo foi projetado para ser inclusivo para pessoas com uma ampla gama de habilidades, idiomas e culturas?

## Hipóteses de Solução

Enumere as soluções que se acredita que proporcionarão os resultados desejados.

> NOTA: Limitar as soluções avaliadas a 2 ou 3 candidatos potenciais pode ajudar a gerenciar o tempo gasto na avaliação. Se houver mais de 3 candidatos, priorize os 3 principais, de acordo com o que a equipe considera os principais. Se apropriado, os candidatos eliminados podem ser mencionados para capturar o motivo de sua eliminação. Além disso, deve haver pelo menos duas opções comparadas, caso contrário, o estudo de análise não era necessário.

### {Solução 1} - Nome curto e facilmente reconhecível

Adicione uma descrição **breve** da solução e como ela é esperada para produzir os resultados desejados. Se apropriado, ilustrações/diagramas podem ser usados para reduzir a quantidade de explicação de texto necessária para descrever a solução.

> NOTA: Usar a linguagem no presente para descrever a solução pode ajudar a evitar confusão entre o estado atual e o estado futuro. Por exemplo, use "Esta solução funciona fazendo..." em vez de "Esta solução funcionaria fazendo...".

Cada seção de solução deve conter o seguinte:

1. Descrição da solução.
1. (opcional) Um diagrama para fazer referência rápida à solução.
1. Variações possíveis - coisas que são pequenas variações na solução principal podem ser agrupadas.
1. Avaliação da ideia com base nos critérios de avaliação acima.

A profundidade, detalhes e conteúdo dessas seções variarão com base na complexidade da funcionalidade a ser desenvolvida.

#### Experimento(s)

Descreva como a solução será avaliada para comprovar ou refutar que ela produzirá os resultados desejados. Isso pode assumir muitas formas, como a construção de um protótipo e a pesquisa de documentação e soluções de amostra existentes.

Além disso, **documente quaisquer suposições** feitas como parte do experimento.

> NOTA: Cronometrar esses experimentos pode ser benéfico para garantir que a equipe esteja aproveitando ao máximo o tempo, concentrando-se na coleta de evidências-chave da maneira mais simples e rápida possível.

#### Evidências

Apresente as evidências coletadas durante a experimentação que apoiam a hipótese de que esta solução atenderá aos resultados desejados. Exemplos podem incluir:

- Demonstrações gravadas ou ao vivo de um protótipo fornecendo as capacidades desejadas.
- Métricas coletadas durante o teste do protótipo.
- Documentação que indica que a solução pode fornecer as capacidades desejadas.

> NOTA: **Não é necessário ter evidências para cada capacidade, métrica ou restrição para que o design seja considerado pronto.** Em vez disso, concentre-se em apresentar evidências que sejam mais relevantes e impactantes para apoiar ou eliminar a hipótese.

### {Solução 2}

...

### {Solução N}

...

## Resultados

Esta seção deve conter uma tabela que classifica cada solução em relação a cada um dos critérios de avaliação:

| Solução   | Critério de Avaliação 1 | Critério de Avaliação 2 | ... | Critério de Avaliação N |
|------------|-----------------------|-----------------------|-----|-----------------------|
| Solução 1 |                       |                       |     |                       |
| Solução 2 |                       |                       |     |                       |
| ...        |                       |                       |     |                       |
| Solução M |                       |                       |     |                       |

Observação: A formatação da tabela pode mudar. No passado, tivemos sucesso com descrições qualitativas nas entradas da tabela e codificação de cores nas células para representar bom, justo, ruim.

## Decisão

A solução escolhida ou uma lista de perguntas que precisam ser respondidas antes que a decisão possa ser tomada.

No segundo caso, cada pergunta precisa ter uma tarefa de ação e uma pessoa atribuída para responder à pergunta. Assim que essas perguntas forem respondidas, o documento deve ser atualizado para refletir as respostas e a decisão final.

No primeiro caso, descreva qual solução foi escolhida e por quê. Resuma quais evidências informaram a decisão e como essas evidências se relacionaram com os resultados desejados.

> **IMPORTANTE**: As decisões devem ser tomadas com a compreensão de que podem mud

ar à medida que a equipe aprende mais. Elas são um ponto de partida, não um contrato.

## Próximos Passos

Quais trabalhos são esperados após a tomada de uma decisão? Exemplos incluem, mas não se limitam a:

1. Criar novos PBI's ou modificar os existentes.
1. Estudos de investigação de acompanhamento.
1. Criar especificações para interfaces públicas e integrações entre outras correntes de trabalho.
1. Entrada no Log de Decisões.
