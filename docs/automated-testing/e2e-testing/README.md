# Teste de Ponta a Ponta (E2E)

O teste de ponta a ponta (E2E) é uma metodologia de teste de software para verificar um fluxo funcional e de dados de uma aplicação composta por vários subsistemas trabalhando juntos do início ao fim.

Muitas vezes, esses sistemas são desenvolvidos em diferentes tecnologias por diferentes equipes ou organizações. Finalmente, eles se unem para formar uma aplicação de negócios funcional. Portanto, testar um único sistema não seria suficiente. Assim, o teste de ponta a ponta verifica a aplicação do início ao fim, colocando todos os seus componentes juntos.

![Teste de Ponta a Ponta](./images/e2e-testing.png)

## Por que Teste E2E [O Porquê]

Em muitos cenários de aplicativos de software comerciais, um sistema de software moderno consiste em sua interconexão com vários subsistemas. Esses subsistemas podem estar dentro da mesma organização ou podem ser componentes de diferentes organizações. Além disso, esses subsistemas podem ter ciclos de lançamento de vida útil semelhantes ou diferentes do sistema atual. Como resultado, se houver qualquer falha ou defeito em qualquer subsistema, isso pode afetar adversamente todo o sistema de software, levando ao seu colapso.

![Pirâmide de Teste E2E](./images/testing-pyramid.png)

