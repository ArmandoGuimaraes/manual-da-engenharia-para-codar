# Revisões de Código em C#

## Guia de Estilo

Os desenvolvedores devem seguir as [Convenções de Codificação C# da Microsoft](https://learn.microsoft.com/dotnet/csharp/fundamentals/coding-style/coding-conventions) e, quando aplicável, as [Diretrizes de Codificação Segura da Microsoft](https://learn.microsoft.com/dotnet/standard/security/secure-coding-guidelines).

## Análise de Código / Linting

Acreditamos firmemente que um estilo consistente aumenta a legibilidade e a manutenibilidade de uma base de código. Portanto, estamos recomendando o uso de analisadores / linters para impor regras de consistência e estilo.

### Configuração do Projeto

Recomendamos o uso de uma configuração comum para sua solução que você pode referenciar em todos os projetos que fazem parte da solução. Crie um arquivo `common.props` que contenha as configurações padrão para todos os seus projetos:

```xml
<Project>
...
    <ItemGroup>
        <PackageReference Include="Microsoft.CodeAnalysis.NetAnalyzers" Version="5.0.3">
          <PrivateAssets>all</PrivateAssets>
          <IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>
        </PackageReference>
        <PackageReference Include="StyleCop.Analyzers" Version="1.1.118">
          <PrivateAssets>all</PrivateAssets>
          <IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>
        </PackageReference>
    </ItemGroup>
    <PropertyGroup>
        <TreatWarningsAsErrors>true</TreatWarningsAsErrors>
    </PropertyGroup>
    <ItemGroup Condition="Exists('$(MSBuildThisFileDirectory)../.editorconfig')" >
        <AdditionalFiles Include="$(MSBuildThisFileDirectory)../.editorconfig" />
    </ItemGroup>
...
</Project>
```

Você pode então fazer referência ao `common.props` em seus outros arquivos de projeto para garantir uma configuração consistente.

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
  <Import Project="..\common.props" />
</Project>
```

O arquivo [.editorconfig](https://learn.microsoft.com/en-us/visualstudio/ide/editorconfig-code-style-settings-reference?view=vs-2019) permite a configuração e substituição de regras. Você pode ter um arquivo .editorconfig no nível do projeto para personalizar regras para diferentes projetos (por exemplo, projetos de teste).

[Detalhes sobre a configuração de diferentes regras](https://learn.microsoft.com/en-us/visualstudio/code-quality/use-roslyn-analyzers?view=vs-2019).

### Analisadores .NET

Os analisadores .NET da Microsoft possuem regras de qualidade de código e regras de uso de APIs .NET implementadas como analisadores usando a plataforma .NET Compiler (Roslyn). Isso é a substituição para os analisadores legados do FxCop da Microsoft.

[Habilite ou instale os analisadores .NET de primeira parte](https://learn.microsoft.com/en-us/visualstudio/code-quality/install-net-analyzers?view=vs-2019).

Se você estiver usando os analisadores legados do FxCop atualmente, [migre dos analisadores FxCop para os analisadores .NET](https://learn.microsoft.com/en-us/visualstudio/code-quality/migrate-from-fxcop-analyzers-to-net-analyzers?view=vs-2019).

### Analisador StyleCop

O analisador StyleCop é um pacote NuGet (StyleCop.Analyzers) que pode ser instalado em qualquer um de seus projetos. Ele se concentra principalmente em regras de estilo de código e garante que a equipe esteja seguindo as mesmas regras sem a necessidade de discussões subjetivas sobre chaves e espaços. Informações detalhadas podem ser encontradas aqui: [Analisadores StyleCop para a Plataforma de Compilador .NET](https://github.com/DotNetAnalyzers/StyleCopAnalyzers).

O conjunto mínimo de regras que as equipes devem adotar é o conjunto de regras [Managed Recommended Rules](https://learn.microsoft.com/en-us/visualstudio/code-quality/managed-minimum-rules-rule-set-for-managed-code?view=vs-2022).

## Formatação Automática de Código

Use o .editorconfig para configurar regras de formatação de código em seu projeto.

## Validação de Build

É importante que você apl

ique seu estilo de código e regras na integração contínua (CI) para evitar que qualquer membro da equipe faça merge de código que não esteja em conformidade com seus padrões em seu repositório Git.

Se você estiver usando analisadores do FxCop e o analisador StyleCop, é muito simples habilitá-los na CI. Você deve garantir que esteja configurando o projeto usando o NuGet e o .editorconfig ([veja a Configuração do Projeto](#configuração-do-projeto)). Uma vez que você tenha essa configuração, você precisará configurar o pipeline para construir seu código. Isso é basicamente tudo. Os analisadores do FxCop serão executados e reportarão o resultado em seu pipeline de build. Se houver regras que estejam sendo violadas, sua build ficará vermelha.

```yaml
    - task: DotNetCoreCLI@2
      displayName: 'Verificação de Estilo & Build'
      inputs:
        command: 'build'
        projects: '**/*.csproj'
```

## Habilitar Suporte ao Roslyn no Visual Studio Code

As etapas acima também funcionam no Visual Studio Code, desde que você habilite o suporte ao Roslyn para o Omnisharp. A configuração é `omnisharp.enableRoslynAnalyzers` e deve ser definida como `true`. Após habilitar essa configuração, você deve "Reiniciar o Omnisharp" (isso pode ser feito a partir da Paleta de Comandos no Visual Studio Code ou reiniciando o Visual Studio Code).

![suporte-roslyn](images/vscode-roslyn.png)

## Checklist de Revisão de Código

Além do [Checklist de Revisão de Código](../process-guidance/reviewer-guidance.md), você também deve procurar por itens específicos de revisão de código em C#:

* [ ] Este código faz uso correto de [construções de programação assíncrona](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/async/#BKMK_AsyncandAwait), incluindo o uso adequado de `await` e `Task.WhenAll` incluindo CancellationTokens?
* [ ] O código está sujeito a problemas de concorrência? Os objetos compartilhados estão devidamente protegidos?
* [ ] A injeção de dependência (DI) é usada? Está configurada corretamente?
* [ ] Os [middleware](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/middleware/index?view=aspnetcore-2.1&tabs=aspnetcore2x) incluídos neste projeto estão configurados corretamente?
* [ ] Os recursos são liberados de forma determinística usando o padrão IDispose? Todos os objetos descartáveis são devidamente liberados ([usando o padrão](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/using-statement))?
* [ ] O código está criando muitos objetos de curta duração. Poderíamos otimizar a pressão no coletor de lixo?
* [ ] O código está escrito de uma forma que causa operações de boxing?
* [ ] O código [manipula exceções corretamente](https://learn.microsoft.com/en-us/dotnet/standard/exceptions/best-practices-for-exceptions)?
* [ ] O gerenciamento de pacotes está sendo usado (NuGet) em vez de DLLs sendo commitadas?
* [ ] Este código faz uso apropriado do LINQ? Trazer o LINQ para um projeto para substituir um único loop curto ou de maneiras que não tenham bom desempenho geralmente não é apropriado.
* [ ] Este código valida corretamente a sanidade dos argumentos (ou seja, [CA1062](https://learn.microsoft.com/en-us/dotnet/fundamentals/code-analysis/quality-rules/ca1062))? Considere o uso de extensões como [Ensure.That](https://github.com/danielwertheim/Ensure.That)
* [ ] Este código inclui instrumentação de telemetria ([métricas, rastreamento](https://learn.microsoft.com/en-us/azure/azure-monitor/app/app-insights-overview) e [registro](https://serilog.net/))?
* [ ] Este código aproveita o [padrão de design de opções](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/configuration/options?view=aspnetcore-3.1) usando classes para fornecer acesso fortemente tipado a grupos de configurações relacionadas?
* [ ] Em vez de usar strings literais, são usadas constantes na classe principal? Ou se essas strings são usadas em vários arquivos/classes, existe uma classe estática para as constantes?
* [ ] Os números mágicos são explicados? Não deve haver números no código sem pelo menos um comentário explicando por que estão lá. Se o número for repetitivo, existe uma constante/enumerador ou equivalente?
* [ ] A manipulação de exceções adequada está configurada? Capturar a classe base de exceção (`catch (Exception)`) geralmente não é o padrão correto. Em vez disso, capture as exceções específicas que podem acontecer, por exemplo, `IOException`.
* [ ] O uso de #pragma é justo?
* [ ] Os testes estão organizados corretamente com o padrão **Arrange/Act/Assert** e devidamente documentados dessa forma?
* [ ] Se houver um método assíncrono, o nome do método termina com o sufixo `Async`?
* [ ] Se um método for assíncrono, é usado `Task.Delay` em vez de `Thread.Sleep`? O `Task.Delay` não bloqueia a thread atual e cria uma tarefa que será concluída sem bloquear a thread, portanto, em um ambiente com várias threads e tarefas, esta é a preferível.
* [ ] É necessário um token de cancelamento para tarefas assíncronas em vez de padrões booleanos?
* [ ] Existe um nível mínimo de registro? Os níveis de registro usados são sensatos?
* [ ] As classes internas versus privadas versus públicas e os métodos são usados da maneira certa?
* [ ] As propriedades automáticas `get` e `set` são usadas da maneira certa? Em um modelo sem construtor e para desserialização, é aceitável ter todas as propriedades acessíveis. Para outras classes, geralmente é melhor usar `set` privado ou `set` interno.
* [ ] O padrão `using` para fluxos e outras classes descartáveis é usado? Se não for, é melhor chamar explicitamente o método `Dispose`.
* [ ] As classes que mantêm coleções na memória são seguras para multithreading? Quando usadas em concorrência, use o padrão de bloqueio.
