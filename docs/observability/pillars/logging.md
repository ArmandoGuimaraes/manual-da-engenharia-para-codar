# Registro

## Visão Geral

Logs são eventos discretos com o objetivo de ajudar os engenheiros a identificar área(s) de problemas durante falhas.

## Métodos de Coleta

Quando se trata de métodos de coleta de logs, duas das técnicas padrão são escrita direta ou uma abordagem baseada em agente.

Eventos de log escritos diretamente são tratados no processo do componente específico, geralmente utilizando uma biblioteca fornecida. O [Azure Monitor](https://azure.microsoft.com/pt-br/services/monitor) tem capacidades de envio direto, mas não é recomendado para uso sério/produção. Essa abordagem tem algumas vantagens:

- Não há processo externo para configurar ou monitorar.
- Sem gerenciamento de arquivos de log (rolagem, expiração) para evitar problemas de falta de espaço em disco.

As potenciais compensações dessa abordagem são:

- Possivelmente, maior uso de memória se a biblioteca específica estiver usando um buffer de memória.
- Em caso de interrupção prolongada do serviço, os dados de log podem ser perdidos ou truncados devido a restrições de buffer.
- O registro de vários processos de componentes gerenciará e emitirá logs individualmente, o que pode ser mais complexo de gerenciar para a carga de saída.

A coleta de logs baseada em agente depende de um processo externo em execução na máquina host, com o componente específico emitindo dados de log para stdout ou arquivo. A gravação de dados de log em stdout é a prática preferida ao executar aplicativos em um ambiente de contêiner, como o Kubernetes. O tempo de execução do contêiner redireciona a saída para arquivos, que podem ser processados por um agente. [Azure Monitor](https://azure.microsoft.com/pt-br/services/monitor), [Grafana Loki](https://github.com/grafana/loki) [Logstash da Elastic](https://www.elastic.co/logstash) e [Fluent Bit](https://fluentbit.io/) são exemplos de agentes de envio de logs.

Existem várias vantagens ao usar um agente para coletar e enviar arquivos de log:

- Configuração centralizada.
- Coleta de várias fontes de dados com um único processo.
- Pré-processamento e filtragem local dos dados de log antes de enviá-los para um serviço central.
- Utilização do espaço em disco como um buffer de dados durante uma interrupção do serviço.

Essa abordagem não é isenta de compensações:

- Recursos exclusivos de CPU e memória são necessários para o processamento de dados de log.
- Espaço em disco persistente para o buffer.

## Melhores Práticas

- Preste atenção aos níveis de registro. Registrar em excesso aumentará os custos e diminuirá a taxa de transferência da aplicação.
- Certifique-se de que a configuração de registro possa ser modificada sem alterações no código. Idealmente, torne-a ajustável sem a necessidade de reiniciar a aplicação.
- Se disponível, aproveite os níveis de registro por categoria, permitindo uma configuração de registro granular.
- Verifique os níveis de log antes de registrar, evitando assim custos de alocação e manipulação de strings.
- Certifique-se de que as versões do serviço estejam incluídas nos logs para identificar versões problemáticas.
- Registre uma exceção levantada apenas uma vez. Em seus manipuladores, capture apenas exceções esperadas que você possa lidar adequadamente (mesmo com um código de retorno específico). Se desejar registrar e relançar, deixe para o manipulador de exceção de nível superior. Faça a quantidade mínima de trabalho de limpeza necessário e, em seguida, lance para manter a pilha de rastreamento original. Não registre um aviso ou rastreamento de pilha para exceções esperadas (por exemplo, códigos HTTP 404 ou 403 devidamente esperados).
- Ajuste os níveis de registro na produção (>= aviso, por exemplo). Durante um novo lançamento, a verbosidade pode ser aumentada para facilitar a identificação de bugs.
- Se estiver usando amostragem, implemente-a no nível do serviço em vez de defini-la no sistema de registro. Dessa forma, temos controle sobre o que é registrado. Um benefício adicional é a redução no número de idas e vindas.
- Inclua apenas falhas de verificações de integridade e solicitações não orientadas por negócios.
- Certifique-se de que uma disfunção do sistema downstream não cause armazenamento repetitivo de logs.
- Não reinvente a roda, use ferramentas existentes para coletar e analisar os dados.
- Garanta que as políticas e restrições de informações de identificação pessoal sejam seguidas.
- Garanta que erros e exceções em serviços dependentes sejam capturados e registrados. Por exemplo, se uma aplicação usa cache Redis, Service Bus ou qualquer outro serviço, quaisquer erros/exceções gerados ao acessar esses serviços devem ser capturados e registrados.

### Se há dados de log suficientes, há necessidade de instrumentação de métricas?

[Logs vs Métricas vs Rastreamentos](../log-vs-metric-vs-trace.md) aborda algumas orientações de alto nível sobre quando utilizar dados métricos e quando utilizar dados de log. Ambos desempenham um papel valioso na criação de sistemas observáveis.

### Tem dificuldade em identificar o que registrar?



**No início da aplicação**:

- Erros irreparáveis no início.
- Avisos se a aplicação ainda estiver em execução, mas não como o esperado (ou seja, não fornecendo a string de conexão do blob, recorrendo assim aos arquivos locais. Outro exemplo é se há necessidade de falha de volta para um serviço secundário ou um estado conhecido bom, porque não recebeu uma resposta de uma dependência primária).
- Informações sobre o estado do serviço no início (número da versão, configurações carregadas etc.).

**Por solicitação recebida**:

- Informações básicas para cada solicitação recebida: a URL (limpa de qualquer dado pessoalmente identificável, ou seja, PII), quaisquer dimensões de usuário/inquilino/solicitação, código de resposta retornado, latência da solicitação para resposta, tamanho do payload, contagens de registros etc. (o que você precisa para aprender algo com os dados agregados)
- Aviso para quaisquer exceções inesperadas, capturadas apenas no controlador/interceptador superior e registradas com ou ao lado das informações da solicitação, com rastreamento de pilha. Retorne um erro 500. Este código não sabe o que aconteceu.

**Por solicitação enviada**:

- Informações básicas para cada solicitação enviada: a URL (limpa de qualquer dado pessoalmente identificável, ou seja, PII), quaisquer dimensões de usuário/inquilino/solicitação, código de resposta retornado, latência da solicitação para resposta, tamanhos de payload, contagens de registros retornados etc. Relate a disponibilidade percebida e a latência das dependências, incluindo dados de fatiamento/agrupamento que possam ajudar em análises posteriores.
