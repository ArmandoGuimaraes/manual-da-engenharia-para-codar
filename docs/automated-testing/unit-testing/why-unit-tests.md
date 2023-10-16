# Por que Testes Unitários

Não é segredo que escrever testes unitários é difícil e ainda mais difícil escrevê-los bem. Escrever testes unitários também aumenta o tempo de desenvolvimento para cada funcionalidade. Então, por que deveríamos nos dar ao trabalho de escrevê-los?

## Reduzir Custos

Não há dúvida de que quanto mais tarde um bug é encontrado, mais caro é para corrigi-lo; especialmente se o bug chegar à produção. Um [estudo de pesquisa de 2008 da IBM](ftp://ftp.software.ibm.com/software/rational/info/do-more/RAW14109USEN.pdf) estima que um bug capturado em produção pode custar 6 vezes mais do que se fosse capturado durante a implementação.

## Aumentar a Confiança do Desenvolvedor

Muitas mudanças que os desenvolvedores fazem não são grandes funcionalidades ou algo que requer uma suíte de testes inteira. Uma suíte de testes unitários robusta ajuda a aumentar a confiança do desenvolvedor de que sua mudança não causará bugs a jusante. Ter testes unitários também ajuda a fazer refatorações seguras e mecânicas que são comprovadamente seguras; usando coisas como ferramentas de refatoração para fazer refatoração mecânica e executar testes unitários que cobrem o código refatorado deve ser suficiente para aumentar a confiança no commit.

## Acelerar o Desenvolvimento

Testes unitários levam tempo para escrever, mas também aceleram o desenvolvimento? Embora isso possa parecer um paradoxo, é uma das forças de uma suíte de testes unitários - com o tempo ela continua a crescer e evoluir até que os testes se tornem uma parte essencial do fluxo de trabalho do desenvolvedor.

Se o único teste disponível para um desenvolvedor é um teste de sistema de longa duração, testes de integração que requerem uma implantação ou testes manuais, isso aumentará a quantidade de tempo necessária para escrever uma funcionalidade. Esses tipos de testes devem fazer parte do "Loop Externo"; testes que podem levar algum tempo para serem executados e validar mais do que apenas o código que você está escrevendo. Geralmente, esses tipos de testes de loop externo são executados na etapa de PR ou até mesmo mais tarde durante as mesclagens em branches.

O Loop Interno do Desenvolvedor é o processo pelo qual os desenvolvedores passam enquanto estão escrevendo código. Isso varia de desenvolvedor para desenvolvedor e de linguagem para linguagem, mas normalmente é algo como código -> construir -> executar -> repetir. Quando testes unitários são inseridos no loop interno, os desenvolvedores podem obter feedback e resultados antecipados do código que estão escrevendo. Como os testes unitários são executados muito rapidamente, executar testes não deve ser visto como uma barreira de entrada para este loop. Ferramentas como [Visual Studio Live Unit Testing](https://learn.microsoft.com/en-us/visualstudio/test/live-unit-testing-start?view=vs-2019) também ajudam a encurtar ainda mais o loop interno.

## Documentação como Código

Escrever testes unitários é uma ótima maneira de mostrar como as unidades de código que você está escrevendo devem ser usadas. De certa forma, os testes unitários são melhores do que qualquer documentação ou amostras porque eles são (ou pelo menos deveriam ser) executados a cada construção, então há confiança de que eles não estão desatualizados. Testes unitários também devem ser tão simples que sejam fáceis de seguir.
