# Perguntas Frequentes (FAQ)

Esta é uma lista de perguntas / problemas frequentemente encontrados ao trabalhar com revisões de código e respostas sobre como você pode possivelmente abordá-los.

## O que torna uma revisão de código diferente de um PR?

Um pull request (PR) é uma forma de notificar que uma tarefa está concluída e pronta para ser mesclada no branch principal de trabalho (fonte da verdade). Uma revisão de código é ter alguém examinando o código em um PR e validando-o antes de ser mesclado, mas, em geral, revisões de código também podem ocorrer fora dos PRs.

| Revisão de Código | Pull Request |
--- | ---
| Focado no código-fonte | Destinado a aprimorar e habilitar revisões de código. Inclui tanto o código-fonte quanto pode ter um escopo mais amplo (por exemplo, documentação, testes de integração, compilações) |
| Destinado para **feedback antecipado** antes de enviar um PR | Não destinado para **feedback antecipado**. Criado quando o autor está pronto para mesclar |
| Geralmente uma revisão síncrona com ciclos de feedback mais rápidos (PRs em rascunho como exceção). Exemplos: reuniões agendadas, revisão presencial, programação em pares | Geralmente uma revisão assíncrona auxiliada por ferramentas, mas pode ser elevada para uma reunião síncrona quando necessário |

## Por que precisamos de revisões de código?

Nossas revisões de código entre pares são estruturadas em torno das melhores práticas, para encontrar tipos específicos de erros. Assim como você ainda executaria um linter em código mobado, você ainda pediria a alguém para fazer a última verificação para garantir que o código esteja em conformidade com os padrões esperados e evite armadilhas comuns.

## Os PRs são muito grandes, como podemos corrigir isso?

Certifique-se de dimensionar os itens de trabalho em pedaços pequenos e claros, para que a pessoa revisora possa entender o código por conta própria. A equipe é instruída a fazer commits antecipadamente, antes que o item completo do backlog do produto / história do usuário esteja completo, mas sim quando um item individual está concluído. Se o trabalho resultaria em um recurso incompleto, certifique-se de que ele possa ser desativado até que o recurso completo seja entregue.
Mais informações podem ser encontradas em [Pull Requests - Orientações de Tamanho](./pull-requests.md#size-guidance).

## Como podemos acelerar as revisões de código?

Revisões de código lentas podem causar atrasos na entrega de recursos e causar frustração entre os membros da equipe.

### Ações possíveis que você pode tomar

- Adicione uma regra para o tempo de resposta do PR ao seu acordo de trabalho.
- Reserve um espaço após o standup para analisar os PRs pendentes e atribuir aqueles que estão inativos.
- Dedique um gerente de revisão de PR que será responsável por manter as coisas fluindo, atribuindo ou notificando pessoas quando o PR ficou obsoleto.
- Use ferramentas para indicar melhor revisões obsoletas - [Personalizar ADO - Quadros de Tarefas](tools.md#task-boards).

## Quais ferramentas posso usar para revisar um PR complexo?

Consulte a seção [Ferramentas](./tools.md) para obter ajuda sobre como realizar revisões fora do Visual Studio ou Visual Studio Code.

## Como podemos impor políticas de revisão de código?

[Ao configurar Políticas de Branch](tools.md#Configuring Branch Policies), você pode facilmente impor regras de revisões de código.

## Nós fazemos programação em pares ou em grupo. Como isso deve se refletir em nossas revisões de código?

Existem duas formas de realizar uma revisão de código:

1. Par - Alguém de fora do par deve realizar a revisão de código. Um dos outros grandes benefícios das revisões de código é disseminar o conhecimento sobre a base de código para outros membros da equipe que normalmente não trabalham na parte da base de código em revisão.
2. Grupo - Um membro do grupo que passou menos (ou nenhum) tempo no teclado deve realizar a revisão de código.
