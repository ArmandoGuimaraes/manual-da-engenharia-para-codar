# Fundamentos de Aprendizado de Máquina na ISE

Este guia documenta as práticas de Aprendizado de Máquina (ML) na ISE. A ISE trabalha com clientes no desenvolvimento de modelos de ML e na colocação deles em produção, com ênfase em boas práticas de engenharia e pesquisa ao longo do ciclo de vida do projeto.

## Objetivos

* Fornecer um conjunto de práticas de ML a serem seguidas em um projeto de ML.
* Fornecer clareza sobre o processo de ML e como ele se encaixa em um projeto de engenharia de software.
* Fornecer as melhores práticas para as diferentes etapas de um projeto de ML.

## Como usar esses fundamentos

* Se você está iniciando um novo projeto de ML, considere ler os [documentos de orientação geral](#orientacao-geral).
* Para aspectos específicos de um projeto de ML, consulte as diretrizes para as diferentes [fases do projeto de ML](#fases-do-projeto-de-ml).

## Fases do Projeto de ML

O diagrama abaixo mostra diferentes fases em um projeto de ML ideal. Devido a restrições práticas e requisitos, nem sempre é possível ter um projeto estruturado dessa maneira, no entanto, as melhores práticas devem ser seguidas para cada fase individual.

![Fluxo do Projeto](images/flow.png)

* **[Concepção](ml-problem-formulation-envisioning.md)**: Compreensão inicial do problema, metas e objetivos do cliente.
* **[Estudo de Viabilidade](ml-feasibility-study.md)**: Avaliar se o problema em questão é viável de ser resolvido satisfatoriamente usando ML com os dados disponíveis.
* **Marco do Modelo**: Há um modelo básico que está alcançando o desempenho mínimo requerido, tanto em termos de desempenho de ML quanto de desempenho do sistema. Usando o conhecimento adquirido até este marco, defina o escopo, objetivos, arquitetura de alto nível, definição de pronto e plano para todo o projeto.
* **[Experimentação de Modelo(s)](ml-experimentation.md)**: Ferramentas e melhores práticas para conduzir experimentos de modelo bem-sucedidos.
* **Operacionalização de Modelo(s)**: Lista de verificação de [preparação do modelo para produção](ml-model-checklist.md).

## Orientação Geral

* [Orientação do Processo de ML](ml-proposed-process.md)
* [Lista de Verificação de Fundamentos de ML](ml-fundamentals-checklist.md)
* [Exploração de Dados](ml-data-exploration.md)
* [Desenvolvimento Ágil de ML](ml-project-management.md)
* [Teste de Código de Ciência de Dados e ML Ops](ml-testing.md)
* [Perfil de Código de Aprendizado de Máquina e ML Ops](ml-profiling.md)
* [IA Responsável](responsible-ai.md)
* [Gerenciamento de Programa para Projetos de ML](ml-tpm-guidance.md)

## Referências

* [Operacionalização de Modelo](https://github.com/Microsoft/MLOps)
