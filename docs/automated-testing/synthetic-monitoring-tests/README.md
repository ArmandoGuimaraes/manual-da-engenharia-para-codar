# Testes de Monitoramento Sintético

Testes de Monitoramento Sintético são um conjunto de testes funcionais que têm como alvo um sistema ativo em produção. O foco desses testes, às vezes chamados de "watchdog", "monitoramento ativo" ou "transações sintéticas", é verificar continuamente a saúde e a resiliência do produto.

## Por que Testes de Monitoramento Sintético

Tradicionalmente, os provedores de software confiam em testes por meio de etapas de CI/CD na conhecida [pirâmide de testes](https://martinfowler.com/bliki/TestPyramid.html) (unitários, integração, e2e) para validar que o produto está saudável e sem regressões. No entanto, à medida que mais organizações hoje fornecem produtos altamente disponíveis (SLA de 99,9+), elas descobrem que a natureza de aplicações distribuídas de longa duração, que normalmente dependem de vários componentes de hardware e software, é falhar.

Para tais sistemas, a ambição das equipes de engenharia de serviço é reduzir ao mínimo o tempo necessário para corrigir erros, ou o [MTTR - Tempo Médio para Reparo](https://en.wikipedia.org/wiki/Mean_time_to_repair). É um esforço contínuo, realizado no sistema ativo/produção. Monitores Sintéticos podem ser usados para detectar os seguintes problemas:

- Disponibilidade - Se o sistema ou região específica está disponível.
- Transações e jornadas do cliente - Requisições conhecidas como boas devem funcionar, enquanto requisições conhecidas como ruins devem gerar erro.
- Desempenho - Quão rápido são as ações e se esse desempenho é mantido sob cargas elevadas e através de lançamentos de versão.
- Componentes de Terceiros - Componentes de nuvem ou software usados pelo sistema podem falhar.

### Testes Shift-Right

Testes de Monitoramento Sintético são um subconjunto de testes que são executados em produção, às vezes chamados de Test-in-Production ou testes Shift-Right. Com paradigmas de [Shift-Left](https://en.wikipedia.org/wiki/Shift-left_testing) que são tão populares, a abordagem é realizar testes o mais cedo possível no ciclo de desenvolvimento da aplicação. Shift-Right complementa e adiciona ao Shift-Left, referindo-se à execução de testes tarde no ciclo, durante e após o lançamento, quando o produto está atendendo ao tráfego de produção.

## Blocos de Design de Testes de Monitoramento Sintético

Um teste de monitoramento sintético é um teste que usa dados sintéticos e contas de teste reais para injetar comportamentos de usuário no sistema e validar seu efeito. Os componentes dos testes de monitoramento sintético incluem **Sondas**, código de teste/contas que geram dados, e **Ferramentas de Monitoramento** colocadas para validar tanto o comportamento do sistema sob teste quanto a saúde das próprias sondas.

### Sondas

Sondas são a fonte de ações de usuário sintéticas que conduzem os testes. Elas têm como alvo a interface do usuário do produto ou APIs voltadas para o público e estão executando em seu próprio ambiente de produção. Um teste de monitoramento sintético está, de fato, muito relacionado a testes de caixa-preta e normalmente se concentra em cenários de ponta a ponta do ponto de vista de um usuário.

### Monitoramento

Dado que os testes de monitoramento sintético estão sendo executados continuamente, em intervalos, em um ambiente de produção, a afirmação do comportamento do sistema por meio da análise depende dos pilares de monitoramento existentes usados no sistema ativo (Logging, Métricas, Rastreamento Distribuído).

## Aplicando Testes de Monitoramento Sintético

### Afirmando o sistema sob testes

Testes de monitoramento sintético são geralmente estatísticos. As métricas de teste são comparadas com alguma média histórica ou em execução com uma dimensão de tempo.

### Construindo uma Solução de Monitoramento Sintético

Em um nível alto, a construção de monitores sintéticos geralmente consiste nas seguintes etapas:

- Determinar a métrica a ser validada (resultado funcional, latência, etc.)
- Construir uma automação que meça essa métrica contra o sistema e colete telemetria na infraestrutura de monitoramento existente do sistema.
- Configurar alarmes/ações/respostas de monitoramento que detectem a falha do sistema em atender ao objetivo desejado da métrica.
- Executar a automação de casos de teste continuamente em um intervalo apropriado.

### Monitorando a saúde dos testes

O tempo de execução das sondas é um ambiente de produção por si só, e a saúde dos testes é crítica. Muitos provedores oferecem sistemas baseados em nuvem que hospedam esses tempos de execução, enquanto algumas organizações usam ambientes de produção existentes para executar esses testes. De qualquer forma, uma estratégia de monitorar o monitor deve ser uma parte essencial dos sistemas de alerta do ambiente de produção.

### Monitoramento Sintético e Monitoramento de Usuários Reais

O monitoramento sintético não substitui a necessidade de RUM. Sondas são códigos previsíveis que verificam cenários específicos, e elas não representam 100% completamente e verdadeiramente como uma sessão de usuário é tratada.

### Riscos

Testar em produção, em geral, tem um fator de risco associado a ele, que não existe em testes executados durante as etapas de CI/CD. Especificamente, em testes de monitoramento sintético, o seguinte pode afetar o ambiente de produção:

- Dados corrompidos ou inválidos - Testes injetam dados de teste que podem ser de alguma forma corrompidos.
- Vazamento de dados protegidos - Testes

 são executados em um ambiente de produção e emitem logs ou rastreamentos que podem conter dados protegidos.
- Sistemas sobrecarregados - Testes sintéticos podem causar erros ou sobrecarregar o sistema.

## Frameworks e Ferramentas de Testes de Monitoramento Sintético

A maioria dos principais players de monitoramento/APM tem um produto empresarial que suporta monitoramento sintético integrado aos seus sistemas. No entanto, tais soluções são tipicamente caras.

Algumas organizações preferem executar sondas em infraestrutura existente usando ferramentas conhecidas como [Postman](https://www.postman.com/), [Wrk](https://github.com/wg/wrk), [JMeter](https://jmeter.apache.org/), [Selenium](https://www.selenium.dev/) ou até mesmo código personalizado para gerar os dados sintéticos.

- [Application Insights availability](https://learn.microsoft.com/en-us/azure/azure-monitor/app/monitor-web-app-availability)
- [DataDog Synthetics](https://www.datadoghq.com/dg/apm/synthetics/api-test/)
- [Dynatrace Synthetic Monitoring](https://www.dynatrace.com/platform/synthetic-monitoring/)
- [New Relic Synthetics](https://newrelic.com/products/synthetics)
- [Checkly](https://checklyhq.com)

## Conclusão

O valor dos testes em produção, em geral, e especificamente do monitoramento sintético, só existe para tipos específicos de engajamento, e há riscos e custos associados a eles. No entanto, quando aplicáveis, eles fornecem garantia contínua de que não há falhas no sistema do ponto de vista do usuário.

## Recursos

- [Livro SRE do Google - Testando Confiabilidade](https://landing.google.com/sre/sre-book/chapters/testing-reliability/)
- [Arquiteturas DevOps da Microsoft - Shift Right para Testar em Produção](https://learn.microsoft.com/en-us/devops/deliver/shift-right-test-production)
- [Martin Fowler - Monitoramento Sintético](https://martinfowler.com/bliki/SyntheticMonitoring.html)
