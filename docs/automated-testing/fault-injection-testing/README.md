# Teste de Injeção de Falhas

O teste de injeção de falhas é a introdução deliberada de erros e falhas em um sistema para validar e fortalecer sua [estabilidade e confiabilidade](../../reliability/README.md). O objetivo é melhorar o design do sistema para resiliência e desempenho sob condições intermitentes de falha ao longo do tempo.

## Quando Usar

### Problema Abordado

Os sistemas precisam ser resilientes às condições que causam interrupções inevitáveis na produção. Aplicações modernas são construídas com um número crescente de dependências; em infraestrutura, plataforma, rede, software de terceiros ou APIs, etc. Tais sistemas aumentam o risco de impacto de interrupções de dependência. Cada componente dependente pode falhar. Além disso, suas interações com outros componentes podem propagar a falha.

Métodos de injeção de falhas são uma forma de aumentar a cobertura e validar a robustez do software e o tratamento de erros, seja no momento da construção ou em tempo de execução, com a intenção de "abraçar a falha" como parte do ciclo de vida do desenvolvimento. Esses métodos auxiliam as equipes de engenharia no projeto e na validação contínua para falhas, contabilizando condições de falha conhecidas e desconhecidas, arquitetura para redundância, emprego de mecanismos de repetição e retrocesso, etc.

### Aplicável a

* **Software** - Caminhos de código para tratamento de erros, gerenciamento de memória no processo.
  * *Exemplos de testes:* Testes de unidade/integração de casos extremos e/ou [testes de carga](../performance-testing/load-testing.md) (ou seja, stress e soak).
