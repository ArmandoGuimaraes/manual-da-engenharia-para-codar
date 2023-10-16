Claro, vou traduzir o conteúdo fornecido conforme suas instruções.

### Exemplo: Criando um teste de unidade

Para ilustrar algumas técnicas de teste de unidade para uma linguagem orientada a objetos, vamos começar com um exemplo de algum código para o qual desejamos adicionar testes de unidade. Neste exemplo, temos uma classe de configuração que contém todas as opções de inicialização para um aplicativo que estamos desenvolvendo. Normalmente, ela lê de um arquivo `.config`, mas estamos enfrentando três problemas com a implementação atual:

1. Há um erro na classe Configuration, e não temos testes de unidade, pois ela depende da leitura de um arquivo de configuração.
2. Não podemos testar nenhuma parte do código que depende da classe Configuration lendo um arquivo de configuração.
3. No futuro, queremos permitir que a configuração seja salva na nuvem e acessada via API REST.

O erro que estamos tentando corrigir é que, se houver várias linhas vazias no arquivo de configuração, uma exceção IndexOutOfRangeException é lançada. Nossa classe atualmente se parece com isto:

```csharp
using System.IO;
using System.Linq;

public class Configuration
{
    // Propriedades públicas de getter do objeto de configuração
    public string MyProperty { get; private set; }

    public void Initialize()
    {
        var configContents = File.ReadAllLines(".config");

        // A configuração está no formato: chave=valor
        var config = configContents.Select(l => l.Split('='))
                                   .ToDictionary(kv => kv[0], kv => kv[1]);

        // Atribuir todas as propriedades aqui
        this.MyProperty = config["myproperty"];
    }
}
```

### Abstração

Em nosso exemplo, temos uma única dependência: o sistema de arquivos. Em vez de apenas abstrair completamente o sistema de arquivos, vamos pensar sobre por que precisamos do sistema de arquivos e abstrair o *conceito* em vez da implementação. Neste caso, estamos usando a classe `File` para ler o arquivo de configuração e o conteúdo da configuração. O conceito de abstração aqui é alguma forma de leitor de configuração que retorna cada linha da configuração em uma matriz de strings. Poderíamos chamá-lo de `ConfigurationReader`, e ele tem um único método, `Read`, que retorna o conteúdo.

Ao criar abstrações, pode ser uma boa prática criar uma interface para essa abstração, em linguagens que a suportam. No exemplo com C#, podemos criar uma interface `IConfigurationReader`, e em vez de apenas termos uma classe `ConfigurationReader`, podemos ser mais específicos e nomeá-la `FileConfigurationReader` para indicar que ela lê do sistema de arquivos:

```csharp
// IConfigurationReader.cs
public interface IConfigurationReader
{
    string[] Read();
}

// FileConfigurationReader.cs
public class FileConfigurationReader : IConfigurationReader
{
    public string[] Read()
    {
        return File.ReadAllLines(".config");
    }
}
```

Agora que a dependência do arquivo foi abstraída, precisamos atualizar o método Initialize da nossa classe Configuration para usar a nova abstração em vez de chamar `File.ReadAllLines` diretamente:

```csharp
public void Initialize()
{
    var configContents = new FileConfigurationReader().Read();

    // A configuração está no formato: chave=valor
    var config = configContents.Select(l => l.Split('='))
                               .ToDictionary(kv => kv[0], kv => kv[1]);

    // Atribuir todas as propriedades aqui
    this.MyProperty = config["myproperty"];
}
```

Como você pode ver, ainda temos uma dependência no sistema de arquivos, mas essa dependência foi abstraída. Precisaremos usar outras técnicas para quebrar completamente a dependência.

### Injeção de Dependência

Na seção anterior, abstraímos o acesso ao arquivo em um `FileConfigurationReader`, mas ainda tínhamos uma dependência no sistema de arquivos em nossa função. Podemos usar a injeção de dependência para injetar o leitor certo em nossa classe `Configuration`:

```csharp
using System.IO;
using System.Linq;

public class Configuration
{
    private readonly IConfigurationReader configReader;

    // Propriedades públicas de getter do objeto de configuração
    public string MyProperty { get; private set; }

    public Configuration(IConfigurationReader reader)
    {
        this.configReader = reader;
    }

    public void Initialize()
    {
        var configContents = configReader.Read();

        // A configuração está no formato: chave=valor
        var config = configContents.Select(l => l.Split('='))
                                   .ToDictionary(kv => kv[0], kv => kv[1]);

        // Atribuir todas as propriedades aqui
        this.MyProperty = config["myproperty"];
    }
}
```

