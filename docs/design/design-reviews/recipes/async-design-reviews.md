# Revisões de Design Assíncronas

## Objetivos

Permitir que os membros da equipe revisem os designs conforme a disponibilidade em suas agendas de trabalho.

## Impacto

Isso resulta nos seguintes benefícios:

- **Maior Participação e Acessibilidade**. Eles não precisam estar online e disponíveis ao mesmo tempo que outros para revisar.
- **Menos Restrição de Tempo**. Os revisores podem gastar mais tempo do que a duração de uma única reunião para pensar na abordagem e fornecer feedback.

## Medidas

As métricas e/ou KPIs usados para revisões de design em geral ainda se aplicariam. Consulte [revisões de design](../README.md#Measures) para orientações sobre medidas.

## Participação

A participação deve ser a mesma de qualquer revisão de design. Consulte [revisões de design](../README.md#Participation) para orientações sobre a participação.

## Orientações para Facilitação

O conceito é fazer com que o design siga o mesmo fluxo de trabalho das mudanças de código para implementar uma história ou tarefa. Em vez de código, no entanto, os artefatos adicionados ou alterados são documentos Markdown, bem como quaisquer outros artefatos de suporte (protótipos, exemplos de código, diagramas, etc).

### Pré-requisitos

#### Documentos de Design Controlados por Origem

A documentação de design deve estar em um repositório de controle de origem que suporta pull requests (ou seja, git). As diretrizes a seguir podem ser usadas para determinar em qual repositório os documentos devem estar.

1. Manter os documentos no mesmo repositório do código afetado permite que os documentos sejam atualizados atomicamente junto com o código no mesmo pull request.
2. Se a documentação representar código que está em muitos repositórios diferentes, pode ser mais sensato manter os documentos em seu próprio repositório.
3. Coloque os documentos de forma que eles não acionem compilações de CI para o código afetado (supondo que a documentação tenha sido a única alteração). Isso pode ser feito colocando-os em um diretório isolado, caso eles estejam junto com o código que representam. Veja o exemplo de estrutura de diretórios abaixo.

```text
-root
  --src
  --docs <-- excluído do gatilho de compilação CI
    --design
```

### Fluxo de Trabalho

1. O designer cria um branch no repositório com a documentação.
2. O designer trabalha na adição ou atualização da documentação relevante para o design.
3. O designer envia uma solicitação de pull e solicita a revisão de membros específicos da equipe.
4. Os revisores fornecem feedback ao designer, que incorpora o feedback.
5. (OPCIONAL) Pode ser realizada uma reunião de revisão de design para fornecer uma explicação mais detalhada do design aos revisores.
6. O design é aprovado/aceito e mesclado no branch principal.

![Fluxo de Trabalho de Revisão de Design Assíncrona](images/async-design-reviews-sequence.png)

### Dicas para Ciclos de Revisão Mais Rápidos

Para garantir que um design seja revisado em tempo hábil, é importante solicitar diretamente as revisões dos membros da equipe. Se os membros da equipe forem designados sem perguntar, ou se ninguém for designado, é provável que o design fique mais tempo sem revisão. Tente as seguintes ações:

1. Faça com que seja responsabilidade do designer encontrar revisores para seu design.
2. O designer deve perguntar diretamente a um membro da equipe (conversa face a face, mensagens assíncronas, etc.) se ele está disponível para revisar. Somente se concordar, então atribua-o como revisor.
3. Indique se o design está pronto para ser mesclado após a aprovação.

### Indicar Completude do Design

Isso ajuda o revisor a entender se o design está pronto para ser aceito ou se ainda está em andamento. O nível e o tipo de feedback que o revisor fornece provavelmente serão diferentes, dependendo do estado do design. Tente as seguintes ações para indicar o estado do design:

1. Marque a PR como um Rascunho (Draft). Algumas ferramentas de ALM suportam a abertura de uma solicitação de pull como um Rascunho, como o Azure DevOps.
2. Coloque um prefixo no título como "RASCUNHO", "EM ANDAMENTO" ou "trabalho em andamento".
3. Configure a solicitação de pull para mesclar automaticamente após aprovações e verificações bem-sucedidas. Isso pode indicar ao revisor que o design está completo do ponto de vista do designer.

### Praticar Comportamentos Inclusivos

Os revisores designados não são os únicos membros da equipe que podem fornecer feedback sobre o design. Se outros membros da equipe voluntariamente dedicaram tempo para fornecer feedback ou fazer perguntas, certifique-se de responder. Utilize a conversa face a face (presencial ou virtual) para resolver feedback ou perguntas de outros conforme necessário. Isso ajuda a construir a coesão da equipe para garantir que todos entendam e estejam dispostos a se comprometer com um determinado design. **Essa prática demonstra um comportamento inclusivo**, o que promoverá confiança e respeito dentro da equipe.

1. Responda a todos os comentários da PR de forma **objetiva e respeitosa**, independentemente do nível, posição ou título do autor.
2. Após duas rodadas de perguntas/respostas, recorra à comunicação síncrona para a resolução (ou seja, conversa face a face virtual ou presencial).
