# Considerações sobre Desenvolvimento Ágil para Projetos de Aprendizado de Máquina

## Visão Geral

Ao conduzir projetos de Aprendizado de Máquina (ML), seguimos a metodologia Ágil para o desenvolvimento de software, com algumas adaptações, pois reconhecemos que a pesquisa e experimentação são, por vezes, difíceis de planejar e estimar.

## Objetivos

1. Executar e gerenciar projetos de ML de forma eficaz.
2. Criar uma colaboração eficaz entre a equipe de ML e as outras equipes que trabalham no projeto.

Para saber mais sobre como a ISE conduz o processo Ágil para equipes de desenvolvimento de software, consulte este [documento](../agile-development).

Dentro deste framework, a equipe segue essas cerimônias Ágeis:

- [Gerenciamento do Backlog](../agile-development/advanced-topics/backlog-management/README.md)
- [Retrospectivas](../agile-development/core-expectations/README.md)
- [Scrum of Scrums](../agile-development/advanced-topics/effective-organization/scrum-of-scrums.md) (quando aplicável)
- [Planejamento de Sprint](../agile-development/core-expectations/README.md)
- [Reuniões Diárias (Stand-ups)](../agile-development/core-expectations/README.md)
- [Acordo de Trabalho (Working agreement)](../agile-development/advanced-topics/team-agreements/working-agreements.md)

### Observações sobre o processo Ágil durante a exploração e experimentação

1. Reconhecendo que histórias de usuário de ML e análises exploratórias são menos previsíveis do que as de desenvolvimento de software, nos esforçamos para ter um entregável para cada história de usuário em cada sprint.

2. Histórias de usuário e análises exploratórias são geralmente estimadas usando tamanhos de camisetas (T-shirt sizes) ou similares, e não em dias/horas reais. Veja mais [aqui](../agile-development/core-expectations/README.md) sobre estimativa de histórias.

3. As sessões de design de ML devem ser incluídas em cada sprint.

#### Exemplos de entregáveis de ML para cada sprint

- Código funcional (por exemplo, modelos, pipelines, código exploratório)
- Documentação de novas hipóteses e a aceitação ou rejeição de hipóteses anteriores como parte de uma Análise Orientada por Hipóteses (HDA). Para mais informações, consulte [Desenvolvimento Orientado por Hipóteses no site de Barry Oreilly](https://barryoreilly.com/explore/blog/how-to-implement-hypothesis-driven-development/)
- Resultados e aprendizados da Análise Exploratória de Dados (EDA) documentados

## Notas sobre a colaboração entre a equipe de ML e a equipe de desenvolvimento de software

- As equipes de ML e Desenvolvimento de Software trabalham juntas no projeto. A equipe usa um único backlog e participa das mesmas cerimônias Ágeis. Em casos em que o projeto envolve muitos participantes, dividimos em grupos de trabalho, mas ainda mantemos toda a equipe participando das cerimônias Ágeis.

- Se possível, o estudo de viabilidade e a experimentação inicial do modelo ocorrem antes do início do trabalho de operacionalização.
- A equipe de ML e a equipe de desenvolvimento compartilham a responsabilidade pela solução de MLOps.
- A interface do modelo de ML (API) é determinada o mais cedo possível, para permitir que os desenvolvedores considerem sua integração à linha de produção.
- Os artefatos de MLOps são desenvolvidos com uma colaboração contínua e revisão da equipe de ML, para garantir que abordagens apropriadas para experimentação e
produtização sejam utilizadas.
- As retrospectivas e o planejamento de sprints são realizados no nível de toda a equipe e não no nível de grupos de trabalho específicos.
