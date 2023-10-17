# Sua Revisão de Design de Milestone/Épico (prefixe com DRAFT/WIP para indicar nível de completude)

> Por favor, consulte <https://microsoft.github.io/code-with-engineering-playbook/design/design-reviews/recipes/milestone-epic-design-review-recipe/> para orientações a serem seguidas ao utilizar este modelo.

* Milestone / Épico: [Nome](http://link-para-o-item-de-trabalho)
* Projeto / Engajamento: [Projeto ou Engajamento]
* Autores: [Autor1, Autor2, etc.]

## Visão Geral / Declaração do Problema

> Descreva o milestone/épico com um resumo de alto nível e uma declaração do problema. Considere incluir ou vincular a qualquer contexto adicional (por exemplo, Plano de Jogo ou documentos de Verificação) se for útil para o contexto histórico.

## Metas / No Escopo

> Liste alguns pontos principais das metas que este milestone/épico alcançará e que são mais relevantes para a discussão da revisão de design. Você pode incluir critérios de aceitação necessários para atender à [Definição de Pronto](../../../agile-development/advanced-topics/team-agreements/definition-of-done.md).

## Não Metas / Fora do Escopo

> Liste alguns pontos principais das não metas para esclarecer o trabalho que está fora do escopo da revisão de design deste milestone/épico.

## Design Proposto / Abordagem Sugerida

> Para otimizar o investimento de tempo, isso deve ser breve, pois é provável que os detalhes mudem à medida que o épico/milestone for decomposto em funcionalidades e histórias. O objetivo é transmitir a visão e complexidade em algo que possa ser compreendido em poucos minutos e possa ajudar a orientar uma discussão (seja de forma assíncrona por meio de comentários ou em uma reunião).

* Um parágrafo para descrever o design proposto / abordagem sugerida para este milestone/épico.
* Um diagrama (por exemplo, arquitetura, sequência, componente, implantação, etc.) ou trecho de pseudo-código para facilitar a discussão da abordagem.
* Liste algumas das abordagens alternativas que foram consideradas e inclua as breves principais **Vantagens e Desvantagens** usadas para justificar a decisão. Por exemplo:

| Prós                             | Contras                                  |
|----------------------------------|---------------------------------------|
| Simples de implementar            | Cria sistema de identidade secundário  |
| Padrão/código repetível          | A implantação requer credenciais de administrador |
|                                  |                                       |

## Tecnologia

> Liste brevemente as linguagens e plataformas que compõem o stack. Isso pode incluir qualquer coisa que seja necessária para entender a solução geral: SO, servidor web, camada de apresentação, camada de persistência, cache, eventos, etc.

## Requisitos Não Funcionais

* Quais são as principais preocupações de desempenho e escalabilidade para este milestone/épico?
* Existem objetivos específicos de latência, disponibilidade e RTO/RPO que devem ser atendidos?
* Existem gargalos específicos ou áreas potenciais de problema? Por exemplo, as operações são limitadas por CPU ou E/S (rede, disco)?
* Quão grandes são os conjuntos de dados e com que rapidez eles crescem?
* Qual é o padrão de uso esperado do serviço? Por exemplo, haverá picos e vales de uso intenso concorrente?
* Existem restrições de custo específicas? (por exemplo, $ por transação/dispositivo/usuario)

## Operacionalização

* Existem considerações específicas para a configuração de CI/CD do milestone/épico?
* Existe um processo (manual ou automatizado) para promover builds de ambientes mais baixos para ambientes mais altos?
* Este milestone/épico requer implantações sem tempo de inatividade e, se sim, como elas são alcançadas?
* Existem mecanismos para reverter uma implantação?
* Qual é o processo de monitoramento da funcionalidade fornecida por este milestone/épico?

## Dependências

* Este milestone/épico precisa ser sequenciado após outro épico atribuído à mesma equipe e por quê?
* O milestone/épico depende de outro trabalho que outra equipe precisa concluir?
* A equipe precisará esperar que esse trabalho seja concluído ou o trabalho pode prosseguir em paralelo?

## Riscos e Mitigações

* A equipe precisa de assistência de especialistas no assunto?
* Quais são as preocupações de segurança e privacidade deste milestone/épico?
* Todas as informações sensíveis e segredos são tratados de forma segura e protegida?

## Perguntas em Aberto

> Inclua quaisquer perguntas em aberto e preocupações.

## Referências Adicionais

> Inclua quaisquer referências adicionais, incluindo links para itens de trabalho ou outros documentos.
