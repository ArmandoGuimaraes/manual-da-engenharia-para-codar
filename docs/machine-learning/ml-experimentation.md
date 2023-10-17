# Experimentação de Modelos

## Visão Geral

A experimentação de modelos de aprendizado de máquina envolve incerteza em torno dos resultados esperados do modelo e futura operacionalização. Para lidar com essa incerteza o máximo possível, propomos um processo semi-estruturado, equilibrando as melhores práticas de engenharia/pesquisa e exploração rápida de modelo/dados.

## Objetivos da experimentação de modelos

- **Desempenho**: Encontrar a solução com melhor desempenho.
- **Operacionalização**: Manter um olho na produção, garantindo que a operacionalização seja viável.
- **Qualidade do código**: Manter a qualidade do código e dos artefatos.
- **Reprodutibilidade**: Manter a pesquisa ativa permitindo o rastreamento e a reprodutibilidade dos experimentos.
- **Colaboração**: Fomentar a colaboração e o trabalho conjunto de várias pessoas na equipe.

## Desafios da experimentação de modelos

- **Processo de tentativa e erro**: Difícil de planejar e estimar durações e capacidade.
- **Rápido e sujo**: Queremos falhar rapidamente e ter uma ideia do que está funcionando eficientemente.
- **Colaboração**: Como formar um processo de tentativa e erro em toda a equipe e brainstorming eficaz.
- **Qualidade do código**: Como manter a qualidade do código que não é de produção durante a pesquisa.
- **Operacionalização**: A mudança entre abordagens pode ter um impacto significativo na operacionalização (por exemplo, GPU/CPU, lote/online, paralelo/sequencial, ambientes de tempo de execução).

Criar um framework de experimentação que facilite a experimentação rápida, colaboração, reprodutibilidade dos experimentos e modelos, avaliação e APIs definidas, e permita que cada membro da equipe se concentre no desenvolvimento e aprimoramento do modelo, enquanto confia no framework para fazer o resto.

As seguintes ferramentas e diretrizes têm como objetivo alcançar os objetivos da experimentação, bem como abordar os desafios mencionados anteriormente.

## Ferramentas e diretrizes para experimentação de modelos bem-sucedida

