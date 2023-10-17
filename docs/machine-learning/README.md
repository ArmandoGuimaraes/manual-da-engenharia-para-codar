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

* **[Concepção](ml-formulacao-problema-concepcao.md)**: Compreensão inicial do problema, metas e objetivos do cliente.
* **[Estudo de Viabilidade](ml-estudo-viabilidade.md)**: Avaliar se o problema em questão é viável de ser resolvido satisfatoriamente usando ML com os dados disponíveis.
* **Marco do Modelo**: Há um modelo básico que está alcançando o desempenho mínimo requerido, tanto em termos de desempenho de ML quanto de desempenho do sistema. Usando o conhecimento adquirido até este marco, defina o escopo, objetivos, arquitetura de alto nível, definição de pronto e plano para todo o projeto.
* **[Experimentação de Modelo(s)](ml-experimentacao.md)**: Ferramentas e melhores práticas para conduzir experimentos de modelo bem-sucedidos.
* **Operacionalização de Modelo(s)**: Lista de verificação de [preparação do modelo para produção](ml-checklist-modelo.md).

## Orientação Geral

* [Orientação do Processo de ML](ml-processo-proposto.md)
* [Lista de Verificação de Fundamentos de ML](ml-lista-verificacao-fundamentos.md)
* [Exploração de Dados](ml-exploracao-dados.md)
* [Desenvolvimento Ágil de ML](ml-gerenciamento-projeto.md)
* [Teste de Código de Ciência de Dados e ML Ops](ml-teste.md)
* [Perfil de Código de Aprendizado de Máquina e ML Ops](ml-perfil-codigo.md)
* [IA Responsável](ia-responsavel.md)
* [Gerenciamento de Programa para Projetos de ML](ml-orientacao-tpm.md)

## Referências

* [Operacionalização de Modelo](https://github.com/Microsoft/MLOps)
