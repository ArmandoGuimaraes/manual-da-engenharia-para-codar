# Ferramentas de Diagnóstico

Além de [Logging](pillars/logging.md), [Tracing](pillars/tracing.md) e [Metrics](pillars/metrics.md), existem ferramentas adicionais para ajudar a diagnosticar problemas quando as aplicações não se comportam conforme o esperado. Em alguns cenários, analisar o consumo de memória e aprofundar o motivo pelo qual um processo específico leva mais tempo do que o esperado pode exigir medidas adicionais. Nestes casos, as ferramentas de diagnóstico específicas da plataforma ou da linguagem de programação entram em jogo e são úteis para depurar um vazamento de memória, perfilar o uso da CPU ou a causa de atrasos em multithreading.

## Profilers e Analisadores de Memória

Existem dois tipos de ferramentas de diagnóstico que você pode querer usar: profilers e analisadores de memória.

### Profiling

O profiling é uma técnica em que você tira pequenos instantâneos de todas as threads em uma aplicação em execução para ver o stack trace de cada thread por uma duração especificada. Essa ferramenta pode ajudá-lo a identificar onde você está gastando tempo de CPU durante a execução de sua aplicação. Existem duas técnicas principais para isso: Amostragem de CPU e Instrumentação.

A Amostragem de CPU é um método não invasivo que tira instantâneos de todas as pilhas em intervalos definidos. É a técnica mais comum para profiling e não requer nenhuma modificação em seu código.

A Instrumentação é a outra técnica em que você insere um pequeno trecho de código no início e no final de cada função que vai sinalizar de volta para o profiler sobre o tempo gasto na função, o nome da função, parâmetros e outros. Dessa forma, você modifica o código de sua aplicação em execução. Existem dois efeitos disso: seu código pode rodar um pouco mais lentamente, mas, por outro lado, você tem uma visão mais precisa de cada função e classe que foi executada até agora em sua aplicação.

#### Quando usar Amostragem vs Instrumentação?

Nem todas as linguagens de programação suportam instrumentação. A instrumentação é principalmente suportada para linguagens compiladas como .NET e Java, e algumas linguagens interpretadas em tempo de execução como Python e JavaScript. Lembre-se de que habilitar a instrumentação pode exigir a modificação de sua pipeline de compilação, ou seja, adicionando parâmetros especiais à linha de comando. Normalmente, você deve começar com a amostragem porque ela não requer a modificação de seus binários, não afeta o desempenho do processo e pode ser mais rápida para começar.

Depois de obter seus dados de profiling, existem várias maneiras de visualizar essas informações, dependendo do formato em que você as salvou. Como exemplo para .NET (dotnet-trace), existem três formatos disponíveis para salvar essas traces: Chromium, NetTrace e SpeedScope. Selecione o formato de saída dependendo da ferramenta que você vai usar. [SpeedScope](https://www.speedscope.app/) é uma aplicação web online que você pode usar para visualizar e analisar traces, e você só precisa de um navegador moderno. Tenha cuidado com ferramentas online, pois dumps/traces podem conter informações confidenciais que você não deseja compartilhar fora de sua organização.

### Analisadores de Memória

Analisadores de memória e memory dumps são outro conjunto de ferramentas de diagnóstico que você pode usar para identificar problemas em seu processo. Normalmente, esses tipos de ferramentas tiram uma "foto" da memória inteira que o processo está usando em um determinado momento e a salvam em um arquivo que pode ser analisado. Ao usar essas ferramentas, você deseja estressar seu processo o máximo possível para amplificar qualquer deficiência que você possa ter em termos de gerenciamento de memória. O memory dump deve ser feito quando o processo estiver nesse estado estressado.

Em alguns cenários, recomendamos tirar mais de um memory dump durante a reprodução de um problema. Por exemplo, se você suspeitar de um vazamento de memória e estiver executando um teste por 30 minutos, é útil tirar pelo menos 3 dumps em intervalos diferentes (ou seja, 10, 20 e 30 minutos) para compará-los entre si.

Existem várias maneiras de fazer um memory dump, dependendo do sistema operacional que você está usando. Além disso, cada sistema operacional possui seu próprio debugger que é capaz de carregar esse memory dump e explorar o estado do processo no momento em que o memory dump foi feito.

Os debuggers mais comuns são:

- Windows - [WinDbg e WinDgbNext](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/debugger-download-tools) (incluído no Windows SDK), [Visual Studio](https://visualstudio.microsoft.com/) também pode carregar um memory dump para um processo .NET Framework e .NET Core.
- Linux - [GDB é o GNU Debugger](https://www.gnu.org/software/gdb/)
- Mac OS - [LLDB Debugger](https://lldb.llvm.org/)

Existem várias ferramentas de diagnóstico específicas da plataforma do desenvolvedor que podem ser usadas:

- [Ferramentas de diagnóstico do .NET Core](https://learn.microsoft.com/en-us/dotnet/core/diagnostics/#net-core-diagnostic

-global-tools), [Repositório no GitHub](https://github.com/dotnet/diagnostics)
- [Ferramentas de diagnóstico Java - específicas da versão](https://docs.oracle.com/en/java/javase/16/troubleshoot/general-java-troubleshooting.html)
- [Depuração e perfil Python - específicas da versão](https://docs.python.org/3/library/debug.html)
- [Grupo de Trabalho de Diagnóstico Node.js](https://github.com/nodejs/diagnostics)

## Ambiente para Profiling

Para criar um perfil de aplicação o mais próximo possível da produção, é necessário considerar o ambiente em que a aplicação está planejada para rodar na produção e pode ser necessário tirar uma "foto" do estado da aplicação [sob carga](../automated-testing/performance-testing/README.md).

### Diagnóstico em Contêineres

Para aplicações monolíticas, as ferramentas de diagnóstico podem ser instaladas e executadas na VM que as hospeda. A maioria das aplicações escaláveis é desenvolvida como [microsserviços](./microservices.md) e possui interações complexas que requerem a instalação das ferramentas nos contêineres que executam o processo ou a alavancagem de um contêiner auxiliar (veja o [padrão sidecar](https://learn.microsoft.com/en-us/azure/architecture/patterns/sidecar)). Algumas plataformas expõem endpoints para interagir com a aplicação e retornar um dump.

Links úteis:

- [Diagnóstico .NET Core em contêineres](https://learn.microsoft.com/en-us/dotnet/core/diagnostics/diagnostics-in-containers)
- [Ferramenta experimental dotnet-monitor](https://devblogs.microsoft.com/dotnet/introducing-dotnet-monitor/), [O que há de novo](https://devblogs.microsoft.com/dotnet/whats-new-in-dotnet-monitor/), [Repositório no GitHub](https://github.com/dotnet/dotnet-monitor/tree/main/documentation)
- [Pontos de extremidade Spring Boot actuator](https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/#actuator.endpoints)
