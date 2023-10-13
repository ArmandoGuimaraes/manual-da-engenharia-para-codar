# Teste de Desempenho

O Teste de Desempenho é um termo sobrecarregado que é usado para se referir a várias subcategorias de testes relacionados ao desempenho, cada uma com um propósito diferente.

Uma boa descrição do teste de desempenho geral é a seguinte:

> O teste de desempenho é um tipo de teste destinado a determinar a capacidade de resposta, taxa de transferência, confiabilidade e/ou escalabilidade de um sistema sob uma determinada carga de trabalho. [Orientação para Teste de Desempenho para Aplicações Web](https://learn.microsoft.com/en-us/archive/blogs/dajung/ebook-pnp-performance-testing-guidance-for-web-applications).

Antes de entrar nas diferentes subcategorias de testes de desempenho, vamos entender por que o teste de desempenho é normalmente realizado.

## Por que Teste de Desempenho

O teste de desempenho é comumente realizado para realizar uma ou mais das seguintes ações:

- **Ajustar o desempenho do sistema**

  - Identificar gargalos e problemas com o sistema em diferentes níveis de carga.

  - Comparar as características de desempenho do sistema para diferentes configurações de sistema.

  - Elaborar uma estratégia de escalabilidade para o sistema.

- Auxiliar no **planejamento de capacidade**

  - O planejamento de capacidade é o processo de determinar que tipo de recursos de hardware e software são necessários para executar uma aplicação para atender a metas de desempenho predefinidas.

  - O planejamento de capacidade envolve a identificação das expectativas de negócios, as flutuações periódicas do uso da aplicação, considerando o custo de execução da infraestrutura de hardware e software.

- Avaliar a **prontidão do sistema** para o lançamento:

  - Avaliar as características de desempenho do sistema (tempo de resposta, taxa de transferência) em um ambiente semelhante ao de produção.
O objetivo é garantir que as metas de desempenho possam ser alcançadas após o lançamento.

- Avaliar o **impacto de desempenho das mudanças na aplicação**

  - Comparar as características de desempenho de uma aplicação após uma mudança com os valores das características de desempenho durante execuções anteriores (ou valores de referência), pode fornecer uma indicação de problemas de desempenho (regressão de desempenho) ou melhorias introduzidas devido a uma mudança.

## Principais Categorias de Teste de Desempenho

O teste de desempenho é um tópico amplo. Há muitas áreas onde você pode realizar testes. De forma geral, você pode realizar testes no backend e na interface do usuário. Você pode testar o desempenho de componentes individuais, bem como testar a funcionalidade de ponta a ponta.

Existem várias categorias de testes também:

### [Teste de Carga](./load-testing.md)

Esta é a subcategoria de teste de desempenho que se concentra em validar as características de desempenho de um sistema, quando o sistema enfrenta os volumes de carga que são esperados durante a operação de produção. Um **Teste de Resistência** ou um **Teste de Soak** é um teste de carga realizado durante um longo período, variando de várias horas a dias.

### Teste de Estresse

Esta é a subcategoria de teste de desempenho que se concentra em validar as características de desempenho de um sistema quando o sistema enfrenta carga extrema. O objetivo é avaliar como o sistema lida com a pressão até seus limites, ele se recupera (ou seja, escala) ou simplesmente quebra e falha?

### Teste de Resistência

O objetivo do teste de resistência é garantir que o sistema possa manter um bom desempenho sob períodos prolongados de carga.

### Teste de Pico

O objetivo do teste de pico é validar que um sistema de software pode responder bem a grandes e repentinos picos.

### Teste de Caos

O teste de caos ou engenharia do caos é a prática de experimentar em um sistema para construir confiança de que o sistema pode resistir a condições turbulentas na produção. Seu objetivo é identificar fraquezas antes que elas se manifestem em todo o sistema. Desenvolvedores frequentemente implementam procedimentos de contingência para falhas de serviço. O teste de caos desliga arbitrariamente diferentes partes do sistema para validar que os procedimentos de contingência funcionam corretamente.

## Métricas de Monitoramento de Desempenho

Ao executar os vários tipos de abordagens de teste, seja estresse, resistência, pico ou teste de caos, é importante capturar várias métricas para ver como o sistema se comporta.

No nível básico de hardware, há quatro áreas a serem consideradas.

- Disco físico
- Memória
- Processador
- Rede

Essas quatro áreas estão inextricavelmente ligadas, o que significa que um desempenho ruim em uma área levará a um desempenho ruim em outra área. Engenheiros preocupados em entender o desempenho da aplicação devem se concentrar nessas quatro áreas principais.

O exemplo clássico de como o desempenho em uma área pode afetar o desempenho em outra é a pressão da memória.

Se a memória disponível de uma aplicação estiver baixa, o sistema operacional tentará compensar as deficiências de memória transferindo páginas de dados da memória para o disco, liberando assim memória. Mas esse trabalho requer ajuda da CPU e do disco físico.

Isso significa que, quando você observa o desempenho quando há baixas quantidades de memória, também notará picos na atividade do disco e da CPU.

## Disco Físico

Quase todos os sistemas de software dependem do desempenho do disco físico. Isso é especialmente verdadeiro para o desempenho de bancos de dados. Abordagens mais modernas para o uso de SSDs para armazenamento de disco físico podem melhorar dramaticamente o desempenho das aplicações. Aqui estão algumas das métricas que você pode capturar e analisar:

| Contador | Descrição |
|:---------|:----------|
| Média do Comprimento da Fila do Disco | Este valor é derivado usando os contadores (Transferências de Disco/seg)*(Seg de Disco/Transferência). Esta métrica descreve a fila do disco ao longo do tempo, suavizando quaisquer picos rápidos. Ter qualquer disco físico com um comprimento médio de fila acima de 2 por períodos prolongados pode ser uma indicação de que seu disco é um gargalo. |
| % de Tempo Ocioso | Esta é uma medida da porcentagem de tempo que o disco estava ocioso. Ou seja, não há solicitações de disco pendentes do sistema operacional esperando para serem concluídas. Um número baixo aqui é um sinal positivo de que o disco tem capacidade excedente para atender ou escrever solicitações do sistema operacional. |
| Média de Seg de Disco/Leitura e Média de Seg de Disco/Escrita | Ambos medem a latência dos seus discos. A latência é definida como o tempo médio que leva para uma transferência de disco ser concluída. Você obviamente quer números tão baixos quanto possível, mas precisa ter cuidado para levar em conta as diferenças de velocidade inerentes entre SSDs e discos rígidos tradicionais. Para este contador, é importante definir uma linha de base após a instalação do hardware. Em seguida, use esse valor daqui para frente para determinar se você está enfrentando problemas de latência relacionados ao hardware. |
| Leituras de Disco/seg e Escritas de Disco/seg | Estes contadores medem o número total de solicitações de E/S concluídas por segundo. Semelhante aos contadores de latência, valores bons e ruins para esses contadores dependem do seu hardware de disco, mas valores mais altos do que sua linha de base inicial normalmente não apontam para um problema de hardware neste caso. Este contador pode ser útil para identificar picos na E/S de disco. |

## Processador

É importante entender a quantidade de tempo gasto no modo kernel ou privilegiado. Em geral, se o código estiver gastando muito tempo executando chamadas do sistema operacional, isso poderá ser uma área de preocupação, pois não permitirá que você execute suas aplicações em modo de usuário, como seus bancos de dados, servidores/serviços web, etc.

A diretriz é que a CPU deve gastar apenas cerca de 20% do tempo total do processador executando no modo kernel.

| Contador | Descrição |
|:---------|:----------|
| % de Tempo do Processador | Esta é a porcentagem do tempo total decorrido que o processador estava ocupado executando. Este contador pode ser muito alto ou muito baixo. Se o tempo do seu processador estiver consistentemente abaixo de 40%, então há uma questão sobre se você superprovisionou sua CPU. 70% é geralmente considerado um bom número alvo e, se você começar a ir acima de 70%, pode querer explorar por que há alta pressão da CPU. |
| % de Tempo Privilegiado (Modo Kernel) | Este mede a porcentagem do tempo decorrido que o processador gastou executando no modo kernel. Como este contador leva em conta apenas operações de kernel, uma alta porcentagem de tempo privilegiado (maior que 25%) pode indicar um problema de driver ou hardware que deve ser investigado. |
| % de Tempo de Usuário | A porcentagem do tempo decorrido que o processador gastou executando no modo de usuário (seu código de aplicação). Uma boa diretriz é estar consistentemente abaixo de 65%, pois você quer ter alguma folga tanto para as operações de kernel mencionadas acima quanto para quaisquer outros picos de CPU exigidos por outras aplicações. |
| Comprimento da Fila | Este é o número de threads que estão prontas para executar, mas esperando que um núcleo fique disponível. Em máquinas de núcleo único, um valor sustentado maior que 2-3 pode significar que você tem alguma pressão da CPU. Da mesma forma, para uma máquina multicore, divida o comprimento da fila pelo número de núcleos e, se isso for continuamente maior que 2-3, pode haver pressão da CPU. |

## Adaptador de Rede

A velocidade da rede é frequentemente um culpado oculto de desempenho ruim. Encontrar a causa raiz do mau desempenho da rede é frequentemente difícil. A fonte de problemas pode se originar de consumidores de largura de banda, como videoconferência, dados de transação, backups de rede, vídeos recreativos.

Na verdade, as três razões mais comuns para uma desaceleração da rede são:

- Congestionamento
- Corrupção de dados
- Colisões

Algumas das ferramentas que podem ajudar incluem:

- ifconfig
- netstat
- iperf
- tcpretrans
- tcpdump
- WireShark

A solução de problemas de desempenho da rede geralmente começa com a verificação do hardware. Coisas típicas a serem exploradas são se há algum fio solto ou verificando que todos os roteadores estão ligados. Nem sempre é possível fazer isso, mas às vezes um simples caso de reciclagem de energia do modem ou roteador pode resolver muitos problemas.

Especialistas em rede frequentemente realizam a seguinte sequência de etapas para solucionar problemas:

- Verificar o hardware
- Usar IP config
- Usar ping e tracert
- Realizar verificação de DNS

Abordagens mais avançadas frequentemente envolvem olhar para alguns dos contadores de desempenho de rede, conforme explicado abaixo.

### Contadores de Rede

A tabela acima oferece alguns pontos de referência para entender melhor o que você pode esperar da sua rede. Aqui estão alguns contadores que podem ajudá-lo a entender onde os gargalos podem existir:

| Contador | Descrição |
|:---------|:----------|
| Bytes Recebidos/seg | A taxa na qual bytes são recebidos em cada adaptador de rede. |
| Bytes Enviados/seg | A taxa na qual bytes são enviados em cada adaptador de rede. |
| Bytes Total/seg | O número de bytes enviados e recebidos pela rede. |
| Segmentos Recebidos/seg | A taxa na qual segmentos são recebidos para o protocolo. |
| Segmentos Enviados/seg | A taxa na qual segmentos são enviados. |
| % de Tempo de Interrupção | A porcentagem de tempo que o processador gasta recebendo e atendendo interrupções de hardware. Este valor é um indicador indireto da atividade de dispositivos que geram interrupções, como adaptadores de rede. |

> Há uma distinção importante entre **latência** e **vazão**. **Latência** mede o tempo que leva para um pacote ser transferido pela rede, seja em termos de uma transmissão unidirecional ou de ida e volta. **Vazão** é diferente e tenta medir a quantidade de dados sendo enviados e recebidos dentro de uma unidade de tempo.

## Memória

| Contador | Descrição |
|:---------|:----------|
| MBs Disponíveis | Este contador representa a quantidade de memória que está disponível para aplicações em execução. Memória baixa pode acionar Falhas de Página, onde pressão adicional é colocada na CPU para trocar memória para dentro e fora do disco. Se a quantidade de memória disponível cair abaixo de 10%, mais memória deve ser obtida. |
| Páginas/seg | Este é na verdade a soma dos contadores "Páginas Entradas/seg" e "Páginas Saídas/seg", que é a taxa na qual as páginas estão sendo lidas e escritas como resultado de falhas de página. Pequenos picos com este valor não significam que há um problema, mas valores sustentados acima de 50 podem significar que a memória do sistema é um gargalo. |
| Arquivo de Paginação(_Total)\% de Uso | A porcentagem do arquivo de paginação do sistema que está atualmente em uso. Isso não está diretamente relacionado ao desempenho, mas você pode enfrentar sérios problemas de aplicação se o arquivo de paginação ficar completamente cheio e memória adicional ainda estiver sendo solicitada por aplicações. |

## Atividades-chave de Teste de Desempenho

As atividades de teste de desempenho variam dependendo da subcategoria de teste de desempenho e dos requisitos e restrições do sistema. Para orientação específica, você pode seguir o link para a subcategoria de testes de desempenho listada acima.
As seguintes atividades podem ser incluídas dependendo da subcategoria de teste de desempenho:

### Identificar os Critérios de Aceitação para os Testes

Isso geralmente incluirá identificar os objetivos e restrições para as características de desempenho do sistema.

### Planejar e Projetar os Testes

Em geral, precisamos considerar os seguintes pontos:

- Definir a carga com a qual a aplicação deve ser testada
- Estabelecer as métricas a serem coletadas
- Estabelecer quais ferramentas serão usadas para os testes
- Estabelecer a frequência do teste de desempenho: os testes de desempenho serão feitos como parte dos sprints de desenvolvimento de recursos ou apenas antes do lançamento para um ambiente importante?

### Implementação

- Implementar os testes de desempenho de acordo com a abordagem projetada.
- Instrumentar o sistema e garantir que ele esteja emitindo as métricas de desempenho necessárias.

### Execução do Teste

- Executar os testes e coletar métricas de desempenho.

### Análise de Resultados e Re-teste

- Analisar os resultados/métricas de desempenho dos testes.
- Identificar as mudanças necessárias para ajustar o sistema (ou seja, código, infraestrutura) para melhor acomodar os objetivos do teste.
- Em seguida, teste novamente. Este ciclo continua até que o objetivo do teste seja alcançado.

O [Modelo de Teste de Desempenho Iterativo](./iterative-perf-test-template.md) pode ser usado para capturar detalhes sobre o resultado do teste para cada iteração.

## Recursos

- [Padrões e Práticas: Orientação para Teste de Desempenho para Aplicações Web](https://learn.microsoft.com/en-us/archive/blogs/dajung/ebook-pnp-performance-testing-guidance-for-web-applications)
