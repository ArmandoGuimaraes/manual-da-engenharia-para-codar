# Teste de Unidade

O teste de unidade é uma ferramenta fundamental no conjunto de ferramentas de todo desenvolvedor. Testes de unidade não apenas nos ajudam a testar nosso código, mas também incentivam boas práticas de design, reduzem as chances de bugs chegarem à produção e podem até servir como exemplos ou documentação sobre como o código funciona. Testes de unidade bem escritos também podem melhorar a eficiência do desenvolvedor.

O teste de unidade também é uma das formas mais comumente mal compreendidas de teste. Teste de unidade se refere a um tipo muito específico de teste; um teste de unidade deve ser:

- **Comprovadamente confiável** - deve ser 100% confiável para que falhas indiquem um bug no código
- **Rápido** - deve ser executado em milissegundos, uma suíte completa de teste de unidade não deve demorar mais do que alguns segundos
- **Isolado** - remover todas as dependências externas garante confiabilidade e velocidade

## Por que Teste de Unidade

Não é segredo que escrever testes de unidade é difícil e ainda mais difícil escrevê-los bem. Escrever testes de unidade também aumenta o tempo de desenvolvimento para cada funcionalidade. Então, por que devemos escrevê-los?

Testes de unidade:

- reduzem custos ao detectar bugs mais cedo e evitar regressões
- aumentam a confiança do desenvolvedor nas mudanças
- aceleram o ciclo interno do desenvolvedor
- atuam como documentação como código

Para mais detalhes, veja todas as [descrições detalhadas dos pontos acima](./why-unit-tests.md).

## Blocos de Design de Teste de Unidade

O teste de unidade é o nível mais baixo de teste e, como tal, geralmente tem poucos componentes e dependências.

O **sistema sob teste** (abreviado como SUT) é a "unidade" que estamos testando. Geralmente, são métodos ou funções, mas dependendo da linguagem, esses podem ser diferentes. Em geral, você quer que a unidade seja o menor possível.

A maioria das linguagens também possui uma ampla suíte de **frameworks de teste de unidade** e executores de teste. Esses frameworks de teste têm uma ampla gama de funcionalidades, mas a funcionalidade básica deve ser uma forma de organizar seus testes e executá-los rapidamente.

Finalmente, há o seu **código de teste de unidade**; o código de teste de unidade geralmente é curto e simples, preferindo repetição a adicionar camadas e complexidade ao código.

## Aplicando o Teste de Unidade

