# Observabilidade em Machine Learning

O processo de desenvolvimento de sistemas de software com componentes de aprendizado de máquina é mais complexo do que o software tradicional. Precisamos monitorar mudanças e variações em três dimensões: o código, o modelo e os dados. Podemos distinguir duas etapas na vida de tal sistema: experimentação e produção, que requerem abordagens diferentes para observabilidade, como discutido abaixo:

## Experimentação e ajuste do modelo

A experimentação é um processo de encontrar um modelo de aprendizado de máquina adequado e seus parâmetros por meio do treinamento e avaliação desses modelos com um ou mais conjuntos de dados.

Ao desenvolver e ajustar modelos de aprendizado de máquina, os cientistas de dados estão interessados em observar e comparar métricas de desempenho selecionadas para vários parâmetros do modelo. Eles também precisam de uma maneira confiável de reproduzir um processo de treinamento, de modo que um determinado conjunto de dados e parâmetros produza os mesmos modelos.

Existem muitas soluções disponíveis para avaliação de métricas de modelo, tanto de código aberto (como o MLFlow) quanto proprietárias (como o Azure Machine Learning Service), e algumas servem para diferentes propósitos. Para capturar métricas de modelo, existem, entre outras, as seguintes opções disponíveis:

[Azure Machine Learning Service SDK](https://ml.azure.com/)
O Azure Machine Learning Service fornece um SDK para Python, R e C# para capturar suas métricas de avaliação em um Experimento do Azure Machine Learning service (AML). Os experimentos são visualizados no painel do AML. A reprodutibilidade é alcançada armazenando o código ou instantâneo do notebook juntamente com a métrica visualizada. Você pode criar Conjuntos de Dados versionados dentro do serviço Azure Machine Learning.

[MLFlow (para Databricks)](https://learn.microsoft.com/en-us/azure/databricks/applications/mlflow/)
O MLFlow é uma estrutura de código aberto e pode ser hospedado no Azure Databricks como seu servidor de rastreamento remoto (atualmente é a única solução que oferece integração de primeira parte com o Databricks). Você pode usar o componente de rastreamento do SDK do MLFlow para capturar suas métricas de avaliação ou qualquer parâmetro que desejar e acompanhá-lo no quadro de experimentação no Azure Databricks. O código-fonte e a versão do conjunto de dados também são salvos com o instantâneo do log para fornecer a reprodutibilidade.

[TensorBoard](https://www.tensorflow.org/tensorboard/)
O TensorBoard é uma ferramenta popular entre os cientistas de dados para visualizar métricas específicas de execuções de Aprendizado Profundo, especialmente de execuções TensorFlow. O TensorBoard não é uma ferramenta de MLOps como o AML/MLFlow e, portanto, não oferece capacidades extensas de registro. Ele é projetado para ser transitório; portanto, pode ser usado como complemento a uma ferramenta de MLOps de ponta a ponta como o AML, mas não como uma ferramenta de MLOps completa.

[Application Insights](https://learn.microsoft.com/en-us/azure/azure-monitor/app/app-insights-overview)
O Application Insights pode ser usado como um sink alternativo para capturar métricas de modelo e, portanto, pode oferecer opções mais extensas, pois as métricas podem ser transferidas para, por exemplo, um painel do PowerBI. Também permite a consulta de logs. No entanto, essa solução significa que um aplicativo personalizado precisa ser desenvolvido para enviar logs para o AppInsights (usando, por exemplo, o OpenCensus Python SDK), o que implicaria esforço extra na criação/manutenção de código personalizado.

Uma comparação detalhada das quatro ferramentas pode ser encontrada da seguinte forma:

|                           | Azure ML      | MLFlow      | TensorBoard   | Application Insights |
|---------------------------| ----------- | ----------- | -----------   | -----------          |
| **Suporte a métricas**       | Valores, imagens, matrizes, logs | Valores, imagens, matrizes e gráficos como arquivos | Métricas relevantes para a pesquisa em DL | Valores, imagens, matrizes, logs
| **Personalizável**         | Básico | Básico | Muito básico | Alto
| **Métricas acessíveis**    | Portal AML, SDK AML | Interface do MLFlow, API de serviço de rastreamento | Interface do Tensorboard, objeto de histórico |

 Application Insights
| **Logs acessíveis**       | Logs contínuos gravados em arquivos .txt no armazenamento de blob, acessíveis via blob ou portal AML. Não é possível consultar | Logs contínuos não são armazenados | Logs contínuos não são armazenados | Application Insights no portal Azure. Consultável com KQL
| **Facilidade de uso e configuração** | Muito direto, apenas um portal | Mais partes móveis devido ao servidor de rastreamento remoto | Um pouco de sobrecarga de processo. Também depende do framework de ML | Mais partes móveis, já que um aplicativo personalizado precisa ser mantido
| **Compartilhamento**          | Entre pessoas com acesso ao espaço de trabalho AML | Entre pessoas com acesso ao servidor de rastreamento remoto | Entre pessoas com acesso ao mesmo diretório | Entre pessoas com acesso ao AppInsights

## Modelo em produção

O modelo treinado pode ser implantado em produção como um contêiner. O Azure Machine Learning service fornece um SDK para implantar modelos como Instância de Contêiner do Azure e publica um ponto de extremidade REST. Você pode monitorá-lo usando métodos de observação de microsserviços (para mais detalhes, consulte a seção [Receitas](README.md)). O MLFlow é uma maneira alternativa de implantar modelos de aprendizado de máquina como um serviço.

## Treinamento e Retreinamento

Para re-treinar automaticamente o modelo, você pode usar Pipelines do AML ou Azure Databricks. Ao re-treinar com Pipelines do AML, você pode monitorar informações de cada execução, incluindo a saída, logs e várias métricas no painel de experimentos do Azure, ou extrair manualmente usando o SDK do AML.

## Desempenho do modelo ao longo do tempo: desvio de dados

Retreinamos modelos de aprendizado de máquina para melhorar seu desempenho e torná-los mais alinhados com os dados que mudam com o tempo. No entanto, em alguns casos, o desempenho do modelo pode degradar. Isso pode acontecer caso os dados mudem drasticamente e não apresentem mais os padrões que observamos durante o desenvolvimento do modelo. Esse efeito é chamado de desvio de dados. O Azure Machine Learning Service possui uma funcionalidade de visualização e relatório de desvio de dados em versão de prévia. Este [artigo](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-monitor-datasets) descreve isso em detalhes.

## Versionamento de dados

É uma prática recomendada adicionar uma versão a todos os conjuntos de dados. Você pode criar um Conjunto de Dados do Azure ML versionado para esse fim ou versioná-lo manualmente se estiver usando outros sistemas.
