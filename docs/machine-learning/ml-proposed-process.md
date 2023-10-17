# Processo Proposto de Aprendizado de Máquina (ML)

## Introdução

O objetivo deste documento é fornecer orientações para produzir aplicações de aprendizado de máquina (ML) baseadas em código, dados e modelos que podem ser reproduzidos e lançados de forma confiável em ambientes de produção. Ao desenvolver aplicações de ML, consideramos as seguintes abordagens:

* Melhores práticas em engenharia de ML:

  * O desenvolvimento de aplicações de ML deve usar fundamentos de engenharia para garantir entregáveis de software de alta qualidade.
  * A aplicação de ML deve ser lançada com confiabilidade na produção, aproveitando a automação o máximo possível.
  * A aplicação de ML pode ser implantada na produção a qualquer momento. Isso torna a decisão de quando lançá-la uma decisão de negócios, em vez de uma decisão técnica.

* Melhores práticas em pesquisa de ML:

  * Todos os artefatos, especialmente dados, código e modelos de ML, devem ser versionados e gerenciados usando ferramentas e fluxos de trabalho padrão, a fim de facilitar a pesquisa e desenvolvimento contínuos.
  * Enquanto as saídas do modelo podem ser não determinísticas e difíceis de reproduzir, o processo de lançar software de ML na produção deve ser reproduzível.
  * Aspectos de IA responsável são cuidadosamente analisados e abordados.

* Equipe multidisciplinar:

  * É necessária uma equipe multidisciplinar composta por diferentes conjuntos de habilidades em ciência de dados, engenharia de dados, desenvolvimento, operações e especialistas em domínio da indústria.

## Processo de ML

O processo de desenvolvimento de ML proposto consiste em:

1. Compreensão dos dados e do problema
2. Avaliação de IA responsável
3. Estudo de viabilidade
4. Experimentação com modelo de linha de base
5. Avaliação e experimentação do modelo
6. Operacionalização do modelo
    * Testes de unidade e integração
    * Implantação
    * Monitoramento e Observabilidade

### Controle de versão

