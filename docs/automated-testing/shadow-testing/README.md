# Teste de Sombra (Shadow Testing)

O Teste de Sombra é uma abordagem para reduzir riscos antes de ir para a produção. Também conhecido como "Implantação Sombra" ou "Sombreamento de Tráfego", tem semelhanças com o "Lançamento Escuro" (Dark Launching).

## Quando Usar

O Teste de Sombra reduz riscos quando você considera substituir o ambiente atual (V-Atual) por um ambiente candidato com um novo recurso (V-Próximo). Essa abordagem envolve monitorar e capturar diferenças entre os dois ambientes e, em seguida, compará-los para reduzir todos os riscos antes de introduzir um novo recurso/lançamento.

![Visão Geral do Teste de Sombra](images/shadow-testing.png)

## Aplicável a

- **Implantações de Produção**: O V-Próximo no Teste de Sombra sempre funciona separadamente e não afeta a produção. Os usuários não são afetados por esse teste.
- **Infraestrutura**: O Teste de Sombra replica o mesmo tráfego, então no ambiente de teste você pode ter o mesmo tráfego que na produção. Isso ajuda a produzir cenários de teste da vida real.
- **Manuseio de Escala**: Todo o tráfego é replicado, e você tem a chance de ver como seu sistema está escalando.

## Frameworks e Ferramentas de Teste de Sombra

Existem algumas ferramentas para implementar o teste de sombra. O principal objetivo dessas ferramentas é comparar as respostas do V-Atual e do V-Próximo e encontrar as diferenças.

- [Diffy](https://github.com/opendiffy/diffy)
- [Envoy](https://www.envoyproxy.io)
- [McRouter](https://github.com/facebook/mcrouter)
- [Scientist](https://github.com/github/scientist)

Um dos mais populares é o [Diffy](https://github.com/opendiffy/diffy). Ele foi criado e usado no Twitter. Hoje, o autor original e um ex-funcionário do Twitter mantêm sua própria versão deste projeto, chamada [Opendiffy](https://github.com/opendiffy/diffy).

![Arquitetura do Diffy para Teste de Sombra](images/diffy-shadow-testing.png)

## Conclusão

O Teste de Sombra é uma abordagem útil para reduzir riscos quando você considera substituir o ambiente atual com um ambiente candidato usando novos recursos. Algumas vantagens do teste de sombra incluem:

- Impacto zero no ambiente de produção.
- Não é necessário gerar cenários de teste e dados de teste.
- Podemos testar cenários da vida real com dados da vida real.
- Podemos simular a escala com tráfego de produção replicado.

## Referências

- [Martin Fowler - Dark Launching](https://martinfowler.com/bliki/DarkLaunching.html)
- [Martin Fowler - Feature Toggle](https://martinfowler.com/bliki/FeatureToggle.html)
- [Traffic Shadowing/Mirroring](https://istio.io/latest/docs/tasks/traffic-management/mirroring/#:~:text=Traffic%20mirroring%2C%20also%20called%20shadowing,path%20for%20the%20primary%20service.)