Começar a escrever um teste de unidade é muito mais fácil do que alguns outros tipos de teste, já que deve exigir quase nenhuma configuração e é apenas código. Cada [framework de teste](#test-frameworks) é diferente na forma como você organiza e escreve seus testes, mas as técnicas gerais e melhores práticas de escrita de um teste de unidade são universais.

### Técnicas

Essas são algumas técnicas comumente usadas que ajudarão na autoria de testes de unidade. Para alguns exemplos, veja as páginas sobre o uso de [abstração e injeção de dependência para criar um teste de unidade](authoring_example.md) ou como fazer [desenvolvimento orientado por testes](tdd_example.md).

Observe que algumas dessas técnicas são mais específicas para linguagens fortemente tipadas e orientadas a objetos. Linguagens funcionais e linguagens de script têm técnicas semelhantes que podem parecer diferentes, mas esses termos são comumente usados em todos os exemplos de teste de unidade.

#### Abstração

A abstração é quando pegamos um detalhe de implementação exato e o generalizamos em um conceito. Essa técnica pode ser usada na criação de design testável e é usada com frequência, especialmente em linguagens orientadas a objetos. Para testes de unidade, a abstração é comumente usada para quebrar uma dependência rígida e substituí-la por uma abstração. Essa abstração permite então maior flexibilidade no código e permite que um [mock ou simulador](mocking.md) seja usado em seu lugar.

Um dos efeitos colaterais da abstração de dependências é que você pode ter uma abstração que não tem cobertura de teste. Este é um caso em que o teste de unidade não é bem adequado, você não pode esperar testar tudo em unidade, coisas como dependências sempre serão um caso não coberto. É por isso que mesmo se você tiver uma suíte robusta de teste de unidade, [teste de integração ou teste funcional](../integration-testing/README.md) ainda deve ser usado - sem isso, uma mudança na forma como a dependência funciona nunca seria detectada.

Ao criar wrappers em torno de dependências de terceiros, é melhor manter as implementações com o mínimo de lógica possível, usando uma [fachada](https://en.wikipedia.org/wiki/Facade_pattern) muito simples que chama a dependência.

Um exemplo de uso de abstração pode ser encontrado [aqui](authoring_example.md#abstraction).

#### Injeção de Dependência

[A injeção de dependência](https://en.wikipedia.org/wiki/Dependency_Injection) é uma técnica que nos permite extrair dependências do nosso código. Em um caso de uso normal de uma classe dependente, a dependência é construída e usada dentro do sistema sob teste. Isso cria uma dependência rígida entre as duas classes, o que pode torná-la particularmente difícil de testar isoladamente. Dependências podem ser coisas como classes que envolvem uma API REST ou até mesmo algo tão simples quanto o acesso a arquivos. Ao injetar as dependências em nosso sistema em vez de construí-las, "invertemos o controle" da dependência. Você pode ver "Inversão de Controle" e "Injeção de Dependência" usados como termos separados, mas é muito difícil ter um e não o outro, com alguns argumentando que [Injeção de Dependência é uma forma mais específica de dizer inversão de controle](https://martinfowler.com/articles/injection.html#InversionOfControl). Em certas linguagens como C#, não usar injeção de dependência pode levar a um código que não é testável em unidade, já que não há como injetar objetos simulados. Manter a testabilidade em mente desde o início e avaliar o uso da injeção de dependência

 pode poupar você de um refatoramento demorado mais tarde.

Uma das [desvantagens da injeção de dependência](https://en.wikipedia.org/wiki/Dependency_Injection#Disadvantages) é que ela pode facilmente sair do controle. Embora não haja mais dependências rígidas, ainda há acoplamento entre as interfaces, e passar todas as implementações de interface para todas as classes apresenta tantas desvantagens quanto não usar Injeção de Dependência. Ser intencional com quais dependências são injetadas em quais classes é a chave para desenvolver um sistema sustentável.

Muitas linguagens incluem frameworks especiais de Injeção de Dependência que cuidam do código de inicialização e construção dos objetos. Exemplos disso são [Spring](https://spring.io/) em Java ou embutido em [ASP.NET Core](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/dependency-injection?view=aspnetcore-3.1).

Um exemplo de uso de injeção de dependência pode ser encontrado [aqui](authoring_example.md#dependency-injection).

#### Desenvolvimento Orientado por Testes

O Desenvolvimento Orientado por Testes (TDD) é menos uma técnica em como seu código é projetado, mas uma técnica para escrever seu código que o levará a um design testável desde o início. A premissa básica do desenvolvimento orientado por testes é que você escreve seu código de teste primeiro e depois escreve o sistema sob teste para corresponder ao teste que você acabou de escrever. Dessa forma, todo o design do teste é feito antecipadamente e, quando você termina de escrever seu código do sistema, já está com 100% de taxa de aprovação e cobertura de teste. Também garante que o design testável seja incorporado ao sistema, já que o teste foi escrito primeiro!

Para mais informações sobre TDD e um exemplo, veja a página sobre [Desenvolvimento Orientado por Testes](./tdd_example.md).

### Melhores Práticas

#### Organizar/Agir/Afirmar

Uma forma comum de organizar seu código de teste de unidade é chamada de Organizar/Agir/Afirmar. Isso divide seu teste de unidade em 3 seções diferentes e discretas:

1. Organizar - Configure todas as variáveis, mocks, interfaces e estados de que você precisará para executar o teste
2. Agir - Execute o sistema sob teste, passando qualquer um dos objetos acima que foram criados
3. Afirmar - Verifique que, com o estado dado, o sistema agiu adequadamente.

Usar esse padrão para escrever testes torna-os muito legíveis e também familiares para futuros desenvolvedores que precisariam ler seus testes de unidade.

##### Exemplo

Vamos supor que temos uma classe `MeuObjeto` com um método `TentarAlgo` que interage com uma matriz de strings, mas se a matriz não tiver elementos, ele retornará falso. Queremos escrever um teste que verifica o caso em que a matriz não tem elementos:

```csharp
[Fact]
public void TentarAlgo_SemElementos_RetornaFalso()
{
    // Organizar
    var elementos = Array.Empty<string>();
    var meuObjeto = new MeuObjeto();

    // Agir
    var meuRetorno = meuObjeto.TentarAlgo(elementos);

    // Afirmar
    Assert.False(meuRetorno);
}
```

#### Mantenha os testes pequenos e teste apenas uma coisa

Os testes de unidade devem ser curtos e testar apenas uma coisa. Isso facilita o diagnóstico quando houve uma falha sem precisar de algo como o número da linha em que o teste falhou. Ao usar [Organizar/Agir/Afirmar](#organizaragirafirmar), pense nisso como testar apenas uma coisa na fase "Agir".

Há algum desacordo sobre se testar uma coisa significa "afirmar uma coisa" ou "testar um estado, com várias afirmações, se necessário". Ambos têm suas vantagens e desvantagens, mas como na maioria dos desacordos técnicos, não há uma "resposta certa". A consistência ao escrever seus testes de uma forma ou de outra é mais importante!

#### Usando uma convenção de nomenclatura padrão para todos os testes de unidade

Sem ter uma convenção padrão estabelecida para os nomes dos testes de unidade, os nomes dos testes de unidade acabam sendo ou não descritivos o suficiente ou duplicados em várias classes de teste diferentes. Estabelecer um padrão não é apenas importante para manter seu código consistente, mas um bom padrão também melhora a legibilidade e a capacidade de depuração de um teste. Neste artigo, a convenção usada para todos os testes de unidade foi `NomeDaUnidade_EstadoSobTeste_ResultadoEsperado`, mas há muitas outras convenções possíveis também, o importante é ser consistente e descritivo. Ter nomes descritivos como o acima torna trivial encontrar o teste quando há uma falha e também já explica qual era a expectativa do teste e qual estado fez com que ele falhasse. Isso pode ser especialmente útil ao olhar para falhas em um sistema de CI/CD onde tudo o que você sabe é o nome do teste que falhou - em vez disso, agora você sabe o nome do teste e exatamente por que ele falhou (especialmente acoplado com um framework de teste que registra saídas úteis em falhas).

### Coisas a Evitar

Alguns problemas comuns ao escrever um teste de unidade que são importantes de evitar:

- Sleeps - Um sleep pode ser um indicador de que talvez algo esteja fazendo uma solicitação a uma dependência que não deveria. Em geral, se seu código é instável sem o sleep, considere por que ele está falhando e se você pode remover a instabilidade introduzindo uma forma mais confiável de comunicar possíveis mudanças de estado. Adicionar sleeps aos seus testes de unidade também quebra um dos nossos princípios originais de teste de unidade: os testes devem ser rápidos, na ordem de milissegundos. Se os testes estão demorando na ordem de segundos, eles se tornam mais difíceis de executar.
- Leitura do disco - Pode ser realmente tentador colocar o valor esperado de um retorno de função em um arquivo e ler esse arquivo para comparar os resultados. Isso cria uma dependência com o sistema de arquivos e quebra nosso princípio de manter nossos testes de unidade isolados e 100% confiáveis

. Qualquer dependência externa, como acesso ao sistema de arquivos, pode potencialmente causar falhas intermitentes. Além disso, isso pode ser um sinal de que talvez o teste ou a unidade sob teste seja muito complexa e deva ser simplificada.
- Chamadas de APIs de terceiros - Quando você não controla uma biblioteca de terceiros que está chamando, é impossível saber com certeza o que ela está fazendo, e é melhor abstrai-la. Caso contrário, você pode estar fazendo chamadas REST ou outras áreas potenciais de falha sem escrever diretamente o código para isso. Isso também é geralmente um sinal de que o design do sistema não é totalmente testável. É melhor envolver chamadas de API de terceiros em interfaces ou outras estruturas para que elas não sejam invocadas em testes de unidade. Para mais informações, consulte a página sobre [mocking](mocking.md).

## Frameworks e Ferramentas de Teste de Unidade

### Frameworks de Teste

Os frameworks de teste de unidade estão em constante mudança. Para uma lista completa de todos os frameworks de teste de unidade [veja a página na Wikipedia](https://en.wikipedia.org/wiki/List_of_unit_testing_frameworks). Os frameworks têm muitos recursos e devem ser escolhidos com base em qual conjunto de recursos se encaixa melhor para o projeto em particular.

### Frameworks de Mock

Muitos projetos começam com um framework de teste de unidade e também adicionam um framework de mock. Embora os frameworks de mock tenham seus usos e às vezes possam ser um requisito, não deve ser algo que é adicionado sem considerar as implicações e riscos mais amplos associados ao uso pesado de mocks.

Para ver se o uso de mocks é adequado para o seu projeto, ou se uma abordagem sem mocks é mais apropriada, consulte a página sobre [mocking](mocking.md).

### Ferramentas

Essas ferramentas permitem a execução constante de seus testes de unidade com cobertura de código em linha, tornando o ciclo interno de desenvolvimento extremamente rápido e permitindo um fácil TDD:

- [Visual Studio Live Unit Testing](https://learn.microsoft.com/en-us/visualstudio/test/live-unit-testing-intro?view=vs-2019)
- [Wallaby.js](https://wallabyjs.com/)
- [Infinitest](http://infinitest.github.io/) para Java
- [PyCrunch](https://plugins.jetbrains.com/plugin/13264-pycrunch--live-testing) para Python

## Coisas a Considerar

### Transferindo a responsabilidade para testes de integração

Em algumas situações, vale a pena considerar incluir os testes de integração no ciclo interno de desenvolvimento para fornecer uma cobertura de código suficiente para garantir que o sistema está funcionando corretamente. O pré-requisito para que essa abordagem seja bem-sucedida é ter testes de integração capazes de serem executados a uma velocidade comparável à dos testes de unidade, tanto localmente quanto em um ambiente de CI. Frameworks de aplicação modernos como .NET ou Spring Boot, combinados com a abordagem de mocking ou stubbing correta para dependências externas, oferecem excelentes capacidades para habilitar tais cenários para testes.

Normalmente, os testes de integração apenas provam que os módulos desenvolvidos independentemente se conectam conforme projetado. A cobertura de teste dos testes de integração pode ser estendida para verificar o comportamento correto do sistema também. A responsabilidade de fornecer uma cobertura de código de ramo e linha suficiente pode ser transferida dos testes de unidade para os testes de integração.
Em vez de vários testes de unidade necessários para testar um caso específico de funcionalidade do sistema, um cenário de integração é criado que cobre todo o fluxo. Por exemplo, no caso de uma API, as respostas HTTP recebidas e seu conteúdo são verificados para cada solicitação no teste. Isso cobre tanto a integração entre os componentes da API quanto a correção de sua lógica de negócios.

Com essa abordagem, testes de integração eficientes podem ser tratados como uma extensão do teste de unidade, assumindo a responsabilidade de validar cenários de caminho feliz/falha. Ele tem a vantagem de testar o sistema como uma caixa preta, sem qualquer conhecimento de seus componentes internos. A refatoração de código não tem impacto nos testes. Técnicas comuns de teste como TDD podem ser aplicadas em um nível mais alto, resultando em um processo de desenvolvimento orientado por testes de aceitação. Dependendo das especificidades do projeto, os testes de unidade ainda desempenham um papel importante. Eles podem ser usados para ajudar a ditar um design testável em um nível mais baixo ou para testar lógica de negócios complexa e casos extremos, se necessário.

## Conclusão

O teste de unidade é extremamente importante, mas também não é a bala de prata; ter testes de unidade adequados é apenas uma parte de um sistema bem testado. No entanto, escrever testes de unidade adequados ajudará no design do seu sistema, bem como ajudará a capturar regressões, bugs e aumentar a velocidade do desenvolvedor.

## Recursos

- [Melhores Práticas de Teste de Unidade](https://learn.microsoft.com/en-us/dotnet/core/testing/unit-testing-best-practices)