* **Protocolo** - Vulnerabilidades em interfaces de comunicação, como parâmetros de linha de comando ou APIs.
  * *Exemplos de testes:* [Fuzzing](https://owasp.org/www-community/Fuzzing) fornece dados inválidos, inesperados ou aleatórios como entrada, podemos avaliar o nível de estabilidade do protocolo de um componente.
* **Infraestrutura** - Interrupções, problemas de rede, falhas de hardware.
  * *Exemplos de testes:* Usar diferentes métodos para causar falha na infraestrutura subjacente, como desligar instâncias de máquinas virtuais (VM), travar processos, expirar certificados, introduzir latência de rede, etc. Esse nível de teste depende de observações de métricas estatísticas ao longo do tempo e da medição dos desvios de seu comportamento observado durante a falha, ou de seu tempo de recuperação.

## Como Usar

### Arquitetura

#### Terminologia

* **Falha** - A causa julgada ou hipotetizada de um erro.
* **Erro** - Aquela parte do estado do sistema que pode causar uma falha subsequente.
* **Falha** - Um evento que ocorre quando o serviço prestado se desvia do estado correto.
* **Ciclo Falha-Erro-Falha** - Um mecanismo chave em [confiabilidade](https://en.wikipedia.org/wiki/Dependability): Uma falha pode causar um erro. Um erro pode causar mais erros dentro do limite do sistema; portanto, cada novo erro atua como uma falha. Quando estados de erro são observados no limite do sistema, eles são denominados falhas.
(Modelado por [Laprie/Avizienis](https://www.nasa.gov/pdf/636745main_day_3-algirdas_avizienis.pdf))

#### Fundamentos do Teste de Injeção de Falhas

A injeção de falhas é uma forma avançada de teste em que o sistema é submetido a diferentes [modos de falha](https://en.wikipedia.org/wiki/Failure_mode_and_effects_analysis), e onde o engenheiro de teste pode saber antecipadamente qual é o resultado esperado, como no caso de testes de validação de lançamento, ou em uma exploração para encontrar problemas potenciais no produto, que devem ser mitigados.

#### Injeção de Falhas e Engenharia do Caos

O teste de injeção de falhas é uma abordagem específica para testar uma condição. Ele introduz uma falha em um sistema para validar sua robustez. A engenharia do caos, cunhada pela Netflix, é uma prática para gerar novas informações. Há uma sobreposição de preocupações e muitas vezes de ferramentas entre os termos, e muitas vezes a engenharia do caos usa injeção de falhas para introduzir os efeitos necessários no sistema.

### Passos de Alto Nível

#### Teste de injeção de falhas no ciclo de desenvolvimento

A injeção de falhas é uma forma eficaz de encontrar bugs de segurança no software, tanto que o [Ciclo de Desenvolvimento de Segurança da Microsoft](https://www.microsoft.com/en-us/securityengineering/sdl/practices) exige fuzzing em todas as interfaces não confiáveis de cada produto e teste de penetração, que inclui a introdução de falhas no sistema, para descobrir vulnerabilidades potenciais resultantes de erros de codificação, falhas de configuração do sistema ou outras fraquezas operacionais de implantação.

A cobertura automatizada de injeção de falhas em um pipeline de CI promove uma abordagem [Shift-Left](https://en.wikipedia.org/wiki/Shift-left_testing) de teste mais cedo no ciclo de vida para possíveis problemas.
Exemplos de realização de injeção de falhas durante o ciclo de vida do desenvolvimento:

* Usando ferramentas de fuzzing no CI.
* Executar testes de cenário de ponta a ponta existentes (como testes de integração ou de estresse), que são aumentados com injeção de falhas.
* Escrever testes de regressão e aceitação com base em problemas que foram encontrados e corrigidos ou com base em incidentes de serviço resolvidos.
* Validações ad-hoc (manuais) de falha no ambiente de desenvolvimento para novos recursos.

#### Teste de injeção de falhas no ciclo de lançamento

Muito parecido com [Testes de Monitoramento Sintético](../synthetic-monitoring-tests/README.md), o teste de injeção de falhas no ciclo de lançamento faz parte da abordagem de [teste Shift-Right](https://learn.microsoft.com/en-us/devops/deliver/shift-right-test-production), que usa métodos seguros para realizar testes em um ambiente de produção ou pré-produção. Dada a natureza das aplicações distribuídas baseadas em nuvem, é muito difícil simular o comportamento real dos serviços fora de seu ambiente de produção. Os testadores são incentivados a executar testes onde realmente importa, em um sistema ao vivo com tráfego de clientes.

Os testes de injeção de falhas dependem da observabilidade de métricas e geralmente são estatísticos; Os seguintes passos de alto nível fornecem uma amostra de como praticar injeção de falhas e engenharia do caos:

* Medir e definir um estado estável (saudável) para a interoperabilidade do sistema.
* Criar hipóteses com base no comportamento previsto quando uma falha é introduzida.
* Introduzir eventos de falha do mundo real no sistema.
* Medir o estado e compará-lo ao estado de referência.
* Documentar o processo e as observações.
* Identificar e agir com base no resultado.

#### Teste de injeção de falhas em Kubernetes

Com o avanço do Kubernetes (k8s) como plataforma de infraestrutura, o teste de injeção de falhas em Kubernetes tornou-se inevitável para garantir que o sistema se comporte de maneira confiável no caso de uma falha ou falha. Pode haver diferentes tipos de cargas de trabalho rodando dentro de um cluster k8s que são escritas em diferentes linguagens. Por exemplo, dentro de um cluster K8s, você pode executar um microserviço, um aplicativo web e/ou um trabalho agendado. Portanto, você precisa ter um mecanismo para injetar falhas em qualquer tipo de carga de trabalho rodando dentro do cluster. Além disso, os clusters Kubernetes são gerenciados de forma diferente da infraestrutura tradicional. As ferramentas usadas para teste de injeção de falhas dentro do Kubernetes devem ter compatibilidade com a infraestrutura k8s. Estas são as principais características que são necessárias:

* Facilidade de injetar falhas em pods do Kubernetes.
* Suporte para instalação rápida de ferramentas dentro do cluster.
* Suporte para configurações baseadas em YAML que funcionam bem com o Kubernetes.
* Facilidade de personalização para adicionar recursos personalizados.
* Suporte para fluxos de trabalho para implantar várias cargas de trabalho e falhas.
* Facilidade de manutenção da ferramenta.
* Facilidade de integração com telemetria.

## Melhores Práticas e Conselhos

Experimentar na produção tem o benefício de executar testes contra um sistema ao vivo com tráfego real de usuários, garantindo sua saúde ou construindo confiança em sua capacidade de lidar com erros de forma elegante. No entanto, tem o potencial de causar dor desnecessária ao cliente.
Um teste pode ter sucesso ou falhar. No caso de falha, é provável que haja algum impacto no ambiente de produção. Pensar sobre o **Raio de Explosão** do efeito, caso o teste falhe, é um passo crucial a ser realizado previamente. As seguintes práticas podem ajudar a minimizar esse risco:

* Execute testes em um ambiente não produtivo primeiro. Entenda como o sistema se comporta em um ambiente seguro, usando carga de trabalho sintética, antes de introduzir riscos potenciais ao tráfego do cliente.
* Use injeção de falhas como portões em diferentes estágios ao longo do pipeline de CD.
* Implantar e testar em implantações Blue/Green e Canary. Use métodos como sombreamento de tráfego (também conhecido como [Tráfego Escuro](https://cloud.google.com/blog/products/gcp/cre-life-lessons-what-is-a-dark-launch-and-what-does-it-do-for-me)) para levar o tráfego do cliente ao slot de staging.
* Esforce-se para alcançar um equilíbrio entre coletar dados de resultados reais, afetando o mínimo possível de usuários de produção.
* Use princípios de design defensivo, como circuit breaking e os padrões de anteparo.
* Acordar um orçamento (em termos de Objetivo de Nível de Serviço (SLO)) como um investimento em caos e injeção de falhas.
* Aumente o risco de forma incremental - Comece fortalecendo o núcleo e expanda em camadas. Em cada ponto, o progresso deve ser consolidado com testes de regressão automatizados.

## Frameworks e Ferramentas de Teste de Injeção de Falhas

### Fuzzing

* [OneFuzz](https://github.com/microsoft/onefuzz) - é uma plataforma de fuzzing como serviço de código aberto da Microsoft que é fácil de integrar em pipelines de CI.
* [AFL](https://lcamtuf.coredump.cx/afl/) e [WinAFL](https://github.com/googleprojectzero/winafl) - Ferramentas de fuzz populares da equipe do projeto zero do Google, que são usadas localmente para direcionar binários no Linux ou Windows.
* [WebScarab](https://github.com/OWASP/OWASP-WebScarab) - Um fuzzer focado na web de propriedade da OWASP, que pode ser encontrado nas distribuições [Kali Linux](https://tools.kali.org/web-applications/webscarab).

### Caos

* [Azure Chaos Studio](https://learn.microsoft.com/en-US/azure/chaos-studio/chaos-studio-overview) - Uma ferramenta em pré-visualização para orquestrar experimentos controlados de injeção de falhas em recursos do Azure.
* [Chaos toolkit](https://chaostoolkit.org/) - Uma plataforma de caos declarativa e modular com muitas extensões, incluindo o [kit de ações e sondas do Azure](https://github.com/chaostoolkit-incubator/chaostoolkit-azure).
* [Kraken](https://github.com/openshift-scale/kraken) - Uma ferramenta de caos específica do Openshift, mantida pela Redhat.
* [Chaos Monkey](https://github.com/netflix/chaosmonkey) - A plataforma da Netflix que popularizou a engenharia do caos (não suporta Azure OOTB).
* [Simmy](https://github.com/Polly-Contrib/Simmy) - Uma biblioteca .NET para testes de caos e injeção de falhas integrada à biblioteca [Polly](https://github.com/App-vNext/Polly) para engenharia de resiliência.
* [Litmus](https://github.com/litmuschaos/litmus) - Uma ferramenta de código aberto do CNCF para testes de caos e injeção de falhas para clusters Kubernetes.
* [Este post no blog de desenvolvimento da ISE](https://devblogs.microsoft.com/cse/2023/03/07/build-test-resilience-dotnet-functions/) fornece trechos de código como um exemplo de como usar Polly e Simmy para implementar uma abordagem baseada em hipóteses para resiliência e testes de caos.

## Conclusão

A partir dos princípios do caos: "Quanto mais difícil é perturbar o estado estável, mais confiança temos no comportamento do sistema. Se uma fraqueza é descoberta, agora temos um alvo para melhoria antes que esse comportamento se manifeste no sistema como um todo".

As técnicas de injeção de falhas aumentam a resiliência e a confiança nos produtos que enviamos. Elas são usadas em toda a indústria para validar aplicações e plataformas antes e enquanto são entregues aos clientes.
A injeção de falhas é uma ferramenta poderosa e deve ser usada com cautela. Casos como o [apagão global de 30 minutos da Cloudflare](https://blog.cloudflare.com/cloudflare-outage/), que foi causado devido a uma implantação de código que deveria ser "lançada às escuras", destacam a importância de limitar o raio de explosão no sistema durante experimentos.

## Recursos

* [Post no blog de Mark Russinovich sobre injeção de falhas e engenharia do caos](https://azure.microsoft.com/en-au/blog/advancing-resilience-through-chaos-engineering-and-fault-injection/)
* [Post no blog de Cindy Sridharan sobre testes em produção](https://medium.com/@copyconstruct/testing-in-production-the-safe-way-18ca102d0ef1)
* [Continuação do post no blog de Cindy Sridharan sobre testes em produção](https://medium.com/@copyconstruct/testing-in-production-the-hard-parts-3f06cefaf592)
* [Injeção de falhas na Azure Search](https://azure.microsoft.com/es-es/blog/inside-azure-search-chaos-engineering/)
* [Framework de Arquitetura Azure - Engenharia do Caos](https://learn.microsoft.com/en-us/azure/architecture/framework/resiliency/chaos-engineering)
* [Framework de Arquitetura Azure - Testando resiliência](https://learn.microsoft.com/en-us/azure/architecture/framework/resiliency/testing)
* [Panorama dos Modelos de Causa de Falha de Software](https://www.researchgate.net/publication/301839557_The_landscape_of_software_failure_cause_models)
