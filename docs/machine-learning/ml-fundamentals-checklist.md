# Checklist de Fundamentos de ML

Este checklist ajuda a garantir que nossos projetos de ML atendam aos nossos Fundamentos de ML. Os itens abaixo não são sequenciais, mas sim organizados por diferentes partes de um projeto de ML.

## Qualidade e Governança dos Dados

- [ ] Há acesso aos dados.
- [ ] Existem rótulos para o conjunto de dados de interesse.
- [ ] Avaliação da qualidade dos dados.
- [ ] É possível rastrear a linhagem dos dados.
- [ ] Compreensão de onde os dados estão vindo e de quais políticas estão relacionadas ao acesso aos dados.
- [ ] Coleta de requisitos de segurança e conformidade.

## Estudo de Viabilidade

- [ ] Um estudo de viabilidade foi realizado para avaliar se os dados suportam as tarefas propostas.
- [ ] Foi realizada uma análise rigorosa dos dados exploratórios (incluindo análise da distribuição dos dados).
- [ ] Hipóteses foram testadas, produzindo evidências suficientes para apoiar ou rejeitar que uma abordagem de ML seja viável para resolver o problema.
- [ ] Foi realizada uma estimativa de ROI (Retorno sobre o Investimento) e análise de risco para o projeto.
- [ ] Os resultados/saídas de ML podem ser integrados ao sistema de produção.
- [ ] As recomendações sobre como proceder foram documentadas.

## Avaliação e Métricas

- [ ] Definição clara de como o desempenho será medido.
- [ ] As métricas de avaliação estão de alguma forma conectadas aos critérios de sucesso.
- [ ] As métricas podem ser calculadas com os conjuntos de dados disponíveis.
- [ ] O fluxo de avaliação pode ser aplicado a todas as versões do modelo.
- [ ] O código de avaliação é testado unitariamente e revisado por todos os membros da equipe.
- [ ] O fluxo de avaliação facilita análises adicionais de resultados e erros.

## Baseline do Modelo

- [ ] Existe um modelo de baseline bem definido e seu desempenho é calculado. ([Mais detalhes sobre baselines bem definidos](ml-model-checklist.md#is-there-a-well-defined-baseline-is-the-model-performing-better-than-the-baseline))
- [ ] O desempenho de outros modelos de ML pode ser comparado com o modelo de baseline.

## Configuração de Experimentação

- [ ] Conjunto de dados de treinamento/teste bem definido com rótulos.
- [ ] Experimentos reprodutíveis e registrados em um ambiente acessível por todos os cientistas de dados para iterações rápidas.
- [ ] Experimentos/hipóteses definidas para testar.
- [ ] Os resultados dos experimentos são documentados.
- [ ] Hiperparâmetros do modelo são ajustados sistematicamente.
- [ ] São usadas as mesmas métricas de avaliação de desempenho e conjuntos de dados consistentes ao comparar modelos candidatos.

## Produção

- [ ] Revisão da [lista de verificação de prontidão do modelo](ml-model-checklist.md).
- [ ] Foram realizadas revisões do modelo (cobrindo depuração do modelo, revisões das abordagens de treinamento e avaliação, desempenho do modelo).
- [ ] Pipeline de dados para inferência, incluindo testes de ponta a ponta.
- [ ] Requisitos de SLAs (Acordos de Nível de Serviço) para os modelos são coletados e documentados.
- [ ] Monitoramento dos feeds de dados e saída do modelo.
- [ ] Garantir que um esquema consistente seja usado em todo o sistema, com entrada/saída esperada definida para cada componente dos pipelines (processamento de dados, bem como modelos).
- [ ] Revisão de [IA Responsável](responsible-ai.md).
