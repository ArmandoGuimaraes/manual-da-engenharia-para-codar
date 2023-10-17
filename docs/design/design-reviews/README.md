# Revisões de Design

## Sumário

- [Objetivos](#objetivos)
- [Medidas](#medidas)
- [Impacto](#impacto)
- [Participação](#participação)
- [Orientação para Facilitação](#orientação-para-facilitação)
- [Avaliação Técnica](#avaliação-técnica)

## Objetivos

- Reduzir a dívida técnica para nossos clientes.
- Continuar a iterar no design após a revisão do Plano de Jogo.
- Gerar artefatos técnicos úteis que possam ser referenciados pela Microsoft e pelos clientes.

## Medidas

### Custo da Mudança

Ao incorporar revisões de design como parte do processo de engenharia, as decisões são tomadas antecipadamente antes do início da implementação. Tomar a decisão de usar o Azure Kubernetes Service em vez dos Serviços de Aplicativos na fase de design provavelmente requer apenas a atualização da documentação. No entanto, fazer essa mudança após o início da implementação ou após a solução estar em uso é muito mais custoso.

Essas mudanças estão ocorrendo antes ou depois da implementação? Qual é o esforço normalmente envolvido?

### Participação dos Revisores

Quantas pessoas participam das revisões dos designs criados? Cumulativamente, se esse número for maior, isso indicaria uma maior contribuição de ideias e perspectivas. Um número menor (ou seja, as mesmas 2 pessoas apenas em cada revisão) pode indicar um conjunto limitado de perspectivas. Alguém está participando de fora da equipe central de desenvolvimento?

### Tempo para Soluções Potenciais

Quanto tempo normalmente leva para ir dos requisitos às opções de solução (múltiplas)?

Há um equilíbrio saudável entre gastar muito ou muito pouco tempo avaliando diferentes soluções potenciais. Muito pouco tempo aumenta o risco de mudanças custosas necessárias após a implementação. Muito tempo atrasa a entrega do valor-alvo e das funcionalidades subsequentes na fila. No entanto, quanto mais rápido a equipe puder *identificar as informações mais críticas necessárias para tomar uma decisão informada*, mais rápido o valor pode ser entregue com menor risco de mudanças custosas no futuro.

### Tempo para Decisões

Quanto tempo leva para tomar uma decisão sobre qual solução implementar?

Também há um equilíbrio saudável em apoiar um debate saudável sem prejudicar a entrega da equipe. O caso ideal é que a equipe assimile rapidamente as opções de solução apresentadas, faça perguntas e debata antes de finalmente alcançar um consenso sobre uma abordagem específica. Nos casos em que não houver consenso, a pessoa com mais contexto sobre o problema (geralmente o proprietário da história) deve tomar a decisão final. Priorize a entrega de valor e aprendizado. Discordem e comprometam-se!

## Impacto

- As soluções podem ser rapidamente implementadas no ambiente de produção do cliente.
- É mais fácil para outras equipes de desenvolvimento aproveitar o trabalho de sua equipe.
- É mais fácil para os engenheiros se envolverem em projetos.
- Aumento da velocidade da equipe ao antecipar mudanças e decisões quando elas custam menos.
- Maior engajamento e transparência da equipe ao solicitar ampla participação dos revisores.

## Participação

### Equipe de Desenvolvimento

A equipe de desenvolvimento deve sempre participar de todas as sessões de revisão de design.

- Engenharia do ISE (Intelligent Security Engineering)
- Engenharia de Clientes

### Especialistas em Domínio

Os especialistas em domínio devem participar das sessões de revisão de design conforme necessário.

- Domínios Técnicos do ISE
- Especialistas em assuntos do cliente (SME - Subject Matter Experts)
- Liderança Sênior

## Orientação para Facilitação

### Receitas

Consulte nossas [Receitas de Revisão de Design](./recipes/README.md) para orientações sobre o processo de design.

### Sincronização de Revisões de Design via Reuniões Presenciais/Virtuais

Reuniões conjuntas com a equipe de desenvolvimento, especialistas em domínio e engenheiros do cliente.

### Revisões de Design Assíncronas via Pull Requests

Consulte a [receita de revisão de design assíncrona](./recipes/async-design-reviews.md) para orientações sobre a facilitação de revisões de design assíncronas. Isso pode ser útil para equipes que estão geograficamente distribuídas em diferentes fusos horários.

## Avaliação Técnica

Um spike técnico é mais frequentemente usado para avaliar o impacto que uma nova tecnologia tem na implementação atual. Leia mais [aqui](./recipes/technical-spike.md).

## Documentação de Design

- Documentar e atualizar o design de arquitetura na documentação de design do projeto.
- Rastrear e documentar decisões de design em um [registro de decisões](decision-log/README.md).
- Documentar o processo de decisão em [estudos de troca](trade-studies/README.md) quando várias soluções existem para o problema dado.

No início das colaborações, a equipe deve decidir onde armazenar os artefatos gerados pelas revisões de design. Normalmente, nos encontramos com o cliente onde eles preferem (por exemplo, usando sua instância Confluence para armazenar a documentação, se essa for a preferência deles). No entanto, semelhante ao armazenamento de registros de decisões, estudos de troca, etc., no repositório de desenvolvimento, também existem grandes benefícios em manter os artefatos de revisão de design no próprio repositório. Normalmente, esses artefatos podem ser adicionados ao diretório de documentação de nível superior ou até mesmo à raiz do projeto correspondente, se o repositório for monolítico. Ao adicioná-los ao repositório do projeto, esses artefatos devem ser revisados em Pull Requests (normalmente precedendo, mas às vezes acompanhando a implementação), o que permite a revisão/discussão assíncrona. Além disso, os artefatos podem ser facilmente vinculados a outras seções do repositório e a arquivos de código-fonte (por meio de [links em Markdown](https://www.w3schools.io/file/markdown-links/)).
