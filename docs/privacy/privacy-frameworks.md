# Quadro de Referência para Privacidade

Nesta seção, discutirei ferramentas e estruturas relacionadas à privacidade que podem ser aproveitadas quando a análise de dados ou o desenvolvimento de modelos precisa ser realizado em dados privados. É importante ressaltar que o uso dessas estruturas ainda requer a aderência às regulamentações de privacidade e a aplicação de medidas adicionais de segurança.

## Cenários Típicos para Utilização de um Quadro de Privacidade

- Compartilhar dados ou resultados enquanto preserva a privacidade dos indivíduos.
- Realizar análises ou modelagem estatística em dados privados.
- Desenvolver modelos de ML que preservem a privacidade e pipelines de dados.

## Quadros de Privacidade

Proteger dados privados envolve todo o ciclo de vida dos dados, desde a aquisição até o armazenamento, processamento, análise, modelagem e uso em relatórios ou modelos de machine learning. Deve-se aplicar salvaguardas e restrições adequadas em cada uma dessas fases.

Nesta seção, fornecerei uma lista não exaustiva de quadros de privacidade que podem ser aproveitados para proteger e preservar a privacidade.

Concentramos-nos em quatro casos principais no ciclo de vida dos dados:

1. [Obtenção de dados não sensíveis](#obtenção-de-dados-não-sensíveis).
2. [Estabelecimento de ambientes de pesquisa e modelagem confiáveis](#ambientes-de-pesquisa-e-modelagem-confiáveis).
3. [Criação de pipelines de dados e ML preservadores de privacidade](#pipelines-de-dados-e-ml-preservadores-de-privacidade).
4. [Prevenção de perda de dados](#prevenção-de-perda-de-dados).

### Obtenção de dados não sensíveis

Em muitos cenários, analistas, pesquisadores e cientistas de dados precisam de acesso a uma versão não sensível ou amostra dos dados privados. Nesta seção, discutirei duas abordagens para obter dados não sensíveis.

**Observação:** Essas duas abordagens não garantem que o resultado não incluirá dados privados, e medidas adicionais devem ser aplicadas.

#### Desidentificação de dados

A desidentificação é o processo de aplicar um conjunto de transformações a um conjunto de dados para reduzir o risco de divulgação não intencional de dados pessoais. Isso envolve a remoção ou substituição de identificadores diretos (como nome ou número de segurança social) ou quasi-identificadores, que podem ser usados para reidentificação com informações externas adicionais.

A desidentificação pode ser aplicada a diferentes tipos de dados, como dados estruturados, imagens e texto. No entanto, a desidentificação de dados não estruturados geralmente envolve abordagens estatísticas que podem resultar na remoção ou substituição não detectada de informações pessoais identificáveis (PII) ou informações não privadas.

Aqui, vou destacar várias soluções de desidentificação disponíveis como open source:

- [Presidio](https://microsoft.github.io/presidio): O Presidio ajuda a garantir que dados sensíveis sejam gerenciados e governados adequadamente. Ele fornece módulos de identificação e anonimização rápidos para entidades privadas em texto, como números de cartão de crédito, nomes, locais, números de previdência social, carteiras de bitcoin, números de telefone dos EUA, dados financeiros e muito mais em texto e imagens não estruturados. É útil quando alta personalização é necessária, por exemplo, para detectar entidades de PII personalizadas ou idiomas específicos.

- [FHIR Tools for Anonymization](https://github.com/microsoft/FHIR-Tools-for-Anonymization): Este projeto de código aberto ajuda a anonimizar dados de saúde FHIR (Recursos de Interoperabilidade Rápida em Saúde) no local ou na nuvem, para uso secundário, como pesquisa e saúde pública. Trabalha com o formato FHIR (Stu3 e R4) e permite diferentes estratégias de anonimização (deslocamento de datas, cripto-hash, criptografia, substituição, perturbação, generalização).

- [ARX](https://arx.deidentifier.org/): O ARX permite a anonimização usando modelos estatísticos, como k-anonimato, ℓ-diversidade, t-closeness e δ-presence. É útil para validar a anonimização de dados agregados.

- [k-Anonymity](https://github.com/Nuclearstar/K-Anonymity): Este repositório do GitHub contém exemplos de como produzir conjuntos de dados k-anônimos. O k-anonimato protege a privacidade das pessoas agrupando seus atributos em grupos de pelo menos *k* pessoas.

#### Geração de dados sintéticos

Um conjunto de dados sintéticos é um repositório de dados gerados a partir de dados reais e possui as mesmas propriedades estatísticas dos dados reais. O grau de semelhança entre um conjunto de dados sintéticos e os dados reais é uma medida de utilidade. Os conjuntos de dados sintéticos podem ser benéficos em aplicações sensíveis, como classificações médicas ou modelagem financeira, onde obter um conjunto de dados rotulados de alta qualidade é muitas vezes proibitivo.

Ao determinar o melhor método para criar dados sintéticos, é essencial primeiro considerar que tipo de dados sintéticos você deseja ter. Existem duas categorias amplas de escolha, cada uma com benefícios e desvantagens diferentes:

- Totalmente sintético: Esses dados não contêm nenhum dado original, o que significa que a reidentificação de qualquer unidade individual é quase impossível, e todas as variáveis ainda estão totalmente disponíveis.

- Parcialmente sintético: Apenas dados sensíveis são substituídos por dados sintéticos, o que requer uma forte dependência do modelo de imputação. Isso leva a uma dependência diminuída do modelo, mas significa que alguma divulgação é possível devido aos valores reais dentro do conjunto de dados.

Aqui estão algumas soluções para geração de dados sintéticos:

- [Synthea](https://synthetichealth.github.io/synthea/): O Synthea foi desenvolvido com base em numerosas fontes de dados coletadas na Internet, incluindo demografia do US Census Bureau, taxas de prevalência e incidência dos Centros de Controle e

 Prevenção de Doenças e relatórios dos Institutos Nacionais de Saúde. Os modelos de doenças e tratamentos incluem anotações e citações para todos os dados, estatísticas e tratamentos. Esses modelos de doenças e tratamentos interagem adequadamente com o registro de saúde.

- [Gerador de conjunto de dados PII](https://github.com/microsoft/presidio-research/blob/master/presidio_evaluator/data_generator/README.md): Um gerador de dados sintéticos desenvolvido com base no Fake Name Generator. Ele pode criar uma lista de amostras de entrada que contêm entidades de PII falsas no lugar de espaços reservados.

- [CheckList](https://github.com/marcotcr/checklist): O CheckList fornece um quadro para técnicas de perturbação para avaliar sistematicamente as capacidades comportamentais específicas de modelos de processamento de linguagem natural (NLP).

- [Mimesis](https://github.com/lk-geimfari/mimesis): O Mimesis é um gerador de dados falsos de alto desempenho para Python, que fornece dados para diversas finalidades em diferentes idiomas.

- [Faker](https://github.com/joke2k/faker): O Faker é um pacote Python que gera dados falsos para você. Pode ser usado para preencher bancos de dados, criar documentos XML com boa aparência, preencher seu sistema de persistência para testes de estresse ou anonimizar dados retirados de um serviço de produção.

- [Plaitpy](https://github.com/plaitpy/plaitpy): O Plaitpy foi projetado para modelar facilmente dados falsos que têm uma forma interessante. Muitos geradores de dados falsos modelam seus dados como uma coleção de variáveis IID; com o Plaitpy, você pode unir essas variáveis em um modelo mais coerente.

### Ambientes de Pesquisa e Modelagem Confiáveis

#### Ambientes de Pesquisa Confiáveis

Ambientes de Pesquisa Confiáveis (TREs) permitem que as organizações criem espaços de trabalho seguros para analistas, cientistas de dados e pesquisadores que precisam de acesso a dados sensíveis.

TREs impõem um limite seguro em torno de espaços de trabalho distintos para permitir controles de governança de informações. Cada espaço de trabalho é acessível por um conjunto de usuários autorizados, previne a exfiltração de dados sensíveis e tem acesso a um ou mais conjuntos de dados fornecidos pela plataforma de dados.

Aqui estão algumas alternativas para Ambientes de Pesquisa Confiáveis:

- [Azure Trusted Research Environment](https://github.com/microsoft/azuretre): Um ambiente TRE de código aberto para o Azure.

- [Aridhia DRE](https://appsource.microsoft.com/en-us/product/web-apps/aridhiainformatics.analytixagility_workspace_123?tab=Overview): Um ambiente de pesquisa confiável.

#### Aprendizado de Máquina "Sem Olhos"

Em certas situações, os cientistas de dados podem precisar treinar modelos com dados que não têm permissão para visualizar. Nesses casos, é recomendável uma abordagem "sem olhos".

Uma abordagem "sem olhos" oferece a um cientista de dados um ambiente no qual os scripts podem ser executados nos dados, mas o acesso direto às amostras não é permitido. Durante o processamento dentro do ambiente "sem olhos", apenas determinadas saídas (por exemplo, logs) podem ser extraídas de volta para o usuário. Isso permite que um usuário envie um script que treina um modelo e inspecione o desempenho do modelo, mas não veja em quais amostras o modelo previu uma saída incorreta.

Além do ambiente "sem olhos", essa abordagem geralmente envolve o fornecimento de acesso a um conjunto de dados "com olhos", que é um conjunto de dados representativo e limpo para fins de design do modelo. O conjunto de dados "com olhos" geralmente é um subconjunto desidentificado do conjunto de dados privados ou um conjunto de dados sintéticos gerado com base nas características do conjunto de dados privados.

#### Plataformas de Compartilhamento de Dados Privados

Várias ferramentas e sistemas permitem que diferentes partes compartilhem dados com terceiros, protegendo entidades privadas e processem dados de forma segura, reduzindo a probabilidade de exfiltração de dados. Essas ferramentas incluem sistemas de Computação Segura entre Várias Partes (SMPC), sistemas de Criptografia Homomórfica, Computação Confidencial, estruturas de análise de dados privados, como [PySift](https://github.com/OpenMined/PySyft), entre outras.

### Pipelines de Dados e ML Preservadores de Privacidade

Mesmo quando nossos dados estão seguros, entidades privadas podem ainda ser extraídas quando os dados são consumidos. Pipelines de dados e modelos de ML preservadores de privacidade focam em minimizar o risco de exfiltração de dados privados durante a consulta de dados ou previsões de modelos.

#### Privacidade Diferencial

A privacidade diferencial (DP) é um sistema que permite extrair insights significativos de conjuntos de dados sobre subgrupos de pessoas, ao mesmo tempo em que oferece garantias sólidas de proteção à privacidade de qualquer indivíduo. Isso é tipicamente alcançado adicionando um pequeno ruído estatístico às informações de cada indivíduo, introduzindo incerteza nos dados. No entanto, as informações obtidas ainda representam com precisão o que pretendemos aprender sobre a população como um todo. Essa abordagem é conhecida por ser robusta a ataques de reidentificação e reconstrução de dados por adversários que possuem informações auxiliares.

Ferramentas que implementam DP incluem [SmartNoise](https://github.com/opendifferentialprivacy/smartnoise-samples) e [Tensorflow Privacy](https://github.com/tensorflow/privacy), entre outras.

#### Criptografia Homomórfica

A criptografia homomórfica (HE) é uma forma de criptografia que permite realizar cálculos em dados criptografados sem descriptografá-los primeiro. O resultado da computação *F* está em uma forma criptografada, que, ao ser descriptografada, nos dá o mesmo resultado se a computação *F* fosse feita nos dados brutos não criptografados.

Frameworks de criptografia homomórfica incluem o [Microsoft SEAL](https://www.microsoft.com/en-us/research/project/microsoft-seal), [Palis

ade](https://palisade-crypto.org/), [PySift](https://github.com/OpenMined/PySyft) e outros.

#### Aprendizado Federado

O aprendizado federado é uma técnica de aprendizado de máquina que permite o treinamento de modelos de ML de forma descentralizada, sem a necessidade de compartilhar os dados reais. Em vez de enviar dados para o mecanismo de processamento do modelo, a abordagem é distribuir o modelo para os diferentes proprietários de dados e realizar o treinamento de forma distribuída.

Frameworks de aprendizado federado incluem [TensorFlow Federated Learning](https://github.com/tensorflow/federated), [FATE](https://fate.fedai.org/) e [IBM Federated Learning](https://github.com/IBM/federated-learning-lib), entre outros.

### Prevenção de Perda de Dados

As organizações têm informações sensíveis sob seu controle, como dados financeiros, dados proprietários, números de cartões de crédito, registros de saúde ou números de previdência social. Para ajudar a proteger esses dados sensíveis e reduzir os riscos, é necessário impedir que os usuários compartilhem inadequadamente esses dados com pessoas que não devem tê-los. Essa prática é chamada de [prevenção de perda de dados (DLP)](https://learn.microsoft.com/en-us/microsoft-365/compliance/dlp-learn-about-dlp).

Abaixo, focamos em dois aspectos da DLP: classificação de dados sensíveis e gerenciamento de acesso.

#### Classificação de Dados Sensíveis

A classificação de dados sensíveis é um aspecto importante da DLP, pois permite que as organizações rastreiem, monitorem, protejam e identifiquem dados sensíveis e privados. Além disso, diferentes níveis de sensibilidade podem ser aplicados a diferentes itens de dados, facilitando a governança e a catalogação adequadas.

Normalmente, existem quatro níveis de classificação de dados:

1. Público
2. Interno
3. Confidencial
4. Restrito / Altamente confidencial

Ferramentas para classificação de dados no Azure incluem o [Microsoft Information Protection](https://learn.microsoft.com/en-us/microsoft-365/compliance/information-protection) (MIP), [Azure Purview](https://azure.microsoft.com/en-us/services/purview/), [Data Discovery & Classification for Azure SQL Database, Azure SQL Managed Instance e Azure Synapse](https://learn.microsoft.com/en-us/azure/azure-sql/database/data-discovery-and-classification-overview) e [Data Discovery & Classification for SQL Server](https://learn.microsoft.com/en-us/sql/relational-databases/security/sql-data-discovery-and-classification?view=sql-server-ver15&tabs=t-sql), entre outros.

Frequentemente, ferramentas usadas para desidentificação também podem servir como classificadores de dados sensíveis. Consulte [ferramentas de desidentificação de dados](#data-de-identification) para tais ferramentas.

Recursos adicionais:

- [Diretrizes de exemplo para classificação de dados](https://www.cmu.edu/iso/governance/guidelines/data-classification.html)
- [Saiba sobre os níveis de sensibilidade](https://learn.microsoft.com/en-us/microsoft-365/compliance/sensitivity-labels?view=o365-worldwide)

#### Gerenciamento de Acesso

O controle de acesso é um componente importante da privacidade desde o início e faz parte da proteção geral do ciclo de vida dos dados. Um controle de acesso bem-sucedido restringirá o acesso apenas a indivíduos autorizados que devem ter acesso aos dados. Uma vez que os dados estão seguros em um ambiente, é importante revisar quem deve acessar esses dados e para que finalidade. O controle de acesso pode ser auditado com uma estratégia de registro abrangente, que pode incluir a integração de [registros de atividades](https://learn.microsoft.com/en-us/azure/azure-monitor/platform/platform-logs-overview) que podem fornecer insights sobre operações realizadas em recursos em uma assinatura.

- [OWASP Access Control Cheat Sheet](https://github.com/OWASP/CheatSheetSeries/blob/master/cheatsheets/Access_Control_Cheat_Sheet.md)
