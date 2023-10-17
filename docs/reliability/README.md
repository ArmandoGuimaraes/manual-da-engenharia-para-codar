# Confiabilidade

Todos os outros fundamentos de engenharia da ISE trabalham para uma infraestrutura mais confiável. A integração e implantação automatizadas garantem que o código seja adequadamente testado e ajuda a eliminar erros humanos, enquanto as implantações lentas constroem confiança no código. A observabilidade ajuda a identificar erros mais rapidamente quando surgem para retornar a um estado estável, e assim por diante.

No entanto, existem algumas etapas adicionais que podemos tomar, que não se encaixam perfeitamente nas categorias anteriores, para garantir uma solução mais confiável. Vamos explorar essas etapas abaixo.

## Remova os "Foot-Guns"

Evite que sua equipe de desenvolvimento se prejudique. As pessoas cometem erros; qualquer erro cometido em produção não é culpa da pessoa, é culpa coletiva do sistema por não prevenir que o erro aconteça.

Confira a lista abaixo de algumas ferramentas comuns para remover esses "foot guns":

* No Kubernetes, aproveite os [Controladores de Admissão](https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/) para evitar que "coisas ruins" aconteçam.
  * Você pode criar controladores personalizados usando o controlador de admissão Webhook.
* [Gatekeeper](https://github.com/open-policy-agent/gatekeeper) é um controlador de admissão Webhook pré-construído, usando [OPA](https://github.com/open-policy-agent/opa) por baixo, com suporte para algumas [proteções prontas para uso](https://github.com/open-policy-agent/gatekeeper-library/tree/master/library).

Se um usuário cometer um erro, não pergunte: "como alguém poderia fazer isso?", pergunte: "como podemos evitar que isso aconteça no futuro?"

## Dimensionamento Automático

Sempre que possível, aproveite o dimensionamento automático para suas implantações. O dimensionamento vertical pode dimensionar suas VMs ajustando parâmetros como CPU, disco e RAM, enquanto o dimensionamento horizontal pode ajustar o número de imagens em execução que sustentam suas implantações. O dimensionamento automático pode ajudar seu sistema a responder ao crescimento não orgânico no tráfego e evitar solicitações falhadas devido à escassez de recursos.

> Observação: Em ambientes como o K8s, tanto o dimensionamento horizontal quanto o vertical são oferecidos como solução nativa. No entanto, as VMs que sustentam cada Pod também podem precisar de dimensionamento automático para lidar com um aumento no número de Pods.

Também deve ser observado que os parâmetros que afetam o dimensionamento automático podem ser difíceis de ajustar. Métricas típicas como utilização de CPU ou RAM, ou taxa de solicitações, podem não ser suficientes. Às vezes, pode ser necessário considerar métricas personalizadas, como a taxa de evicção de cache.

## Descarte de Carga e Proteção contra Ataques de Negação de Serviço (DOS)

Muitas vezes, pensamos em ataques de Negação de Serviço [DOS] como um ato de um ator malicioso, então colocamos algum descarte de carga nos portões do nosso sistema e chamamos isso de dia. Na realidade, muitos ataques DOS são acidentais e autoinfligidos. Uma implantação ruim que derruba um cache resulta em sobrecarregar serviços downstream. A coleta de dados de um sistema distribuído se sincroniza e resulta em um [problema de manada trovejante](https://en.wikipedia.org/wiki/Thundering_herd_problem). Uma má configuração resulta em um erro que desencadeia tentativas de recuperação incontroláveis por parte dos clientes. As solicitações se acumulam em um objeto armazenado até que ele fique tão grande que leituras futuras travem o servidor. A lista continua.

Siga estas etapas para se proteger:

* Adicione uma oscilação (aleatória) a qualquer ação que ocorra a partir de um fluxo não acionado pelo usuário (ou seja, adicione uma duração aleatória ao tempo de espera em um cron ou trabalho que consulta continuamente um serviço downstream).
* Implemente políticas de tentativas de recuperação com [backoff exponencial](https://en.wikipedia.org/wiki/Exponential_backoff) em seu código do cliente.
* Adicione descarte de carga aos seus servidores (sim, também aos seus microsserviços internos).
  * Isso pode ser configurado facilmente ao usar um sidecar como o Envoy.
* Tenha cuidado ao desserializar solicitações de usuário e use limites de buffer.
  * Por exemplo, servidores HTTP/gRPC podem definir limites sobre quanto dados serão lidos do soquete.
* Defina alertas para utilização, reinicialização de servidores ou desconexão para detectar quando seu sistema pode estar falhando.

Esses tipos de erros podem resultar em falhas em cascata, onde uma parte não crítica do seu sistema derruba todo o serviço. Planeje adequadamente e certifique-se de pensar em como seu sistema pode degradar durante falhas.

## Backup de Dados

Dados são perdidos, corrompidos ou excluídos acidentalmente. Isso acontece. Faça backups dos dados para ajudar a colocar seu sistema online o mais rápido possível. Isso pode ocorrer na pilha de aplicativos, com código excluindo ou corrompendo dados, ou na camada de armazenamento, com perda de volumes ou chaves de criptografia.

Considere coisas como:

* Quanto tempo levará para restaurar os dados.
* Quanta perda de dados você pode tolerar.
* Quanto tempo levará para perceber que ocorreu perda de dados.

Pesquise a diferença entre **snapshot** e **incremental** backups. Uma boa política pode ser fazer backups incrementais em um período de N e backups de snapshot em um período de M (onde N < M).

## Meta de Tempo de Atividade e Falha Graciosa

É um fato conhecido que os sistemas não podem ter como meta 100% de tempo de atividade. Existem muitos fatores nos sistemas de software de hoje que impedem isso, muitos deles fora de nosso controle. Mesmo um serviço que nunca é atualizado e é 100% livre de bugs falhará. Servidores DNS upstream têm problemas o tempo todo. Hardware quebra. Quedas de energia, geradores de backup falham. O mundo é caótico. Bons serviços têm como meta algum número de "9s" de tempo de atividade. Por exemplo, 99,

99% de tempo de atividade significa que o sistema tem um "orçamento" de 4 minutos e 22 segundos de tempo de atividade a cada mês. Alguns meses podem atingir 100% de tempo de atividade, o que significa que esse orçamento é rolado para o próximo mês. O que o tempo de atividade significa é diferente para todos e depende do serviço definir.

Uma boa prática é usar qualquer tempo de atividade restante no final do período (por exemplo, ano, trimestre) para derrubar intencionalmente esse serviço e garantir que o resto de seus sistemas falhe conforme o esperado. Muitas vezes, outros engenheiros e serviços começam a depender dessa disponibilidade adicional alcançada, e pode ser saudável garantir que os sistemas falhem graciosamente.

Podemos incorporar a falha graciosa (ou degradação graciosa) em nossa pilha de software antecipando falhas. Algumas táticas incluem:

* Failover para serviços saudáveis.
  * A [Eleição de Líder](https://en.wikipedia.org/wiki/Leader_election) pode ser usada para manter serviços saudáveis em espera caso o líder tenha problemas.
  * A falha do cluster inteiro pode redirecionar o tráfego para outra região ou zona de disponibilidade.
  * Propague as falhas a jusante de serviços dependentes pela pilha por meio de verificações de integridade, para que seus pontos de entrada possam redirecionar o tráfego para serviços saudáveis.
* Os [disjuntores](https://techblog.constantcontact.com/software-development/circuit-breakers-and-microservices/#:~:text=The%20Circuit%20breaker%20pattern%20helps,unavailable%20or%20have%20high%20latency.) podem interromper precocemente as solicitações em vez de propagar erros por todo o sistema.
  Considere usar uma biblioteca bem conhecida e testada, como o [Polly](https://github.com/App-vNext/Polly) (.NET), que permite implementações configuráveis desse e de outros padrões comuns de tratamento de falhas de resiliência e transitórias.

## Prática

[Nenhuma das recomendações acima funcionará se não forem testadas](https://thinkmeta.net/2010/11/06/what-is-an-untested-dr-plan-worth/). Seus backups são inúteis se você não souber como montá-los. Suas estratégias de failover e outras mitigações regredirão com o tempo se não forem testadas. Aqui estão algumas dicas para testar o que foi mencionado acima:

### Mantenha Playbooks

Nenhum serviço de software está completo sem playbooks para orientar os desenvolvedores por território desconhecido. Os playbooks devem ser abrangentes e cobrir todos os cenários de falha conhecidos e suas mitigações.

### Realize exercícios de manutenção

Dedique tempo para criar cenários fictícios e conduza uma campanha no estilo D&D para resolver seus problemas. Isso pode ser tão elaborado quanto criar um novo ambiente e injetar erros ou tão simples quanto pedir aos "jogadores" que acessem um painel e descrevam o que veriam no cenário fictício (pequenas doses de imaginação são necessárias). Os playbooks devem guiar facilmente o usuário para a solução/mitigação correta. Se não o fizerem, atualize seus playbooks.

### Teste de Caos

Aproveite os testes de caos automatizados para ver como as coisas quebram. Você pode ler o [artigo deste playbook sobre testes de injeção de falhas](../automated-testing/fault-injection-testing/README.md) para obter mais informações sobre o desenvolvimento de uma suíte de testes de caos orientados por hipóteses. A seguinte lista de ferramentas de teste de caos, bem como [esta seção no artigo linkado acima](../automated-testing/fault-injection-testing/README.md#chaos), têm mais detalhes sobre plataformas e ferramentas disponíveis para esse fim:

* [Azure Chaos Studio](https://learn.microsoft.com/en-US/azure/chaos-studio/chaos-studio-overview) - Uma ferramenta em prévia para orquestrar experimentos controlados de injeção de falhas em recursos do Azure.
* [Chaos toolkit](https://chaostoolkit.org/) - Uma plataforma de caos declarativa e modular com muitas extensões, incluindo o [kit de ações e sondas do Azure](https://github.com/chaostoolkit-incubator/chaostoolkit-azure).
* [Kraken](https://github.com/openshift-scale/kraken) - Uma ferramenta específica para o Openshift, mantida pela Red Hat.
* [Chaos Monkey](https://github.com/netflix/chaosmonkey) - A plataforma Netflix que popularizou a engenharia do caos (não oferece suporte ao Azure out-of-the-box).
* Muitas malhas de serviços, como o [Linkerd](https://linkerd.io/2/features/fault-injection/), oferecem ferramentas de injeção de falhas por meio do uso de seus sidecars.
* [Chaos Mesh](https://github.com/chaos-mesh/chaos-mesh)
* [Simmy](https://github.com/Polly-Contrib/Simmy) - Uma biblioteca .NET para teste de caos e injeção de falhas integrada com a biblioteca [Polly](https://github.com/App-vNext/Polly) para engenharia de resiliência.
[Este post no blog de desenvolvimento da ISE](https://devblogs.microsoft.com/cse/2023/03/07/build-test-resilience-dotnet-functions/) fornece trechos de código como exemplo de como usar o Polly e o Simmy para implementar uma abordagem orientada por hipóteses para testes de resiliência e caos.

## Analise Todas as Falhas

Escrever um [pós-mortem](https://en.wikipedia.org/wiki/Postmortem_documentation) é uma ótima maneira de documentar as causas raiz e as ações necessárias para suas falhas. Também é uma ótima maneira de rastrear problemas recorrentes e criar um forte argumento para priorizar correções.

Isso pode até ser vinculado às suas [retrospectivas](../agile-development/core-expectations/README.md) regulares do Agile.