Acima, foi usada uma técnica chamada [Injeção de Construtor](https://en.wikipedia.org/wiki/Dependency_injection#Constructor_injection). Isso usa o construtor do objeto para definir quais serão nossas dependências, o que significa que qualquer objeto que cria o objeto `Configuration` controlará qual leitor precisa ser passado. Este é um exemplo de "inversão de controle", anteriormente o objeto `Configuration` controlava a dependência, mas em vez disso, empurramos o controle para qualquer componente que cria este objeto.

Note que injetamos a interface `IConfigurationReader` e não a classe concreta. Isso é o que nos permite quebrar a dependência; enquanto originalmente tínhamos uma dependência codificada na classe `File`, agora dependemos apenas de um objeto que implementa `IConfigurationReader`.

### Escrevendo nossos primeiros testes de unidade

Começamos essa aventura porque temos um erro na classe `Configuration` que não foi detectado porque não temos testes de unidade. Vamos escrever alguns testes de unidade que nos dão cobertura total da classe `Configuration`, incluindo um teste que testa o cenário descrito pelo erro (se houver várias linhas vazias no arquivo de configuração, uma exceção IndexOutOfRangeException está sendo lançada).

No entanto, ainda temos um problema, temos apenas uma única implementação de `IConfigurationReader`, e ela usa o sistema de arquivos, o que significa que qualquer teste de unidade que escrevermos ainda terá uma dependência no sistema de arquivos! Felizmente, como usamos a injeção de dependência, tudo o que precisamos fazer é criar uma implementação de `IConfigurationReader` que não dependa do sistema de arquivos. Poderíamos criar um mock aqui, mas em vez disso, vamos criar uma implementação concreta da interface que simplesmente retorna a matriz de strings passada

 - podemos chamá-la de `PassThroughConfigurationReader` (para mais detalhes sobre por que essa abordagem pode ser melhor do que a criação de mocks, consulte a página sobre [mocking](mocking.md))

```csharp
public class PassThroughConfigurationReader : IConfigurationReader
{
    private readonly string[] contents;

    public PassThroughConfigurationReader(string[] contents)
    {
        this.contents = contents;
    }

    public string[] Read()
    {
        return this.contents;
    }
}
```

Esta simples classe será usada em nossos testes de unidade, para que possamos criar diferentes estados sem exigir muito acesso ao arquivo. Agora que temos isso no lugar, podemos prosseguir e escrever nossos testes de unidade, começando com os testes que descrevem o comportamento atual:

```csharp
public class ConfigurationTests
{
    [Fact]
    public void Initialize_EmptyConfig_Throws()
    {
        var reader = new PassThroughConfigurationReader(Array.Empty<string>());
        var config = new Configuration(reader);

        Assert.Throws<KeyNotFoundException>(() => config.Initialize());
    }

    [Fact]
    public void Initialize_CorrectFormat_SetsProperty()
    {
        var reader = new PassThroughConfigurationReader(new[] {
            "myproperty=myvalue"
        });
        var config = new Configuration(reader);

        config.Initialize();

        Assert.Equal("myvalue", config.MyProperty);
    }
}
```

### Corrigindo o erro

Todos os nossos testes atuais passam e nos dão 100% de cobertura, no entanto, como evidenciado pelo erro, devemos não estar cobrindo todas as entradas e saídas possíveis. No caso do erro, várias linhas vazias causariam um problema. Além disso, `KeyNotFoundException` não é uma exceção muito amigável e é um detalhe de implementação, não algo que faz sentido ao projetar a API de Configuração. Vamos adicionar mais alguns testes e alinhar os testes com o que pensamos que a classe `Configuration` deve se comportar:

```csharp
public class ConfigurationTests
{
    [Fact]
    public void Initialize_EmptyConfig_Throws()
    {
        var reader = new PassThroughConfigurationReader(Array.Empty<string>());
        var config = new Configuration(reader);

        Assert.Throws<InvalidOperationException>(() => config.Initialize());
    }

    [Fact]
    public void Initialize_MalformedLine_Throws()
    {
        var reader = new PassThroughConfigurationReader(new[] {
            "myproperty",
        });
        var config = new Configuration(reader);

        Assert.Throws<InvalidOperationException>(() => config.Initialize());
    }

    [Fact]
    public void Initialize_MultipleEqualSigns_PropertyContainsNoEquals()
    {
        var reader = new PassThroughConfigurationReader(new[] {
            "myproperty=myval1=myval2",
        });
        var config = new Configuration(reader);

        config.Initialize();

        Assert.Equal("myval1=myval2", config.MyProperty);
    }

    [Fact]
    public void Initialize_WithBlankLines_Ignores()
    {
        var reader = new PassThroughConfigurationReader(new[] {
            "myproperty=myvalue",
            string.Empty,
        });
        var config = new Configuration(reader);

        config.Initialize();

        Assert.Equal("myvalue", config.MyProperty);
    }

    [Fact]
    public void Initialize_CorrectFormat_SetsProperty()
    {
        var reader = new PassThroughConfigurationReader(new[] {
            "myproperty=myvalue"
        });
        var config = new Configuration(reader);

        config.Initialize();

        Assert.Equal("myvalue", config.MyProperty);
    }
}
```

Agora temos 4 testes falhando e 1 teste passando, mas estabelecemos firmemente através do uso desses testes como esperamos que os chamadores usem a classe Configuration e o que é e não é permitido como entradas. Agora só precisamos corrigir a classe `Configuration` para que nossos testes passem:

```csharp
public void Initialize()
{
    var configContents = configReader.Read();

    if (configContents.Length == 0)
    {
        throw new InvalidOperationException("Configuração vazia");
    }

    // A configuração está no formato: chave=valor
    var config = configContents.Where(l => !string.IsNullOrWhiteSpace(l))
                               .Select(l =>
                               {
                                   var splitLine = l.Split('=', 2);
                                   if (splitLine.Length < 2)
                                   {
                                       throw new InvalidOperationException("Linha malformada");
                                   }
                                   return splitLine;
                               })
                               .ToDictionary(kv => kv[0], kv => kv[1]);

    // Atribuir todas as propriedades aqui
    this.MyProperty = config["myproperty"];
}
```

Agora todos os nossos testes passam! Corrigimos nosso erro, adicionamos testes de unidade à classe `Configuration` e temos muito mais confiança em mudanças futuras.

### Código Não Testável

Como descrito na [seção de abstração](README.md#abstraction), nem todo código pode ser devidamente testado por unidade. Em nosso caso, temos uma única classe que tem 0% de cobertura de teste: `FileConfigurationReader`. Isso é esperado; neste caso, mantivemos o `FileConfigurationReader` o mais leve possível, sem lógica adicional além de chamar a dependência de terceiros. `FileConfigurationReader` é um exemplo do [padrão de design de fachada](https://en.wikipedia.org/wiki/Facade_pattern).

### Design Testável e Melhorias Futuras

Um dos nossos problemas originais descritos neste exemplo é que, no futuro, esperamos carregar a configuração a partir de uma API web. Ao fazer todo o trabalho de abstrair a forma como carregamos o texto de configuração e quebrar a dependência no sistema de arquivos, já fizemos todo o trabalho árduo para habilitar esse cenário futuro! Tudo o que precisa ser feito a seguir é criar uma implementação `WebApiConfigurationReader` e usá-la para construir o objeto `Configuration`, e ele deve funcionar.

Esse é um dos benefícios do design testável, no processo de escrever nossos testes de forma segura, um efeito colateral disso é que já temos nossas dependências que podem mudar abstraídas e exigirão mudanças mínimas para implementar.

Outro benefício adicional é que temos várias possibilidades abertas por esse design testável. Por exemplo, agora podemos ter uma configuração em cascata usando todas as 3 implementações `IConfigurationReader`, incluindo a que escrevemos apenas para nossos testes! Podemos primeiro verificar se o acesso à Internet está disponível e, se estiver, usar `WebApiConfigurationReader`. Se não houver internet disponível, podemos recorrer ao arquivo de configuração local no sistema atual usando `FileConfigurationReader`. Se por algum motivo o arquivo de configuração não existir, podemos usar o `PassThrough

ConfigurationReader` para fornecer um conjunto de configurações padrão.

### Conclusão

Espero que este exemplo tenha demonstrado como o design testável pode ser benéfico para o desenvolvimento de software. Não apenas nos dá a capacidade de escrever testes de unidade, mas também nos dá a capacidade de escrever código mais modular e flexível.