- [Ambientes virtuais](#ambientes-virtuais)
- [Controle de código-fonte e estrutura de pastas/pacotes](#controle-de-codigo-fonte-e-estrutura-de-pastas-ou-pacotes)
- [Rastreamento de experimentos](#rastreamento-de-experimentos)
- [Abstrações de conjuntos de dados e modelos](#abstracoes-de-conjuntos-de-dados-e-modelos)
- [Avaliação de modelos](#avaliacao-de-modelos)

### Ambientes virtuais

Em linguagens como Python e R, é sempre aconselhável usar ambientes virtuais. Ambientes virtuais facilitam a reprodutibilidade, colaboração e productização.
Ambientes virtuais nos permitem ser consistentes em nossos ambientes locais de desenvolvimento, assim como com os recursos de computação. Os arquivos de configuração desses ambientes podem ser usados para construir o código a partir da fonte de uma maneira consistente.
Para obter mais detalhes sobre por que precisamos de ambientes virtuais, visite [este post no blog](https://realpython.com/python-virtual-environments-a-primer/#why-the-need-for-virtual-environments).

#### Qual framework de ambiente virtual devo escolher

Todos os frameworks de ambiente virtual criam isolamento, alguns também propõem gerenciamento de dependências e recursos adicionais. A decisão sobre qual framework usar depende da complexidade do ambiente de desenvolvimento (dependências e outros recursos necessários) e da facilidade de uso do framework.

#### Tipos de ambientes virtuais

Na ISE, frequentemente escolhemos entre `venv`, `Conda` ou `Poetry`, dependendo dos requisitos e complexidade do projeto.

- [venv](https://docs.python.org/3/library/venv.html) está incluído no Python, é o mais fácil de usar, mas carece de recursos mais avançados, como gerenciamento de dependências.
- [Conda](https://docs.conda.io/en/latest/) é um framework popular de gerenciamento de pacotes, dependências e ambiente. Suporta várias pilhas (Python, R) e várias versões do mesmo ambiente (por exemplo, várias versões do Python). O `Conda` mantém seu próprio repositório de pacotes, portanto, alguns pacotes podem não ser baixados e gerenciados diretamente pelo `Conda`.
- [Poetry](https://python-poetry.org/) é um sistema de gerenciamento de dependências Python que gerencia dependências de maneira padrão usando arquivos `pyproject.toml` e arquivos `lock`. Semelhante ao `Conda`, o processo de resolução de dependências do `Poetry` às vezes é lento (consulte [FAQ](https://python-poetry.org/docs/faq/#why-is-the-dependency-resolution-process-slow)), mas em casos em que problemas de dependência são comuns ou complicados, ele fornece uma maneira robusta de criar ambientes reproduzíveis e estáveis.

#### Resultados esperados para a configuração de ambientes virtuais

1. Documentação descrevendo como criar o ambiente virtual selecionado e como instalar as dependências.
2. Arquivos de configuração do ambiente, se aplicável (por exemplo, `requirements.txt` para `venv`, [environment.yml](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#creating-an-environment-from-an-environment-yml-file) para `Conda` ou [pyrpoject.toml](https://python-poetry.org/docs/pyproject/) para `Poetry`).

#### Benefícios de ambientes virtuais

- Productização
- Colaboração
- Reprodutibilidade

### Controle de código-fonte e estr

utura de pastas ou pacotes

Projetos de ML aplicado frequentemente contêm código-fonte, notebooks, scripts de DevOps, documentação, recursos científicos, conjuntos de dados e muito mais. Recomendamos criar uma estrutura de pastas acordada para manter os recursos organizados. Considere decidir sobre uma estrutura de pastas genérica para projetos (por exemplo, que contenha as pastas `data`, `src`, `docs` e `notebooks`), ou adote estruturas populares como a [estrutura de pastas CookieCutter Data Science](https://drivendata.github.io/cookiecutter-data-science/).

[Controle de código-fonte](../source-control/README.md) deve ser aplicado para permitir colaboração, versionamento, revisões de código, rastreabilidade e backup. Em projetos de ciência de dados, o controle de código-fonte deve ser usado para código, e o armazenamento e versionamento de outros artefatos (por exemplo, dados, literatura científica) deve ser decidido dependendo do cenário.

#### Resultados da estrutura de pastas e controle de código-fonte

- Estrutura de pastas definida para que todos os usuários a utilizem, enviada para o repositório.
- Arquivo [.gitignore](https://git-scm.com/docs/gitignore) determinando quais pastas devem ser sincronizadas com o `git` e quais devem ser mantidas localmente. Por exemplo, [este](https://github.com/drivendata/cookiecutter-data-science/blob/master/%7B%7B%20cookiecutter.repo_name%20%7D%7D/.gitignore).
- Determinar como os notebooks são armazenados e versionados (por exemplo, [remover a saída dos notebooks Jupyter](https://github.com/kynan/nbstripout)).

#### Benefícios de controle de código-fonte e estrutura de pastas

- Colaboração
- Reprodutibilidade
- Qualidade do código

### Rastreamento de experimentos

Ferramentas de rastreamento de experimentos permitem que cientistas de dados e pesquisadores acompanhem experimentos anteriores para uma melhor compreensão do processo de experimentação e para a reproducibilidade de experimentos ou modelos.

#### Tipos de frameworks de rastreamento de experimentos

Frameworks de rastreamento de experimentos diferem pelo conjunto de recursos que oferecem para coletar metadados de experimentos e comparar e analisar experimentos. Na ISE, principalmente usamos [MLFlow](https://mlflow.org/) no [Databricks](https://databricks.com/product/managed-mlflow) ou [Azure ML Experimentation](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-track-experiments). Observe que alguns frameworks de rastreamento de experimentos exigem uma implantação, enquanto outros são SaaS.

#### Resultados do rastreamento de experimentos

1. Decida sobre um framework de rastreamento de experimentos.
2. Garanta que ele seja acessível a todos os usuários.
3. Documente a configuração em ambientes locais.
4. Defina conjuntos de dados e avaliações de uma maneira que permita a comparação de todos os experimentos. **A consistência nos conjuntos de dados e avaliação é fundamental para a comparação de experimentos**.
5. Garanta a plena reprodutibilidade, assegurando que todos os detalhes necessários sejam rastreados (ou seja, nomes e versões de conjuntos de dados, parâmetros, código, ambiente).

#### Benefícios do rastreamento de experimentos

- Desempenho do modelo
- Reprodutibilidade
- Colaboração
- Qualidade do código

### Abstrações de conjuntos de dados e modelos

Ao criar abstrações para blocos de construção (por exemplo, conjuntos de dados, modelos, avaliadores), permitimos a fácil introdução de novas lógicas na tubulação de experimentação, mantendo intacta a tubulação de experimentação acordada.

Essas abstrações podem ser criadas usando diferentes mecanismos. Por exemplo, podemos usar soluções de Programação Orientada a Objetos (OOP) como classes abstratas:

- [Um exemplo do scikit-learn descrevendo a criação de novos estimadores compatíveis com a API](https://scikit-learn.org/stable/developers/develop.html).
- [Um exemplo do PyTorch sobre a extensão da classe abstrata `Dataset`](https://pytorch.org/tutorials

/beginner/data_loading_tutorial.html#dataset-class).

#### Resultados de abstração

1. Diferentes blocos de construção têm APIs definidas que permitem substituí-los ou estendê-los.
2. A substituição dos blocos de construção não quebra o fluxo original de experimentação.
3. Blocos de construção simulados são usados para testes unitários.
4. APIs/mocks são compartilhados com as equipes de engenharia para integração com outros módulos.

#### Benefícios de abstrações

- Colaboração
- Qualidade do código
- Reprodutibilidade
- Operacionalização
- Desempenho do modelo

### Avaliação de modelos

Ao decidir sobre a avaliação do modelo de ML/processo, considere a seguinte lista de verificação:

- [ ] A lógica de avaliação é aprovada por todas as partes interessadas.
- [ ] A relação entre a lógica de avaliação e os KPIs de negócios é analisada e decidida.
- [ ] O fluxo de avaliação é aplicável a todos os modelos presentes e futuros (ou seja, não assume alguma estrutura de previsão ou processo específico do método).
- [ ] O código de avaliação é testado unitariamente e revisado por todos os membros da equipe.
- [ ] O fluxo de avaliação facilita análises adicionais de resultados e erros.

## Resultados do processo de desenvolvimento de avaliação

1. A estratégia de avaliação é acordada por todas as partes interessadas.
2. Pesquisa e discussão sobre vários métodos e métricas de avaliação são documentados.
3. O código que contém a lógica e as estruturas de dados para avaliação é revisado e testado.
4. A documentação sobre como aplicar a avaliação é revisada.
5. As métricas de desempenho são rastreadas automaticamente no rastreador de experimentos.

## Benefícios do processo de desenvolvimento de avaliação

- Desempenho do modelo
- Qualidade do código
- Colaboração
- Reprodutibilidade
