# Projetos e Repositórios

Cada repositório de código-fonte deve incluir documentação específica para ele (por exemplo, em uma Wiki dentro do repositório), enquanto o projeto em si deve incluir documentação geral que seja comum a todos os seus repositórios associados (por exemplo, em uma Wiki dentro da ferramenta de gerenciamento de backlog).

## Documentação específica para um repositório

- Introdução
- Primeiros passos
  - Integração
  - Configuração: linguagem de programação, frameworks, plataformas, ferramentas, etc.
  - Ambiente de teste
  - Acordo de trabalho
  - Guia de contribuição
- Estrutura: pastas, projetos, etc.
- Como compilar, testar, construir, implantar a solução/cada projeto
  - Diferentes versões do sistema operacional
  - Linha de comando + editores/IDEs
- [Logs de Decisão de Design](../../design/design-reviews/decision-log/README.md)
  - [Registro de Decisões de Arquitetura (ADRs)](../../design/design-reviews/decision-log/README.md#architecture-decision-record-(ADR))
  - [Estudos de Análise](../../design/design-reviews/trade-studies/README.md)

Algumas seções na documentação do repositório podem apontar para a documentação do projeto (por exemplo, Integração, Acordo de Trabalho, Guia de Contribuição).

## Documentação comum a todos os repositórios

- Introdução
  - Projeto
  - Partes interessadas
  - Definições
  - Requisitos
- [Integração](../../developer-experience/onboarding-guide-template.md)
- Guia do repositório
  - Produção, Spikes
- [Acordos da equipe](../../agile-development/advanced-topics/team-agreements/README.md)
  - [Manifesto da Equipe](../../agile-development/advanced-topics/team-agreements/team-manifesto.md)
    - Resumo breve das expectativas em torno da forma técnica de trabalhar e da mentalidade apoiada na equipe.
    - Por exemplo, propriedade, respeito, colaboração, transparência.
  - [Acordo de Trabalho](../../agile-development/advanced-topics/team-agreements/working-agreements.md)
    - Como trabalhamos juntos como equipe e quais são nossas expectativas e princípios.
    - Por exemplo, comunicação, equilíbrio entre trabalho e vida, ritmo scrum, gerenciamento de backlog, gerenciamento de código.
  - [Definição de Pronto](../../agile-development/advanced-topics/team-agreements/definition-of-done.md)
    - Lista de tarefas que devem ser concluídas para encerrar uma história de usuário, um sprint ou uma etapa.
  - [Definição de Pronto para Estimar](../../agile-development/advanced-topics/team-agreements/definition-of-ready.md)
    - Quão completa uma história de usuário deve estar para ser selecionada como candidata para estimativa no planejamento do sprint.
- Guia de Contribuição
  - Estrutura do repositório
  - Documentos de design
  - [Estratégia de Nomenclatura de Branches](../../source-control/naming-branches.md)
  - [Estratégia de Histórico de Merge e Commit](../../source-control/merge-strategies.md)
  - [Pull Requests](./pull-requests.md)
  - [Processo de Revisão de Código](../../code-reviews/README.md)
  - [Lista de Verificação de Revisão de Código](../../code-reviews/process-guidance/reviewer-guidance.md)
    - [Listas de Verificação Específicas de Idioma](../../code-reviews/recipes/README.md)
- [Design do Projeto](../../design/design-reviews/README.md)
  - [Design de Alto Nível / Plano de Jogo](../../design/design-reviews/recipes/high-level-design-recipe.md)
  - [Revisão de Design de Marco / Épico](../../design/design-reviews/recipes/milestone-epic-design-review-recipe.md)
- [Receitas de Revisão de Design](../../design/design-reviews/README.md#Receitas)
  - [Modelo de Revisão de Design de Marco / Épico](../../design/design-reviews/recipes/milestone-epic-design-review-template.md)
  - [Modelo de Revisão de Design de Recurso / História](../../design/design-reviews/recipes/feature-story-design-review-template.md)
  - [Modelo de Revisão de Design de Tarefa](../../design/design-reviews/recipes/task-design-review-template.md)
  - [Modelo de Registro de Decisão](../../design/design-reviews/decision-log/doc/decision-log.md)
  - [Modelo de Registro de Decisão de Arquitetura (ADR)](../../design/design-reviews/decision-log/README.md#architecture-decision-record-(ADR)) ([Exemplo 1](../../design/design-reviews/decision-log/doc/adr/0001-record-architecture-decisions.md),
    [Exemplo 2](../../design/design-reviews/decision-log/doc/adr/0002-app-level-logging.md))
  - [Modelo de Estudo de Trade](../../design/design-reviews/trade-studies/template.md)
