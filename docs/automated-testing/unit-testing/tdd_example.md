# Exemplo de Desenvolvimento Orientado por Testes (TDD)

Com este método, em vez de escrever todos os seus testes de uma vez, você escreve um teste de cada vez e, em seguida, muda para escrever o código do sistema que faria esse teste passar. É importante escrever o mínimo de código necessário, mesmo que ele não seja tecnicamente "correto". Uma vez que o teste passa, você pode refatorar o código para talvez torná-lo mais sensato, mas novamente a lógica deve ser simples. À medida que você escreve mais testes, a lógica fica cada vez mais complexa, mas você pode continuar a fazer as mudanças mínimas no código do sistema com confiança, porque todo o código que foi escrito está coberto.

Como exemplo, vamos supor que estamos tentando escrever uma nova função que valida se uma string é um formato de senha válido. O formato da senha deve ser uma string maior que 8 caracteres contendo pelo menos um número. Começamos com o teste mais simples possível; uma das maneiras mais fáceis de fazer isso é escrever primeiro testes que validem as entradas na função:

```csharp
// Tests.cs
public class Tests
{
    [Fact]
    public void ValidatePassword_NullInput_Throws()
    {
        var s = new MyClass();
        Assert.Throws<ArgumentNullException>(() => s.ValidatePassword(null));
    }
}

// MyClass.cs
public class MyClass
{
    public bool ValidatePassword(string input)
    {
        return false;
    }
}
```

Se executarmos este código, o teste falhará, pois nenhuma exceção foi lançada, já que nosso código em `ValidateString` é apenas um stub. Isso está ok! Esta é a parte "Vermelha" do ciclo Vermelho-Verde-Refatorar. Agora queremos passar para a parte "Verde" - fazer a mudança mínima necessária para fazer este teste passar:

```csharp
// MyClass.cs
public class MyClass
{
    public bool ValidatePassword(string input)
    {
        throw new ArgumentNullException(nameof(input));
    }
}
```

Nossos testes passam, mas esta função realmente não funciona, ela sempre lançará a exceção. Tudo bem! À medida que continuamos a escrever testes, vamos adicionando lentamente a lógica para esta função, e ela se construirá sobre si mesma, garantindo que nossos testes continuem a passar.

Vamos pular a etapa "Refatorar" neste momento porque não há nada para refatorar. Em seguida, vamos adicionar um teste que verifica se a função retorna falso se a senha tiver menos de 8 caracteres:

```csharp
[Fact]
public void ValidatePassword_SmallSize_ReturnsFalse()
{
    var s = new MyClass();
    Assert.False(s.ValidatePassword("abc"));
}
```

Este teste passará, pois ainda só lança uma `ArgumentNullException`, mas novamente, isso é uma falha esperada. Corrigindo nossa função, ela deverá passar:

```csharp
public bool ValidatePassword(string input)
{
    if (input == null)
    {
        throw new ArgumentNullException(nameof(input));
    }

    return false;
}
```

Finalmente, algum código que parece real! Note como não foi o teste que verificou o valor nulo que nos fez adicionar a instrução `if` para a verificação de nulo, mas sim o teste subsequente que desbloqueou um novo ramo. Ao adicionar essa instrução `if`, fizemos a mudança mínima necessária para fazer **ambos** os testes passarem, mas ainda temos trabalho a fazer.

Em geral, trabalhar na ordem de adicionar um teste negativo primeiro antes de adicionar um teste positivo garantirá que ambos os casos sejam cobertos pelo código de uma forma que possa ser testada. O ciclo Vermelho-Verde-Refatorar torna esse processo super fácil, exigindo a mudança mínima - já que só queremos fazer as mudanças mínimas, simplesmente retornamos falso aqui, sabendo muito bem que estaremos adicionando lógica mais tarde que se expandirá sobre isso.

Falando nisso, vamos adicionar o teste positivo agora:

```csharp
[Fact]
public void ValidatePassword_RightSize_ReturnsTrue()
{
    var s = new MyClass();
    Assert.True(s.ValidatePassword("abcdefgh1"));
}
```