A ilustração acima é uma pirâmide de teste do [blog de Kent C. Dodd](https://blog.kentcdodds.com/write-tests-not-too-many-mostly-integration-5e8c7fff591c), que é uma combinação das pirâmides do [blog de Martin Fowler](https://martinfowler.com/bliki/TestPyramid.html) e do [Google Testing Blog](https://testing.googleblog.com/2015/04/just-say-no-to-more-end-to-end-tests.html).

A maioria dos seus testes está na parte inferior da pirâmide. À medida que você sobe na pirâmide, o número de testes diminui. Além disso, subindo na pirâmide, os testes ficam mais lentos e mais caros para escrever, executar e manter. Cada tipo de teste varia para o seu propósito, aplicação e as áreas que se destina a cobrir. Para mais informações sobre a análise comparativa de diferentes tipos de testes, consulte este documento [## Unidade vs Integração vs Sistema vs Teste E2E](../README.md).

## Blocos de Design de Teste E2E [O Quê]

![Blocos de Design de Teste E2E](./images/e2e-blocks.png)

Vamos examinar todas as 3 categorias uma por uma:

### Funções do Usuário

As seguintes ações devem ser realizadas como parte da construção de funções do usuário:

- Liste as funções iniciadas pelo usuário dos sistemas de software e seus subsistemas interconectados.
- Para qualquer função, acompanhe as ações realizadas, bem como os dados de entrada e saída.
- Encontre as relações, se houver, entre diferentes funções do usuário.
- Descubra a natureza das diferentes funções do usuário, ou seja, se são independentes ou reutilizáveis.

### Condições

As seguintes atividades devem ser realizadas como parte da construção de condições com base nas funções do usuário:

- Para cada uma das funções do usuário, um conjunto de condições deve ser preparado.
- O tempo, as condições de dados e outros fatores que afetam as funções do usuário podem ser considerados como parâmetros.

### Casos de Teste

Os seguintes fatores devem ser considerados para a construção de casos de teste:

- Para cada cenário, um ou mais casos de teste devem ser criados para testar cada funcionalidade das funções do usuário. Se possível, esses casos de teste devem ser automatizados por meio dos processos padrão de pipeline de CI/CD com o acompanhamento de cada build bem-sucedido e falho no AzDO.
- Cada condição deve ser listada como um caso de teste separado.

## Aplicando o Teste E2E [O Como]

Como qualquer outro teste, o teste E2E também passa por fases formais de planejamento, execução de teste e encerramento.

O teste E2E é feito com as seguintes etapas:

### Planejamento

- Análise de requisitos de negócios e funcionais
- Desenvolvimento do plano de teste
- Desenvolvimento do caso de teste
- Configuração do ambiente de teste semelhante à produção
- Configuração dos dados de teste
- Decidir critérios de saída
- Escolher os métodos de teste mais aplicáveis ao seu sistema. Para a definição dos vários métodos de teste, consulte o documento [Métodos de Teste](./testing-methods.md).

### Pré-requisito

- O teste do sistema deve estar completo para todos os sistemas participantes.
- Todos os subsistemas devem ser combinados para funcionar como uma aplicação completa.
- O ambiente de teste semelhante à produção deve estar pronto.

### Execução do Teste

- Execute os casos de teste
- Registre os resultados do teste e decida sobre a aprovação e falha
- Relate os bugs na ferramenta de relatório de bugs
- Re-verifique as correções de bugs

### Encerramento do Teste

- Preparação do relatório de teste
- Avaliação dos critérios de saída
- Encerramento da fase de teste

### Métricas de Teste

Rastrear as métricas de qualidade dá uma visão sobre o status atual do teste. Algumas métricas comuns de teste E2E são:

- **Status de preparação do caso de teste**: Número de casos de teste prontos versus o número total de casos de teste.
- **Progresso frequente do teste**: Número de casos de teste executados de maneira frequente e consistente, por exemplo, semanalmente, versus um número-alvo de casos de teste no mesmo período de tempo.
- **Status dos defeitos**: Esta métrica representa o status dos defeitos encontrados durante o teste. Os defeitos devem ser registrados na ferramenta de rastreamento de defeitos (por exemplo, backlog do AzDO) e resolvidos de acordo com sua gravidade e prioridade. Portanto, a porcentagem de defeitos abertos e fechados de acordo com sua gravidade e prioridade deve ser calculada para rastrear esta métrica. A consulta do painel AzDO pode ser usada para rastrear essa métrica.
- **Disponibilidade do ambiente de teste**: Esta métrica rastreia a duração do ambiente de teste usado para o teste de ponta a ponta versus sua duração de alocação programada.

## Frameworks e Ferramentas de Teste E2E

### 1. Gauge Framework

![Gauge Framework](./images/gauge.jpg)

Gauge é um framework gratuito e de código aberto para escrever e executar testes E2E. Algumas características-chave do Gauge que o tornam único incluem:

- Sintaxe simples, flexível e rica baseada em Markdown.
- Suporte consistente entre plataformas/linguagens para escrever código de teste.
- Uma arquitetura modular com suporte a plugins.
- Suporta execução orientada a dados e fontes de dados externas.
- Ajuda você a criar suítes de teste sustentáveis.
- Suporta Visual Studio Code, Intellij IDEA, IDE Support.
- Suporta relatórios em html, json e XML.

[Site do Gauge Framework](https://gauge.org/)

### 2. Robot Framework

![Robot Framework](./images/robot.jpg)

Robot Framework é um framework de automação de código aberto genérico. O framework tem uma sintaxe fácil, utilizando palavras-chave legíveis por humanos. Suas capacidades podem ser estendidas por bibliotecas implementadas com Python ou Java.

Robot compartilha muitos dos mesmos "prós" que o Gauge, exceto as ferramentas de desenvolvimento e a sintaxe. Em nosso uso, descobrimos que o Intellisense do VS Code oferecido com o Gauge era muito mais estável do que as ofertas para o Robot. Também achamos a sintaxe menos legível do que o que o Gauge oferecia. Embora ambos os frameworks permitam definições de casos de teste baseadas em marcação, a sintaxe do Gauge lê muito mais como uma frase em português do que o Robot. Finalmente, o Intellisense está incorporado nos arquivos de marcação para casos de teste do Gauge, o que criará um stub de função para a definição de teste real se o desenvolvedor permitir. O mesmo não pode ser dito do Robot Framework.

[Site do Robot Framework](https://robotframework.org/#introduction)

### 3. TestCraft

![TestCraft](./images/TestCraft-logo.png)

TestCraft é uma plataforma de automação de teste Selenium sem código. Sua tecnologia revolucionária de IA e modelagem visual única permitem uma criação e execução de teste mais rápida, eliminando a sobrecarga de manutenção de teste.

Os testadores criam cenários de teste totalmente automatizados sem codificação. Os clientes encontram bugs mais rapidamente, lançam com mais frequência, integram-se com a abordagem CI/CD e melhoram a qualidade geral de seus produtos digitais. Isso cria uma experiência completa de teste de ponta a ponta.

[Site do Perfecto (TestCraft)](https://www.perfecto.io/) ou obtenha-o no [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=testcraft.build-release-task)

### 4. Ranorex Studio

![Ranorex Studio](./images/ranorex-studio2.png)

**Ranorex Studio** é uma ferramenta completa de automação de teste de ponta a ponta para aplicações desktop, web e móveis. Crie testes confiáveis rapidamente sem qualquer codificação ou usando o IDE completo. Use arquivos CSV ou Excel externos ou um banco de dados SQL como entradas para seus testes.

Execute testes em paralelo ou em uma grade Selenium com o Selenium WebDriver integrado. O Ranorex Studio se integra ao seu processo de CI/CD para encurtar seus ciclos de lançamento sem sacrificar a qualidade.

Os testes do **Ranorex Studio** também se integram ao Azure DevOps (AzDO), que podem ser executados como parte de um pipeline de build no AzDO.

[Site do Ranorex Studio](https://www.ranorex.com/free-trial)

### 5. Katalon Studio

![Katalon](./images/New-Logo-Katalon-Studio.png)

**Katalon Studio** é uma excelente solução de automação de ponta a ponta para testes web, API, móveis e desktop com suporte DevOps.

Com o Katalon Studio, o teste automatizado pode ser facilmente integrado a qualquer pipeline de CI/CD para lançar produtos mais rapidamente, garantindo alta qualidade. O Katalon Studio personaliza para usuários de iniciantes a especialistas. Funções robustas como Espionagem, Gravação, Interface de editor duplo e Palavras-chave personalizadas tornam possível a configuração, criação e manutenção de testes para os usuários.

Construído em cima do Selenium e Appium, o Katalon Studio ajuda a padronizar seus testes de ponta a ponta. Ele também está em conformidade com os frameworks mais populares para trabalhar de forma integrada com outras ferramentas no ecossistema de teste automatizado.

O Katalon é endossado pela Gartner, profissionais de TI e uma grande comunidade de teste.

> Nota: No momento da escrita deste texto, a extensão do Katalon Studio para AzDO NÃO estava disponível para Linux.

[Site do Katalon Studio](https://www.katalon.com/) ou leia sobre sua [integração com o AzDO](https://docs.katalon.com/katalon-studio/docs/azure-devops-extension.html#installation)

### 6. BugBug.io

![BugBug](./images/bugbug-logo-208x65.png)

**BugBug** é uma maneira fácil de automatizar testes para aplicações web. A ferramenta foca na simplicidade, mas ainda permite que você cubra todos os casos de teste essenciais sem codificação. É uma solução completa - você pode criar testes facilmente e usar a nuvem integrada para executá-los de acordo com um cronograma ou a partir do seu CI/CD, sem alterações na sua própria infraestrutura.

BugBug é uma alternativa interessante ao Selenium porque é realmente uma tecnologia completamente diferente. Ele é baseado em uma extensão do Chrome que permite ao BugBug gravar e executar testes mais rapidamente do que os frameworks da velha escola.

A maior vantagem do BugBug é sua facilidade de uso. A maioria dos testes criados com o BugBug simplesmente funciona imediatamente. Isso torna mais fácil para pessoas não técnicas manterem os testes - com o BugBug, você pode economizar dinheiro na contratação de um engenheiro de QA.

[Site do BugBug](https://bugbug.io?utm_source=microsoft_github&utm_medium=referral)

## Conclusão

Espero que você tenha aprendido vários aspectos do teste E2E, como seus processos, métricas, a diferença entre testes Unitários, de Integração e E2E, e os vários frameworks e ferramentas de teste E2E recomendados.

Para qualquer lançamento comercial do software, a verificação do teste E2E desempenha um papel importante, pois testa toda a aplicação em um ambiente que imita exatamente os usuários do mundo real, como comunicação de rede, interação com middleware e serviços de back-end, etc.

Por fim, o teste E2E é frequentemente realizado manualmente, pois o custo de automatizar tais casos de teste é muito alto para ser suportado por qualquer organização. Dito isso, o objetivo final de cada organização é tornar o teste E2E o mais eficiente possível, adicionando componentes de teste totalmente e semi-automatizados ao processo. Portanto, os vários frameworks e ferramentas de teste E2E listados neste artigo vêm em socorro.

## Recursos

- [Wikipedia: Teste de Software](https://en.wikipedia.org/wiki/Software_testing)
- [Wikipedia: Teste Unitário](https://en.wikipedia.org/wiki/Unit_testing)
- [Wikipedia: Teste de Integração](https://en.wikipedia.org/wiki/Integration_testing)
- [Wikipedia: Teste de Sistema](https://en.wikipedia.org/wiki/System_testing)
