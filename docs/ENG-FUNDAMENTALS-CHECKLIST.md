# Lista de Verificação de Fundamentos de Engenharia

Esta lista de verificação ajuda a garantir que nossos projetos atendam aos nossos Fundamentos de Engenharia.

## Controle de Fonte

- [ ] A branch alvo padrão está bloqueada.
- [ ] As mesclagens são feitas através de PRs.
- [ ] PRs referenciam itens de trabalho relacionados.
- [ ] O histórico de commits é consistente e as mensagens de commit são informativas (o quê, por quê).
- [ ] Convenções de nomenclatura de branch consistentes.
- [ ] Documentação clara da estrutura do repositório.
- [ ] Segredos não fazem parte do histórico de commits ou são tornados públicos. (veja [Verificação de Credenciais](continuous-integration/dev-sec-ops/secret-management/credential_scanning.md))
- [ ] Repositórios públicos seguem as [diretrizes de OSS](source-control/README.md#creating-a-new-repository), veja `Arquivos obrigatórios na branch padrão para repositórios públicos`.

Mais detalhes sobre [controle de fonte](source-control/README.md)

## Rastreamento de Item de Trabalho

- [ ] Todos os itens são rastreados no AzDevOps (ou similar).
- [ ] O quadro está organizado (faixas de nado, tags de recursos, tags de tecnologia).

Mais detalhes sobre [gerenciamento de backlog](agile-development/advanced-topics/backlog-management/README.md)

## Testes

- [ ] Testes unitários cobrem a maioria de todos os componentes (>90% se possível).
- [ ] Testes de integração são executados para testar a solução de ponta a ponta.

Mais detalhes sobre [testes automatizados](automated-testing/README.md)

## CI/CD

- [ ] O projeto executa CI com build e teste automatizados em cada PR.
- [ ] O projeto usa CD para gerenciar implantações em um ambiente réplica antes que os PRs sejam mesclados.
- [ ] A branch principal está sempre pronta para ser enviada.

Mais detalhes sobre [integração contínua](continuous-integration/README.md) e [entrega contínua](continuous-delivery/README.md)

## Segurança

- [ ] O acesso é concedido apenas conforme a necessidade.
- [ ] Segredos são armazenados em locais seguros e não são incluídos no código.
- [ ] Os dados são criptografados em trânsito (e, se necessário, em repouso) e as senhas são hashadas.
- [ ] O sistema está dividido em segmentos lógicos com separação de preocupações? Isso ajuda a limitar vulnerabilidades de segurança.

Mais detalhes sobre [segurança](security/README.md)

## Observabilidade

- [ ] Eventos significativos de negócios e funcionais são rastreados e métricas relacionadas coletadas.
- [ ] Falhas e erros da aplicação são registrados.
- [ ] A saúde do sistema é monitorada.
- [ ] Os dados de observabilidade do lado do cliente e do servidor podem ser diferenciados.
- [ ] A configuração de registro pode ser modificada sem alterações de código (por exemplo: modo verboso).
- [ ] [Contexto de rastreamento de entrada](observability/correlation-id.md) é propagado para permitir fins de depuração de problemas de produção.
- [ ] A conformidade com o GDPR é assegurada em relação ao PII (Informações Pessoalmente Identificáveis).

Mais detalhes sobre [observabilidade](observability/README.md)

## Ágil/Scrum

- [ ] O Líder de Processo (fixo/rotativo) conduz o standup diário.
- [ ] O processo ágil está claramente definido dentro da equipe.
- [ ] O Líder de Desenvolvimento (+ PO/Outros) são responsáveis pelo gerenciamento e refinamento do backlog.
- [ ] Um acordo de trabalho é estabelecido entre os membros da equipe e o cliente.

Mais detalhes sobre [desenvolvimento ágil](agile-development/README.md)

## Revisões de Design

- [ ] O processo para realizar revisões de design está incluído no [Acordo de Trabalho](agile-development/advanced-topics/team-agreements/working-agreements.md).
- [ ] Revisões de design para cada componente principal da solução são realizadas e documentadas, incluindo alternativas.
- [ ] Histórias e/ou PRs estão vinculados ao documento de design.
- [ ] Cada história de usuário inclui uma tarefa para revisão de design por padrão, que é atribuída ou removida durante o planejamento do sprint.
- [ ] Consultores de projeto são convidados para revisões de design ou solicitados a dar feedback às decisões de design capturadas na documentação.
- [ ] Descubra todas as revisões que os processos do cliente exigem e planeje para elas.
- [ ] Requisitos não funcionais claros capturados (veja [Orientação para Requisitos Não Funcionais](design/design-patterns/non-functional-requirements-capture-guide.md))

Mais detalhes sobre [revisões de design](design/design-reviews/README.md)

## Revisões de Código

- [ ] Há um acordo claro na equipe quanto à função das revisões de código.
- [ ] A equipe possui uma lista de verificação de revisão de código ou processo estabelecido.
- [ ] Um número mínimo de revisores (geralmente 2) para uma mesclagem de PR é imposto por política.
- [ ] Linters/Analisadores de Código, testes unitários e builds bem-sucedidos para mesclagens de PR estão configurados.
- [ ] Há um processo para impor uma rápida revisão.

Mais detalhes sobre [revisões de código](code-reviews/README.md)

## Retrospectivas

- [ ] Retrospectivas são realizadas a cada semana/ao final de cada sprint.
- [ ] A equipe identifica 1-3 experimentos propostos para tentar a cada semana/sprint para melhorar o processo.
- [ ] Experimentos têm proprietários e são adicionados ao backlog do projeto.
- [ ] A equipe realiza retrospectivas mais longas para Marcos e conclusão do projeto.

Mais detalhes sobre [retrospectivas](agile-development/core-expectations/README.md)

## Feedback de Engenharia

- [ ] A equipe envia feedback sobre bloqueadores de negócios e técnicos que impedem o sucesso do projeto.
- [ ] Sugestões para melhorias são incorporadas na solução.
- [ ] O feedback é detalhado e repetível.

Mais detalhes

 sobre [feedback de engenharia](engineering-feedback/README.md)

## Experiência do Desenvolvedor (DevEx)

Desenvolvedores na equipe podem:

- [ ] Construir/Compilar a fonte para verificar se está livre de erros de sintaxe e compila.
- [ ] Executar todos os testes automatizados (unitários, e2e, etc).
- [ ] Iniciar/Lançar de ponta a ponta para simular a execução em um ambiente implantado.
- [ ] Anexar um depurador à solução iniciada ou aos testes automatizados em execução, definir pontos de interrupção, percorrer o código e inspecionar variáveis.
- [ ] Instalar automaticamente dependências pressionando F5 (ou equivalente) em seu IDE.
- [ ] Usar valores de configuração de desenvolvimento local (ou seja, .env, appsettings.development.json).

Mais detalhes sobre [experiência do desenvolvedor](developer-experience/README.md)
