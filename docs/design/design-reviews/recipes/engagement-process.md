# Incorporando Revisões de Design em um Engajamento

## Introdução

As revisões de design não devem parecer um fardo. As revisões de design podem ser facilmente incorporadas ao processo de desenvolvimento da equipe com um mínimo de sobrecarga.

- Crie revisões de design apenas quando necessário. Nem toda história ou tarefa requer uma revisão de design completa.
- Utilize este guia para fazer alterações que se adaptem melhor à equipe. Cada equipe trabalha de maneira diferente.
- Recorra aos especialistas em determinados assuntos da Microsoft (SME) conforme necessário durante as revisões de design. Nem toda história precisa de um SME ou da aprovação da liderança. A maioria das revisões de design pode ser totalmente executada dentro de uma equipe de desenvolvimento.
- [Use diagramas](./preferred-diagram-tooling.md) para visualizar conceitos e arquitetura.

As seguintes diretrizes descrevem como a Microsoft e o cliente podem incorporar revisões de design em seu processo ágil do dia a dia.

## Visão Geral / Sessão de Design de Arquitetura (ADS)

No início de um engajamento, a Microsoft trabalha com os clientes para entender seus objetivos e metas exclusivos e estabelecer uma definição de feito. A Microsoft mergulha profundamente na infraestrutura e arquitetura existentes do cliente para entender as possíveis restrições. Além disso, buscamos compreender e descobrir [requisitos não funcionais](../../design-patterns/non-functional-requirements-capture-guide.md) específicos que influenciam a solução.

Durante esse período, a equipe descobre muitos desconhecidos, aproveitando todas as informações recém-descobertas, a fim de ajudar a gerar um design impactante que atenda aos objetivos do cliente. Após a ADS, pode ser útil realizar [Engenharia de Viabilidade (Engineering Feasibility Spikes)](../recipes/engineering-feasibility-spikes.md) para reduzir ainda mais os riscos das tecnologias consideradas para o engajamento.

> **Dica**: Todos os desconhecidos ainda não foram abordados neste ponto.

## Planejamento do Sprint

Em muitos engajamentos, a Microsoft trabalha com os clientes usando um processo de desenvolvimento ágil SCRUM que começa com o planejamento do sprint. O [planejamento do sprint](../../../agile-development/core-expectations/README.md) é uma ótima oportunidade para mergulhar profundamente no próximo conjunto de trabalho de alta prioridade. Alguns pontos-chave a serem abordados são os seguintes:

1. Identificar histórias que exigem revisões de design.
1. Separar o design da implementação para histórias complexas.
1. Atribuir um responsável para cada história de design.

Histórias que se beneficiarão com revisões de design têm uma ou mais das seguintes características em comum:

1. Existem muitos requisitos desconhecidos ou pouco claros.
1. Há uma ampla distribuição da carga de trabalho prevista, ou pontos de história, em toda a equipe de desenvolvimento.
1. O desenvolvedor não consegue ilustrar claramente todas as tarefas necessárias para a história.

> **Dica**: Após o planejamento do sprint estar concluído, a equipe deve considerar a realização de uma discussão inicial de revisão de design para aprofundar os requisitos de design das histórias que foram identificadas. Isso fornecerá mais clareza para que a equipe possa avançar com uma revisão de design, de forma síncrona ou assíncrona, e concluir as tarefas.

## Refinamento do Backlog do Sprint

Se sua equipe ainda não realiza uma sessão de [Refinamento do Backlog do Sprint](../../../agile-development/advanced-topics/backlog-management/README.md) pelo menos uma vez por semana, você deve considerar fazê-lo. É uma ótima oportunidade para:

1. Manter o backlog limpo.
1. Repriorizar o trabalho com base nas mudanças nas prioridades do negócio.
1. Preencher descrições e critérios de aceitação ausentes.
1. Identificar histórias que exigem revisões de design.

A equipe pode seguir as mesmas etapas do [planejamento do sprint](#planejamento-do-sprint) para ajudar a identificar quais histórias exigem revisões de design. Isso muitas vezes pode economizar muito tempo durante as próprias reuniões de planejamento do sprint para se concentrar na tarefa em questão.

## Retrospectivas do Sprint

As [retrospectivas do sprint](../../../agile-development/core-expectations/README.md) são um ótimo momento para fazer check-in com a equipe de desenvolvimento, identificar o que está funcionando ou não, e propor mudanças para continuar melhorando.

Também é um ótimo momento para verificar as revisões de design:

- Os designs mudaram desde o último sprint?
- Como as mudanças de design impactaram o engajamento?
- Os artefatos de design anteriores foram atualizados para refletir novas mudanças?

Todos os artefatos de design devem ser tratados como documentos vivos. À medida que os requisitos mudam ou mais desconhecidos são descobertos, a equipe de desenvolvimento deve atualizar retroativamente todos os artefatos de design. Não cumprir esta etapa crítica pode fazer com que o cliente incorra em dívida técnica futura. Artefatos que não estão atualizados são `bugs` no design.

> **Dica**: Mantenha seus artefatos atualizados adicionando-os à [Definição de Feito (Definition of Done)](../../../agile-development/advanced-topics/team-agreements/definition-of-done.md) de sua

 equipe para todas as histórias de usuário.

## Revisões de Design Síncronas

É frequentemente útil agendar 1-2 sessões de design por sprint como parte da cadência de reuniões mencionada anteriormente.
Ao longo do sprint, as pessoas podem adicionar tópicos de design à pauta da reunião e, se não houver nada a discutir para uma determinada reunião, ela pode ser simplesmente cancelada.
Embora essas sessões nem sempre sejam usadas, elas ajudam os membros do projeto a se alinharem quanto ao tempo e ao propósito desde cedo, estabelecendo um precedente e incentivando a participação para que os tópicos de design não sejam negligenciados.
Muitas vezes, é útil que os membros do projeto que pretendem apresentar seu design ao grupo mais amplo distribuam a documentação sobre seu design antes da sessão, para que os outros participantes possam se preparar com contexto antes da reunião.

Deve-se observar que a necessidade dessas sessões certamente evolui ao longo do engajamento.
No início, ou em outros momentos de maior ambiguidade, essas reuniões são geralmente usadas com mais frequência e de maneira mais completa.

Por fim, embora seja sugerido que as revisões de design síncronas sejam agendadas durante a cadência normal do sprint, não deve haver desencorajamento ao agendamento de sessões ad hoc - mesmo que essas revisões se limitem aos participantes de um fluxo de trabalho específico.

## Sprints de Encerramento

Os sprints de encerramento são um ótimo momento para amarrar pontas soltas com o cliente e entregar a solução. A entrega ao cliente fica muito mais fácil quando há artefatos de design para referenciar e entregar junto com a solução concluída.

Durante seus sprints de encerramento, a equipe de desenvolvimento deve considerar o seguinte:

1. Os artefatos de design estão atualizados?
1. Os artefatos de design estão armazenados em um local acessível?
