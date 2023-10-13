# Teste de Interface do Usuário (UI)

Esta seção é voltada principalmente para UIs baseadas na web, mas as orientações são semelhantes para aplicativos móveis e baseados em sistemas operacionais.

## Aplicabilidade

O teste de UI nem sempre será aplicável, por exemplo, em aplicações sem uma UI ou partes de uma aplicação que não requerem interação humana. Nesses casos, os testes unitários, funcionais e de integração/e2e seriam os principais meios. O teste de UI será principalmente aplicável ao lidar com uma UI voltada para o público que é usada em um ambiente diversificado ou em uma UI crítica que requer maior fidelidade. Com algo como uma UI de administração que é usada por apenas algumas pessoas, o teste de UI ainda é valioso, mas não tão prioritário.

## Objetivos

O teste de UI fornece a capacidade de garantir que os usuários tenham uma experiência visual consistente em uma variedade de meios de acesso e que a interação do usuário seja consistente com os requisitos funcionais.

- Garantir que a aparência e interação da UI satisfaçam os requisitos funcionais e não funcionais
- Detectar mudanças na UI tanto entre dispositivos e plataformas de entrega quanto entre mudanças de código
- Dar confiança a designers e desenvolvedores de que a experiência do usuário é consistente
- Apoiar a rápida evolução do código e refatoração, reduzindo o risco de regressões

## Evidências e Medidas

Integrar testes de UI no seu CI/CD é necessário, mas mais desafiador do que testes unitários. O desafio aumentado é que os testes de UI precisam ser executados em modo headless com algo como [Puppeteer](https://github.com/puppeteer/puppeteer) ou precisa haver uma orquestração mais extensa com Azure DevOps ou GitHub que cuidaria da integração completa de testes para você, como [BrowserStack](https://www.browserstack.com/automate/azure).

Integrações como `BrowserStack` são interessantes, pois fornecem relatórios do Azure DevOps como parte da execução do teste.

Dito isso, o Azure DevOps suporta uma variedade de adaptadores de teste, então você pode usar qualquer framework de teste de UI que suporte a saída dos resultados do teste para um dos formatos de saída listados em [Publish Test Results task](https://learn.microsoft.com/en-us/azure/devops/pipelines/tasks/test/publish-test-results?view=azure-devops&tabs=yaml).

Se você estiver usando um pipeline do Azure DevOps para executar testes de UI, considere usar um [agente auto-hospedado](https://learn.microsoft.com/en-us/azure/devops/pipelines/agents/agents?view=azure-devops&tabs=browser) para gerenciar as versões do framework e evitar atualizações inesperadas.

## Orientação Geral

O escopo do teste de UI deve ser estratégico. Os testes de UI podem levar uma quantidade significativa de tempo tanto para implementar quanto para executar, e é desafiador testar todos os tipos de interação do usuário em um aplicativo de produção devido ao grande número de interações possíveis.

Projetar os testes de UI em torno dos testes funcionais faz sentido. Por exemplo, dado um formulário de entrada, um teste de UI garantiria que a representação visual seja consistente entre dispositivos, seja acessível e fácil de interagir, e seja consistente entre mudanças de código.

Os testes de UI pegarão bugs de 'tempo de execução' que os testes unitários e funcionais não pegarão. Por exemplo, se o botão de envio de um formulário de entrada for renderizado, mas não for clicável devido a um bug de posicionamento na UI, então isso poderia ser considerado um bug de tempo de execução que não teria sido pego por testes unitários ou funcionais.

Os testes de UI podem ser executados em dados fictícios ou em instantâneos de dados de produção, como em QA ou staging.

### Escrevendo Testes

Bons testes de UI seguem alguns princípios gerais:

- Escolha um framework de teste de UI que permita um feedback rápido e seja fácil de usar
- Projete a UI para ser facilmente testável. Por exemplo, adicione seletores CSS ou defina o id em elementos de uma página da web para permitir uma seleção mais fácil.
- Teste em todos os dispositivos primários que o usuário usa, não teste apenas em um único dispositivo ou sistema operacional.
- Quando um teste altera dados, garanta que os dados sejam criados sob demanda e limpos posteriormente. A consequência de não fazer isso seria testes inconsistentes.

### Problemas Comuns

O teste de UI pode se tornar muito desafiador no nível mais baixo, especialmente com um framework de teste como o Selenium. Se você optar por seguir esse caminho, provavelmente encontrará timeouts, elementos ausentes e terá atrito significativo com o próprio framework de teste. Devido a muitos problemas com o teste de UI, surgiram várias soluções gratuitas e pagas que ajudam a aliviar certos problemas com frameworks como o Selenium. É por isso que você encontrará o Cypress nos frameworks recomendados, pois ele resolve muitos dos problemas conhecidos com o Selenium.

Este é um ponto importante. Dependendo do framework de teste de UI que você escolher, o resultado será uma experiência de criação de teste mais suave ou uma muito frustrante e demorada. Se você optar apenas pelo Selenium, os custos de desenvolvimento e os custos de tempo provavelmente serão muito altos. É melhor usar um framework construído em cima do Selenium ou um que tente resolver muitos dos problemas com algo como o Selenium.

Note que há outras considerações, como quando executado em modo headless, a UI pode renderizar de forma diferente do que você pode ver na sua máquina de desenvolvimento, particularmente com aplicações web. Além disso, note que ao renderizar em diferentes dimensões de página, elementos podem desaparecer na página devido a regras de CSS, portanto, não seriam selecionáveis por certos frameworks com opções padrão fora da caixa. Todos esses problemas podem ser resolvidos e contornados, mas a renderização demonstra outro desafio particular do teste de UI.

## Orientação Específica

Frameworks de teste recomendados:

- Web
  - [BrowserStack](https://www.browserstack.com)
  - [Cypress](https://www.cypress.io)
  - [Jest](https://jestjs.io/docs/en/snapshot-testing)
  - [Selenium](https://www.selenium.dev)
- OS/Aplicações Móveis
  - [Coded UI tests (CUITs)](https://learn.microsoft.com/en-us/visualstudio/test/use-ui-automation-to-test-your-code?view=vs-2019)
  - [Xamarin.UITest](https://learn.microsoft.com/en-us/appcenter/test-cloud/uitest/)

> Note que o framework listado acima que é pago é o BrowserStack, ele está listado porque é um padrão da indústria, o resto são de código aberto e gratuitos.
