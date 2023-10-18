# DevSecOps

## O conceito de DevSecOps

DevSecOps ou segurança DevOps trata da introdução da segurança mais cedo no ciclo de vida do desenvolvimento de aplicativos (também conhecido como shift-left), minimizando assim o impacto das vulnerabilidades e aproximando a segurança da equipe de desenvolvimento.

## Por que

Ao adotar a mentalidade shift-left, o DevSecOps incentiva as organizações a reduzir a lacuna que muitas vezes existe entre as equipes de desenvolvimento e segurança, a ponto de muitos dos processos de segurança serem automatizados e tratados efetivamente pela equipe de desenvolvimento.

## Práticas do DevSecOps

Esta seção aborda diferentes ferramentas, estruturas e recursos que permitem a introdução das melhores práticas do DevSecOps em seu projeto nas fases iniciais do desenvolvimento.
Tópicos abordados:

1. [Verificação de Credenciais](./secret-management/credential_scanning.md) - inspecionar automaticamente um projeto para garantir que nenhuma informação confidencial esteja incluída no código-fonte do projeto.
1. [Rotação de Segredos](./secret-management/secrets_rotation.md) - processo automatizado pelo qual o segredo usado pelo aplicativo é atualizado e substituído por um novo segredo.
1. [Análise de Código Estático](./static-code-analysis/static_code_analysis.md) - analisar o código-fonte ou versões compiladas do código para ajudar a encontrar falhas de segurança.
1. [Teste de Penetração](./penetration-testing/penetration_testing.md) - um ataque simulado contra seu aplicativo para verificar a existência de vulnerabilidades exploráveis.
1. [Verificação de Dependências de Contêiner](./dependency-container-scanning/dependency_container_scanning.md) - procurar vulnerabilidades nos sistemas operacionais de contêiner, pacotes de linguagem e dependências de aplicativos.
