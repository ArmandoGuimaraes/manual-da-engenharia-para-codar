# Exploração de Dados

Após a [concepção](./ml-formulacao-problema-concepcao.md) e tipicamente como parte do [estudo de viabilidade de ML](./ml-estudo-viabilidade.md), o próximo passo é confirmar o acesso aos recursos e, em seguida, mergulhar profundamente nos dados disponíveis por meio de workshops de exploração de dados.

## Objetivo do Workshop de Exploração de Dados

O objetivo do workshop de exploração de dados é o seguinte:

1. Garantir que a equipe tenha acesso aos dados e recursos computacionais necessários para o estudo de viabilidade de ML.

2. Certificar-se de que os dados fornecidos têm qualidade e são relevantes para a solução de ML.

3. Certificar-se de que a equipe do projeto tenha um bom entendimento dos dados.

4. Certificar-se de que os SMEs (Especialistas em Assunto) necessários estejam presentes no Workshop de Exploração de Dados.

5. Listar as pessoas necessárias para o workshop de exploração de dados.

## Acesso a Recursos

Antes de iniciar os workshops de exploração de dados, é importante confirmar que você tem acesso aos recursos necessários (incluindo dados).

Abaixo está uma lista de **exemplo** de perguntas a serem consideradas antes de iniciar um workshop de exploração de dados.

1. Quais são os requisitos para a criação de uma conta para que a equipe possa acessar dados e recursos computacionais?
2. Existem requisitos de segurança para acessar recursos (assinaturas, Recursos Azure, gerenciamento de projetos, etc.), como VPN, autenticação de dois fatores (2FA), jump boxes, etc.?
3. Acesso aos dados:
    * Está localizado localmente ou já está no Azure?
    * Se estiver localizado localmente, podemos mover os dados necessários para o Azure sob a assinatura apropriada? Quem tem permissão para mover os dados?
    * O acesso aos dados é aprovado do ponto de vista legal e de conformidade?
4. Computação:
    * É necessário uma VPN para a equipe do projeto acessar esses nós de computação (Máquinas Virtuais, clusters Databricks, etc.) de seus PCs/Macs de trabalho?
    * Alguma restrição para acessar o sistema de dados de origem a partir desses nós de computação?
    * Se quisermos criar recursos de computação, quem tem permissão para fazê-lo?
5. Repositório de código-fonte:
    * Você tem alguma preferência quanto à localização do repositório de código-fonte?
6. Gerenciamento de backlog e planejamento de trabalho:
    * Você tem alguma preferência quanto ao gerenciamento de backlog e planejamento de trabalho, como Azure DevOps, Jira ou qualquer outra coisa?
    * Se for um sistema existente, são necessárias contas especiais/configurações de sistema para acessar?
7. Linguagem de programação:
    * Python/PySpark é a linguagem preferida?
    * Existem processos de aprovação interna para as bibliotecas Python/PySpark que desejamos usar neste projeto?

## Workshop de Exploração de Dados

Os principais objetivos dos workshops de exploração incluem o seguinte:

1. Compreender e documentar as características, localização e disponibilidade dos dados.
2. Qual é a ordem de grandeza dos dados atuais (por exemplo, GB, TB)? Isso é relevante?
3. Como a organização decide quando coletar dados adicionais ou adquirir dados externos? Existem exemplos disso?
4. Compreender a qualidade dos dados. Já existe uma estratégia de validação de dados em vigor?
5. Que dados foram usados até agora para analisar projetos recentes baseados em dados? O que se mostrou mais útil? O que não foi útil? Como isso foi avaliado?
6. Que dados internos adicionais podem fornecer insights úteis para a tomada de decisões baseada em dados para projetos propostos? Quais dados externos podem ser úteis?
7. Quais são as possíveis restrições ou desafios no acesso ou incorporação desses dados?
8. Como os dados foram coletados? Existem viéses óbvios devido à forma como os dados foram coletados?
9. Que mudanças na coleta de dados, codificação, integração, etc., ocorreram nos últimos 2 anos que podem afetar a interpretação ou disponibilidade dos dados coletados?
