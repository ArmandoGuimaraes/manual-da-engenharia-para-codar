# Testando

## Mapa de Resultados para Técnicas de Teste

A tabela abaixo mapeia resultados -- os resultados que você pode querer alcançar em seus esforços de validação -- para uma ou mais técnicas que podem ser usadas para alcançar esse resultado.

| Quando estou trabalhando em... | Quero obter este resultado... | ...então devo considerar |
|-------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Desenvolvimento | Comprovar compatibilidade retroativa com chamadores e clientes existentes | [Shadow testing](shadow-testing/README.md) | Desenvolvimento; [Integration testing](integration-testing/README.md) | Garantir que a telemetria seja suficientemente detalhada e completa para rastrear e diagnosticar mau funcionamento em fluxos de [End-to-End testing](e2e-testing/README.md) | Desafios de Debug Distribuído; Análise de cadeia de chamadas órfãs |
| Desenvolvimento | Garantir que a lógica do programa esteja correta para uma variedade de entradas esperadas, principais, de borda e inesperadas | [Unit testing](unit-testing/README.md); Testes funcionais; [Consumer-driven Contract Testing](cdc-testing/README.md); [Integration testing](integration-testing/README.md) |
| Desenvolvimento | Prevenir regressões na correção lógica; quanto mais cedo, melhor | [Unit testing](unit-testing/README.md); Testes funcionais; [Consumer-driven Contract Testing](cdc-testing/README.md); [Integration testing](integration-testing/README.md); Anéis (cada um desses são escopos de cobertura em expansão) |
| Desenvolvimento | Validar rapidamente a correção principal de um ponto de funcionalidade (por exemplo, uma única API), manualmente | Testes de fumaça manuais Ferramentas: postman, powershell, curl |
| Desenvolvimento | Validar interações entre componentes isoladamente, garantindo que componentes consumidores e provedores sejam compatíveis e estejam de acordo com um entendimento compartilhado documentado em um contrato | [Consumer-driven Contract Testing](cdc-testing/README.md) |
| Desenvolvimento; [Integration testing](integration-testing/README.md) | Validar que múltiplos componentes funcionem juntos em várias interfaces em uma cadeia de chamadas, incluindo saltos de rede | [Integration testing](integration-testing/README.md); Testes de ponta a ponta ([End-to-End testing](e2e-testing/README.md)); Testes de ponta a ponta segmentados ([End-to-End testing](e2e-testing/README.md)) |
| Desenvolvimento | Comprovar recuperação de desastres – recuperação de corrupção de dados | Simulações de DR |
| Desenvolvimento | Encontrar vulnerabilidades na autenticação ou autorização do serviço | Cenário (segurança) |
| Desenvolvimento | Comprovar interpretação correta de RBAC e reivindicações no código de autorização | Cenário (segurança) |
| Desenvolvimento | Documentar e/ou impor uso válido da API | [Unit testing](unit-testing/README.md); Testes funcionais; [Consumer-driven Contract Testing](cdc-testing/README.md) |
| Desenvolvimento | Comprovar a correção da implementação antes de uma dependência ou na ausência de uma dependência | [Unit testing](unit-testing/README.md) (com mocks); [Unit testing](unit-testing/README.md) (com emuladores); [Consumer-driven Contract Testing](cdc-testing/README.md) |
| Desenvolvimento | Garantir que a interface do usuário seja acessível | [Accessibility](../accessibility/README.md) |
| Desenvolvimento | Garantir que os usuários possam operar a interface | [UI testing (automated)](ui-testing/README.md) (observação de usabilidade humana) |
| Desenvolvimento | Prevenir regressão na experiência do usuário | Automação de UI; [End-to-End testing](e2e-testing/README.md) |
| Desenvolvimento | Detectar e prevenir fenômenos de 'vizinho barulhento' | [Load testing](performance-testing/load-testing.md) |
| Desenvolvimento | Detectar quedas de disponibilidade | [Synthetic Transaction testing](synthetic-monitoring-tests/README.md); Sondas externas |
| Desenvolvimento | Prevenir regressão em casos de uso / fluxos de trabalho de cenário 'composto' (por exemplo, um sistema de comércio eletrônico pode ter muitas APIs que, usadas juntas em uma sequência, executam um cenário de "compra e venda") | [End-to-End testing](e2e-testing/README.md); Cenário |
| Desenvolvimento; Operações | Prevenir regressões em métricas de desempenho em tempo de execução, por exemplo, latência / custo / consumo de recursos; quanto mais cedo, melhor | Anéis; [Synthetic Transaction testing](synthetic-monitoring-tests/README.md) / Transação; Cães de guarda de reversão |
| Desenvolvimento; Otimização | Comparar qualquer métrica dada entre 2 implementações candidatas ou variações na funcionalidade | Testes A/B; Flighting |
| Desenvolvimento; Staging | Comprovar que o sistema de produção de capacidade provisionada atende aos objetivos de confiabilidade, disponibilidade, consumo de recursos, desempenho | [Load testing (stress)](performance-testing/load-testing.md); Pico; Soak; [Performance testing](performance-testing/README.md) |
| Desenvolvimento; Staging | Entender as principais características de desempenho da experiência do usuário – latência, conversação, resiliência a erros de rede | Carga; [Performance testing](performance-testing/README.md); Cenário (particionamento de rede) |
| Desenvolvimento; Staging; Operação | Descobrir pontos de fusão (as cargas nas quais ocorre falha ou consumo máximo tolerável de recursos) para cada componente individual na pilha | Squeeze; [Load testing (stress)](performance-testing/load-testing.md) |
| Desenvolvimento; Staging; Operação | Descobrir o ponto de fusão geral do sistema (as cargas nas quais o sistema de ponta a ponta falha) e qual componente é o elo mais fraco em toda a pilha | Squeeze; [Load testing (stress)](performance-testing/load-testing.md) |
| Desenvolvimento; Staging; Operação | Medir limites de capacidade para provisionamento dado para prever ou satisfazer necessidades futuras de provisionamento | Squeeze; [Load testing (stress)](performance-testing/load-testing.md) |
| Desenvolvimento; Staging; Operação | Criar / exercitar manual de failover | Simulações de Failover |
| Desenvolvimento; Staging; Operação | Comprovar recuperação de desastres – perda de data center (o cenário do meteoro); medir MTTR | Simulações de DR |
| Desenvolvimento; Staging; Operação | Entender se os painéis de observabilidade estão corretos e se a telemetria está completa e fluindo | Validação de Rastreamento; [Load testing (stress)](performance-testing/load-testing.md); Cenário; [End-to-End testing](e2e-testing/README.md) |
| Desenvolvimento; Staging; Operação | Medir impacto da sazonalidade do tráfego | [Load testing](performance-testing/load-testing.md) |
| Desenvolvimento; Staging; Operação | Comprovar que transações e alertas notificam / tomam ações corretamente | [Synthetic Transaction testing](synthetic-monitoring-tests/README.md) (casos negativos); [Load testing](performance-testing/load-testing.md) |
| Desenvolvimento; Staging; Operação; Otimização | Entender a curva de escalabilidade, ou seja, como o sistema consome recursos com carga | [Load testing (stress)](performance-testing/load-testing.md); [Performance testing](performance-testing/README.md) |
| Operação; Otimização | Descobrir o comportamento do sistema ao longo do tempo | Soak |
| Otimização | Encontrar oportunidades de economia de custos | Squeeze |
| Staging; Operação | Medir impacto de failover / aumento de escala (reparticionamento, aumento de provisionamento) / redução de escala | Simulações de Failover; Simulações de Escala |
| Staging; Operação | Criar/Exercitar manual para aumento/redução de provisionamento | Simulações de Escala |
| Staging; Operação | Medir comportamento sob mudanças rápidas no tráfego | Spike |
| Staging; Otimização | Descobrir métricas de custo por unidade de volume de carga (quais fatores influenciam o custo em quais pontos de carga, por exemplo, custo por milhão de usuários simultâneos) | Carga (stress) |
| Desenvolvimento; Operação | Descobrir pontos onde um sistema não é resiliente a falhas imprevisíveis, mas inevitáveis (queda de rede, falha de hardware, manutenção de host de VM, falhas de rack/switch, atos aleatórios do Divino Malévolo, flares solares, tubarões que comem relés de cabos submarinos, radiação cósmica, quedas de energia, operadores de retroescavadeiras renegados, lobos mastigando caixas de junção, ...) | Caos |
| Desenvolvimento | Realizar testes unitários em conectores personalizados da plataforma Power | [Custom Connector Testing](unit-testing/custom-connector.md) |

## Seções dentro de Testes

- [Consumer-driven contract (CDC) testing](cdc-testing/README.md)
- [End-to-End testing](e2e-testing/README.md)
- [Fault Injection testing](fault-injection-testing/README.md)
- [Integration testing](integration-testing/README.md)
- [Performance testing](performance-testing/README.md)
- [Shadow testing](shadow-testing/README.md)
- [Smoke testing](smoke-testing/README.md)
- [Synthetic Transaction testing](synthetic-monitoring-tests/README.md)
- [UI testing](ui-testing/README.md)
- [Unit testing](unit-testing/README.md)

## Testes Específicos de Tecnologia

- [Usando o padrão DevTest para construir contêineres com AzDO](tech-specific-samples/azdo-container-dev-test-release)
- [Usando Azurite para executar testes de armazenamento de blob no pipeline](tech-specific-samples/blobstorage-unit-tests/README.md)
