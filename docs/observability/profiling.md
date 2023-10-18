# Profiling

## Visão Geral

O profiling é uma forma de análise em tempo de execução que mede vários componentes da execução, como alocação de memória, coleta de lixo, threads e travas, pilhas de chamadas ou frequência e duração de funções específicas. Ele pode ser usado para ver quais funções são as mais custosas em seu binário, permitindo que você concentre seus esforços em remover as maiores ineficiências o mais rapidamente possível. Ele pode ajudar a encontrar deadlocks, vazamentos de memória ou alocação ineficiente de memória e ajudar nas decisões relacionadas à alocação de recursos (ou seja, CPU ou RAM).

## Como Perfilar Suas Aplicações

O profiling é um tanto dependente da linguagem, então comece procurando por "perfil $linguagem" (algumas ferramentas comuns estão listadas abaixo). Além disso, o Linux Perf é uma boa alternativa, já que muitas linguagens têm ligações em C/C++.

O profiling incorre em algum custo, pois requer a inspeção da pilha de chamadas e, às vezes, a pausa completa da aplicação (ou seja, para acionar uma coleta completa de lixo em Java). É recomendável perfilar continuamente seus serviços, por exemplo, durante 10 segundos a cada 10 minutos. Considere o custo ao decidir ajustar esses parâmetros.

Diferentes ferramentas visualizam os perfis de maneira diferente. Perfis de CPU comuns podem usar um gráfico direcionado ![gráfico](images/pprof-dot.png) ou um gráfico de chamas. ![chamas](images/flame.png)

Infelizmente, cada ferramenta de profiler geralmente usa seu próprio formato para armazenar perfis e vem com sua própria visualização.

## Ferramentas Específicas

- (Java, Go, Python, Ruby, eBPF) [Pyroscope](https://github.com/pyroscope-io/pyroscope) realiza profiling contínuo prontamente.
- (Java e Go) [Flame](https://github.com/VerizonMedia/kubectl-flame) - perfis de contêineres no Kubernetes
- (Java, Python, Go) [Datadog Continuous profiler](https://www.datadoghq.com/product/code-profiling/)
- (Go) [profefe](https://github.com/profefe/profefe), que cria `pprof` para fornecer profiling contínuo
- (Java) [Eclipse Memory Analyzer](https://www.eclipse.org/mat/)
