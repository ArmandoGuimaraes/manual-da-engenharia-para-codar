# Simulação em Testes de Unidade

Um dos componentes-chave para escrever testes de unidade é remover as dependências que seu sistema possui e substituí-las por uma implementação que você controla. O método mais comum que as pessoas usam como substituto para a dependência é um mock, e existem frameworks de simulação para tornar esse processo mais fácil.

Muitos frameworks e artigos usam diferentes significados para as diferenças entre test doubles. Um test double é um termo genérico para qualquer objeto "fingido" usado no lugar de um real. Este termo, bem como outros usados nesta página, são as [definições fornecidas por Martin Fowler](https://martinfowler.com/articles/mocksArentStubs.html#TheDifferenceBetweenMocksAndStubs). A forma mais comumente usada de test double são os Mocks, mas há muitos casos em que Mocks talvez não sejam a melhor escolha e Fakes devem ser considerados em vez disso.

## Stubs

Stub permite que você tenha um comportamento predeterminado que substitui o comportamento real. A dependência (classe abstrata ou interface) é implementada como um stub com uma lógica conforme esperado pelo cliente. Stubs podem ser úteis quando os clientes dos stubs esperam o mesmo conjunto de respostas, por exemplo, você usa um serviço de terceiros. O conceito-chave aqui é que os stubs nunca devem falhar em um teste de unidade ou integração onde um mock pode falhar. Stubs não requerem nenhum tipo de framework para serem executados, mas geralmente são suportados por frameworks de simulação para construir rapidamente os stubs. Stubs são comumente usados em combinação com frameworks ou bibliotecas de injeção de dependência, onde o objeto real é substituído por uma implementação de stub.

Stubs podem ser especialmente úteis durante o desenvolvimento inicial de um sistema, mas como quase todo teste requer seus próprios stubs (para testar os diferentes estados), isso rapidamente se torna repetitivo e envolve muito código boilerplate. Raramente você encontrará um código-fonte que usa apenas stubs para simulação, eles geralmente são emparelhados com outros test doubles.

### Vantagens

- Não requerem nenhum framework, fácil de configurar.

### Desvantagens

- Pode envolver a reescrita do mesmo código muitas vezes, muito código boilerplate.

## Mocks

Fowler descreve **mocks** como objetos pré-programados com expectativas que formam uma especificação das chamadas que se espera receber. Em outras palavras, mocks são um objeto de substituição para a dependência que tem certas expectativas que são colocadas nele; essas expectativas podem ser coisas como validar que um submétodo foi chamado um determinado número de vezes ou que argumentos são passados de uma determinada maneira.

Frameworks de simulação são abundantes para cada linguagem, com algumas linguagens tendo mocks incorporados nos pacotes de teste de unidade. Eles tornam a escrita de testes de unidade fácil e ainda incentivam boas práticas de teste de unidade.

A principal diferença entre um mock e a maioria dos outros test doubles é que mocks fazem **verificação comportamental**, enquanto outros test doubles fazem **verificação de estado**. Com a verificação comportamental, você acaba testando que a implementação do sistema sob teste é como você espera, enquanto com a verificação de estado a implementação não é testada, apenas as entradas e saídas para o sistema são validadas.

A maior desvantagem da verificação comportamental é que ela está atrelada à implementação. Uma das maiores vantagens de escrever testes de unidade é que, quando você faz alterações no código, tem confiança de que, se seus testes de unidade continuarem a passar, você está fazendo uma mudança relativamente segura. Se os testes precisam ser atualizados toda vez porque o comportamento do método mudou, então você perde essa confiança porque bugs também podem ser introduzidos no código do teste. Isso também aumenta o tempo de desenvolvimento e pode ser uma fonte de frustração.

### Vantagens da Simulação

- Fácil de escrever.
- Incentiva o design testável.

### Desvantagens da Simulação

- Testes comportamentais podem apresentar problemas com a manutenibilidade no código do teste de unidade.
- Geralmente requer um framework para ser instalado (ou, se nenhum framework, muito código boilerplate)

## Fakes

**Objetos Fake** realmente têm implementações funcionais, mas geralmente usam algum atalho que pode torná-los inadequados para produção. Um dos exemplos comuns de uso de um Fake é um banco de dados em memória - normalmente você quer que seu banco de dados seja capaz de salvar dados em algum lugar entre as execuções do aplicativo, mas ao escrever testes de unidade, se você tem uma implementação fake de suas APIs de banco de dados que armazenam todos os dados na memória, você pode usar esses para testes de unidade e não quebrar a abstração, bem como ainda manter seus testes rápidos.

Escrever um fake leva mais tempo do que outros test doubles, porque eles são implementações completas e podem ter seu próprio conjunto de testes de unidade. Nesse sentido, porém, eles aumentam a confiança em seu código ainda mais porque seu test double foi minuciosamente testado para bugs antes de você mesmo usá-lo como uma dependência downstream.

### Vantagens dos Fakes

- Nenhum framework necessário, é como qualquer outra implementação.
- Incentiva o design testável.
- O código pode ser "promovido" para o código do produto, então não é um esforço desperdiçado.

### Desvantagens dos Fakes

- Leva mais tempo para implementar.

## Melhores Práticas

Para manter sua simulação eficiente, considere essas melhores práticas para tornar seu código testável, economizar tempo e tornar suas asserções de teste mais significativas.

### Injeção de Dependência

Se você não mantiver a testabilidade em mente desde o início, uma vez que você começar a escrever seus testes, poderá perceber que precisa fazer uma refatoração demorada para tornar o código testável em unidade. Um problema comum que pode levar a código não testável em certas linguagens, como C#, é não usar injeção de dependência. Considere usar injeção de dependência para que um mock possa ser facilmente injetado em seu Subject Under Test (SUT) durante um teste de unidade.

## Conclusão

Usar test doubles em testes de unidade é uma parte essencial

 de ter um conjunto de testes saudável. Ao olhar para frameworks de simulação e usar test doubles, é importante considerar as implicações futuras de integrar com um framework de simulação desde o início. Às vezes, certos recursos de frameworks de simulação parecem essenciais, mas geralmente isso é um sinal de que o código em si não é abstrato o suficiente se requer um framework.

Se possível, começar sem um framework de simulação e tentar criar implementações fake levará a uma base de código mais saudável, mas quando isso não for possível, a responsabilidade está nos líderes técnicos da equipe para encontrar casos em que mocks podem ser excessivamente usados, depender muito de detalhes de implementação ou acabar não testando as coisas certas.
