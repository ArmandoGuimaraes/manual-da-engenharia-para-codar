# Teste de Integração

O teste de integração é uma metodologia de teste de software usada para determinar o quão bem os componentes ou módulos de um sistema desenvolvidos individualmente se comunicam entre si. Este método de teste confirma que um agregado de um sistema, ou sub-sistema, funciona corretamente em conjunto ou, caso contrário, expõe comportamentos errôneos entre duas ou mais unidades de código.

## Por que Teste de Integração

Como um componente de um sistema pode ser desenvolvido de forma independente ou isolada de outro, é importante verificar a interação de alguns ou todos os componentes. Um sistema complexo pode ser composto por bancos de dados, APIs, interfaces e mais, que interagem entre si ou com sistemas externos adicionais. Os testes de integração expõem problemas no nível do sistema, como esquemas de banco de dados quebrados ou integração defeituosa de APIs de terceiros. Ele garante uma maior cobertura de teste e serve como um importante ciclo de feedback ao longo do desenvolvimento.

## Blocos de Design de Teste de Integração

Considere um aplicativo bancário com três módulos: login, transferências e saldo atual, todos desenvolvidos de forma independente. Um teste de integração pode verificar quando um usuário faz login, ele é redirecionado para o seu saldo atual com o valor correto para o usuário fictício específico. Outro teste de integração pode realizar uma transferência de uma quantia específica de dinheiro. O teste pode confirmar que há fundos suficientes na conta para realizar a transferência e, após a transferência, o saldo atual é atualizado adequadamente para o usuário fictício. A página de login pode ser simulada com um usuário de teste e credenciais fictícias se este módulo não estiver concluído ao testar o módulo de transferências.

O teste de integração é feito pelo desenvolvedor ou pelo testador de QA. No passado, o teste de integração sempre acontecia após o teste de unidade e antes do teste de sistema e E2E. Em comparação com os testes de unidade, os testes de integração são menores em quantidade, geralmente mais lentos e mais caros para configurar e desenvolver. Agora, se uma equipe está seguindo princípios ágeis, os testes de integração podem ser realizados antes ou depois dos testes de unidade, cedo e frequentemente, pois não há necessidade de esperar por processos sequenciais. Além disso, os testes de integração podem utilizar dados fictícios para simular um sistema completo. Há uma abundância de frameworks de teste específicos para cada linguagem que podem ser usados ao longo de todo o ciclo de desenvolvimento.

\*\* É importante notar a diferença entre teste de integração e teste de aceitação. O teste de integração confirma que um grupo de componentes funciona em conjunto como pretendido do ponto de vista técnico, enquanto o teste de aceitação confirma que um grupo de componentes funciona em conjunto como pretendido a partir de um cenário de negócios.

## Aplicando Teste de Integração

Antes de escrever testes de integração, os engenheiros devem identificar os diferentes componentes do sistema e seus comportamentos, entradas e saídas pretendidos. A arquitetura do projeto deve estar totalmente documentada ou especificada em algum lugar que possa ser prontamente consultado (por exemplo, o diagrama de arquitetura).

Existem duas técnicas principais para teste de integração.

### Big Bang

O teste de integração Big Bang é quando todos os componentes são testados como uma única unidade. Isso é melhor para sistemas pequenos, pois um sistema muito grande pode ser difícil de localizar para possíveis erros de testes falhados. Esta abordagem também requer que todos os componentes no sistema em teste estejam concluídos, o que pode atrasar o início dos testes.

![Teste de Integração Big Bang](./images/bigBang.jpg)

### Teste Incremental

O teste incremental é quando dois ou mais componentes que são logicamente relacionados são testados como uma unidade. Após o teste da unidade, componentes adicionais são combinados e testados todos juntos. Este processo se repete até que todos os componentes necessários sejam testados.

#### Top Down

O teste Top Down é quando os componentes de nível superior são testados seguindo o fluxo de controle de um sistema de software. Nesse cenário, o que é comumente chamado de stubs são usados para emular o comportamento de módulos de nível inferior ainda não concluídos ou mesclados no teste de integração.

![Teste de Integração Top Down](./images/topDown.png)

#### Bottom Up

O teste Bottom Up é quando os módulos de nível inferior são testados juntos. Nesse cenário, o que é comumente chamado de drivers são usados para emular o comportamento de módulos de nível superior ainda não concluídos ou incluídos no teste de integração.

![Teste de Integração Bottom Up](./images/bottomUp.jpg)

Uma terceira abordagem conhecida como o modelo sanduíche ou híbrido combina as abordagens de baixo para cima e de cima para baixo para testar componentes de nível inferior e superior ao mesmo tempo.

### Coisas a Evitar

Há um trade-off que o desenvolvedor deve fazer entre a cobertura de código do teste de integração e os ciclos de engenharia. Com dependências fictícias, dados de teste e vários ambientes em teste, muitos testes de integração são inviáveis de manter e se tornam cada vez menos significativos. Muita simulação vai desacelerar o conjunto de testes, tornar o dimensionamento difícil e pode ser um sinal de que o desenvolvedor deve considerar outros testes para o cenário, como testes de aceitação ou E2E.

Os testes de integração de sistemas complexos requerem alta manutenção. Evite testar a lógica de negócios em testes de integração, mantendo os conjuntos de testes separados. Não teste além dos critérios de aceitação da tarefa e certifique-se de limpar quaisquer recursos criados para um determinado teste. Além disso, evite escrever testes em um ambiente de produção. Em vez disso, escreva-os em um ambiente de cópia reduzida.

## Frameworks e Ferramentas de Teste de Integração

Muitas ferramentas e frameworks podem ser usados para escrever tanto testes de unidade quanto de integração. As seguintes ferramentas são para automatizar testes de integração.

- [JUnit](https://junit.org/junit5/)
- [Robot Framework](https://robotframework.org/)
- [moq](https://github.com/moq/moq4)
- [Cucumber](https://cucumber.io/)
- [Selenium](https://www.selenium.dev/)
- [Behave (Python)](https://behave.readthedocs.io/)

## Conclusão

O teste de integração demonstra como um módulo de um sistema, ou sistema externo, se interfaceia com outro. Isso pode ser um teste de dois componentes, um sub-sistema, um sistema inteiro ou uma coleção de sistemas. Os testes devem ser escritos com frequência e ao longo de todo o ciclo de vida do desenvolvimento, usando uma quantidade apropriada de dependências e dados fictícios. Como os testes de integração provam que os módulos desenvolvidos independentemente se interfaceiam conforme tecnicamente projetado, isso aumenta a confiança no ciclo de desenvolvimento, fornecendo um caminho para um sistema que é implantado e escalável.

## Recursos

- [Abordagens de teste de integração](https://www.softwaretestinghelp.com/what-is-integration-testing/)
- [Prós e contras do teste de integração](https://www.geeksforgeeks.org/software-engineering-integration-testing/)
- [Mocks e stubs em testes de integração](https://circleci.com/blog/how-to-test-software-part-i-mocking-stubbing-and-contract-testing/)
- [Software Testing: Principles and Practices](https://www.goodreads.com/book/show/21278464-software-testing)
- [Início rápido do teste Behave](https://github.com/Nick287/Behave-Quick-Start)