* Durante todas as etapas do processo, sugere-se que os artefatos sejam controlados por versão. Tipicamente, o processo é iterativo e artefatos versionados podem ajudar na rastreabilidade e revisão. Veja mais [aqui](ml-experimentation.md#controle-de-fonte-e-estrutura-de-pasta-ou-pacote).

### Compreensão do problema

* Defina o problema de negócios para o projeto de ML:
  * Concordar com os critérios de sucesso com o cliente.
  * Identificar fontes de dados potenciais e determinar a disponibilidade dessas fontes.
  * Definir métricas de avaliação de desempenho nos dados de referência.
* Realize uma [avaliação de IA responsável](responsible-ai.md) para garantir o desenvolvimento e implantação da solução de ML de maneira responsável.
* Realize um estudo de viabilidade para avaliar se o problema de negócios é viável de ser resolvido de forma satisfatória usando ML com os dados disponíveis. O objetivo do estudo de viabilidade é mitigar o potencial superinvestimento, garantindo evidências suficientes de que o ML é possível e seria a melhor solução. O estudo também fornece indicações iniciais de como a solução de ML deve ser. Isso garante soluções de qualidade com base em consideração e evidência rigorosas. Consulte [estudo de viabilidade](ml-feasibility-study.md).
* A análise exploratória de dados é realizada e discutida com a equipe.

* **Saída típica**:

  * Código-fonte da exploração de dados (notebooks Jupyter/scripts) e slides/documentação
  * Código inicial do modelo de ML (notebook Jupyter ou scripts)
  * Arquitetura inicial da solução com requisitos iniciais de engenharia de dados
  * Dicionário de dados (se ainda não estiver disponível)
  * Lista de suposições

### Experimentação com Modelo de Linha de Base

* Preparação de dados: criação de conectores de fonte de dados, determinação de serviços de armazenamento a serem usados e versionamento potencial de conjuntos de dados brutos.
* Engenharia de recursos: criação de novos recursos a partir de dados brutos para aumentar o poder preditivo do algoritmo de aprendizado. Os recursos devem capturar informações adicionais que não são aparentes no conjunto de recursos original.
* Divisão de dados em conjuntos de treinamento, validação e teste: criação de conjuntos de dados de treinamento, validação e teste com dados de referência para desenvolver modelos de ML. Isso envolveria a junção ou fusão de vários conjuntos de dados de engenharia de recursos. O conjunto de treinamento é usado para treinar o modelo para encontrar os padrões entre seus recursos e rótulos (dados de referência). O conjunto de validação é usado para avaliar a arquitetura do modelo, e os dados de teste são usados para confirmar a qualidade das previsões do modelo.
* Código inicial para acessar fontes de dados, transformar dados brutos em recursos e treinar modelos, bem como código de pontuação de modelo.
* Durante esta fase, o código de experimentação (notebooks Jupyter ou scripts) e o código de utilidade correspondente devem ser controlados por versão usando ferramentas como o ADO (Azure DevOps).

* **Saída típica**: Notebooks Jupyter ou scripts em Python ou R, resultados iniciais do modelo de linha de base.

Para obter mais informações sobre experimentação, consulte a seção de [experimentação](ml-experimentation.md).

### Avaliação do Modelo

* Compare a eficácia de diferentes algoritmos no problema dado.

* **Saída típica**:
  * O fluxo de avaliação está [totalmente configurado](ml-experimentation.md#avaliação-de-modelo).
  * Experimentos reproduzíveis para as diferentes abordagens testadas.

### Operacionalização do Modelo

* Transformar o código "experimental" para que possa ser implantado. Isso inclui pré-processamento de dados, código de featurização, código de treinamento de modelo (se necessário treinar usando CI/CD) e código de inferência de modelo.

* **Saída típica**:
  * Código de produção (preferencialmente na forma de um pacote) para:
    * Pré-processamento / pós-processamento de dados


    * Servir um modelo
    * Treinar um modelo
  * Scripts de CI/CD.
  * Etapas de reprodutibilidade para o modelo em produção.
  * Veja mais [aqui](ml-model-checklist.md).

#### Testes de Unidade e Integração

* Garantir que o código de produção se comporte da maneira que esperamos e que seus resultados correspondam aos que vimos durante as fases de Avaliação e Experimentação do Modelo.
* Consulte o [teste de ML](ml-testing.md) para obter mais detalhes.
* **Saída típica**: Conjunto de testes com testes de unidade e testes de ponta a ponta criados e concluídos com sucesso.

#### Implantação

* Considerações de [IA responsável](responsible-ai.md), como análise de viés e equidade. Além disso, a explicabilidade/interpretabilidade do modelo também deve ser considerada.
* É recomendável que um ser humano participe do processo de verificação do modelo e aprove a implantação para produção.
* Colocar o modelo em produção, onde ele pode começar a adicionar valor servindo previsões. Artefatos típicos são APIs para acessar o modelo e integrar o modelo à arquitetura da solução.
* Além disso, determinados cenários podem exigir o treinamento periódico do modelo em produção.
* Etapas de reprodutibilidade do modelo em produção estão disponíveis.
* **Saída típica**: Lista de verificação de preparação do modelo [model readiness checklist](ml-model-checklist.md) está concluída.

#### Monitoramento e Observabilidade

* Esta é a fase final, onde garantimos que nosso modelo esteja fazendo o que esperamos na produção.
* Saiba mais sobre a [observabilidade de ML](../observability/ml-observability.md).
* Saiba mais sobre as ofertas de ML da Azure em torno do monitoramento de modelos de ML em produção [aqui](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-enable-data-collection).
* É recomendável considerar a incorporação do processo de monitoramento de deriva de dados na solução de produção. Isso ajudará a detectar possíveis mudanças nos novos conjuntos de dados apresentados para inferência que podem impactar significativamente o desempenho do modelo. Para obter mais informações sobre como detectar a deriva de dados com o Azure ML, consulte o artigo da Microsoft sobre [como monitorar conjuntos de dados](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-monitor-datasets).
* **Saída típica**: Scripts e ferramentas de registro e monitoramento configurados, permissões para que os usuários acessem ferramentas de monitoramento.
