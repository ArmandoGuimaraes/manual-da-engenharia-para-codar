# Teste de Fumaça (Smoke Testing)

Testes de fumaça, às vezes chamados de testes de ***Sanidade***, ***Aceitação*** ou ***Verificação de Build/Lançamento***, são um subtipo de testes de sistema/funcionais geralmente usados como portões que verificam a prontidão da aplicação como um passo preliminar. Se uma aplicação passa nos testes de fumaça, ela é aceitável ou está em um estado estável o suficiente para as próximas etapas de teste ou implantação.

## Quando Usar

### Problema Abordado

Os testes de fumaça têm como objetivo descobrir, o mais cedo possível, se uma aplicação está funcionando ou não. O objetivo dos testes de fumaça é economizar tempo; se a versão atual da aplicação não passar nos testes de fumaça, então o resto da cadeia de integração ou implantação para ela pode ser abandonado. Os testes de fumaça não visam fornecer cobertura total de funcionalidade, mas sim focar em algumas invocações de aceitação rápidas para as quais a aplicação deve, em todos os momentos, responder corretamente.

### Ponto de Inflexão do ROI

Os testes de fumaça cobrem apenas o caminho crítico da aplicação e não devem ser usados para realmente testar o comportamento da aplicação, mantendo o tempo de execução e a complexidade no mínimo. Os testes podem ser formados por um subconjunto dos testes de integração ou e2e da aplicação, e eles cobrem tanto da funcionalidade com a menor profundidade possível.

A regra de ouro de um bom teste de fumaça é que ele economiza tempo na validação de que a aplicação é aceitável para um estágio onde testes melhores e mais completos começarão.

### Aplicável a

- [x] **Desktop de desenvolvimento local** - *Exemplo:* Aplicação de teste de fumaça manual para verificar se a aplicação está OK.
- [x] **Pipelines de Build** - *Exemplo:* Execução de um pequeno conjunto da suíte de teste de integração antes de executar a cobertura total de testes, que pode levar muito tempo.
- [x] **Implantações em ambientes não-produtivos e produtivos** - *Exemplo:* Execução de um comando curl para a API do produto e afirmando que a resposta é 200 antes de executar testes de carga que consomem recursos.
- [x] **Validação de PR** - *Exemplo:* Implantação do gráfico da aplicação em um namespace de teste e validação de que o lançamento é bem-sucedido e que nenhuma regressão imediata é mesclada.

## Conclusão

O teste de fumaça é uma etapa de baixo esforço e alto impacto para enviar software mais confiável. Ele deve ser considerado entre as primeiras etapas a serem implementadas ao planejar sistemas continuamente integrados e entregues.

## Recursos

- [Wikipedia - Teste de Fumaça](https://en.wikipedia.org/wiki/Smoke_testing_(software))
- [Livro SRE do Google - Testes de Sistema](https://landing.google.com/sre/sre-book/chapters/testing-reliability/#system-tests)