Novamente, este teste falhará no início. Uma coisa a notar aqui é que é importante tentarmos tornar nossos testes resilientes a mudanças futuras. Quando escrevemos o código sob teste, agimos de forma muito ingênua, apenas tentando fazer os testes atuais que temos passar; quando você escreve testes, você quer garantir que tudo o que está fazendo é um caso válido no futuro. Neste caso, poderíamos ter escrito a string de entrada como `abcdefgh` e, quando eventualmente escrevêssemos a função, ela passaria, mas mais tarde, quando adicionássemos testes que validassem que a função tem o resto das entradas adequadas, ela falharia incorretamente.

De qualquer forma, a próxima mudança de código é:

```csharp
public bool ValidatePassword(string input)
{
    if (input == null)
    {
        throw new ArgumentNullException(nameof(input));
    }

    if (input.Length > 8)
    {
        return true;
    }
    return false;
}
```

Aqui agora temos um teste que passa! No entanto, a lógica realmente não faz muito sentido. Fizemos a mudança mínima, que foi adicionar uma nova condição que passou para strings mais longas, mas pensando para frente, sabemos que isso não funcionará assim que adicionarmos validações adicionais. Então, vamos usar nosso primeiro passo de "Refatorar" no fluxo Vermelho-Verde-Refatorar!

```csharp
public bool ValidatePassword(string input)
{
    if (input == null)
    {
        throw new ArgumentNullException(nameof(input));
    }

    if (input.Length < 8)
    {
        return false;
    }
    return true;
}
```

Isso parece melhor. Note como, do ponto de vista funcional, inverter a instrução `if` não muda o que a função retorna. Esta é uma parte importante do fluxo de refatoração, mantendo a lógica fazendo refatorações comprovadamente seguras, geralmente através do uso de ferramentas e refatorações automatizadas do seu IDE.

Finalmente, temos um último requisito para o nosso método `ValidatePassword` e é que ele precisa verificar se há um número na senha. Vamos começar novamente com o teste negativo e validar que, com uma string com o comprimento válido, a função retorna `false` se não passarmos um número:

```csharp
[Fact]
public void ValidatePassword_ValidLength_ReturnsFalse()
{
    var s = new MyClass();
    Assert

.False(s.ValidatePassword("abcdefghij"));
}
```

Claro que o teste falha, pois ele está apenas verificando os requisitos de comprimento. Vamos corrigir o método para verificar os números:

```csharp
public bool ValidatePassword(string input)
{
    if (input == null)
    {
        throw new ArgumentNullException(nameof(input));
    }

    if (input.Length < 8)
    {
        return false;
    }

    if (!input.Any(char.IsDigit))
    {
        return false;
    }
    return true;
}
```

Aqui usamos um método LINQ útil para verificar se algum dos `char`s na `string` é um dígito, e se não for, retornamos falso. Os testes agora passam, e podemos refatorar. Para melhorar a legibilidade, por que não combinar as instruções `if`:

```csharp
public bool ValidatePassword(string input)
{
    if (input == null)
    {
        throw new ArgumentNullException(nameof(input));
    }

    if ((input.Length < 8) ||
        (!input.Any(char.IsDigit)))
    {
        return false;
    }

    return true;
}
```

Ao refatorar esse código, nos sentimos 100% confiantes nas mudanças que fizemos, pois temos 100% de cobertura de testes que testam tanto cenários positivos quanto negativos. Neste caso, já temos um método que testa o caso positivo, então nossa função está pronta!

Agora que nosso código está completamente testado, podemos fazer todo tipo de mudanças e ainda ter confiança de que ele funciona. Por exemplo, se quiséssemos mudar a implementação do método para usar regex, todos os nossos testes ainda passariam e ainda seriam válidos.

É isso aí! Terminamos de escrever nossa função, temos 100% de cobertura de testes e, se tivéssemos feito algo um pouco mais complexo, teríamos a garantia de que o que projetamos já é testável, pois os testes foram escritos primeiro!
