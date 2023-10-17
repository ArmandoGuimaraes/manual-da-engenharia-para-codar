# Tipos de Diagramas

Criar e manter diagramas é um desafio para qualquer equipe. Motivos comuns para esses desafios incluem:

- Não aproveitar ferramentas para ajudar na geração de diagramas.
- Incerteza sobre o que incluir em um diagrama e quando criá-lo.

Superar esses desafios e usar efetivamente diagramas de design pode ampliar a capacidade de uma equipe de executar ao longo de todo o Ciclo de Desenvolvimento de Software, desde a fase de design ao propor vários designs até usá-los como documentação na fase de manutenção.

Esta seção compartilhará ferramentas de exemplo para geração de diagramas, fornecerá uma visão geral de alto nível dos diferentes tipos de diagramas e dará exemplos de alguns desses tipos.

Existem duas classes principais de diagramas:

- Estruturais
- Comportamentais

Dentro de cada uma dessas classes, existem muitos tipos de diagramas, cada um destinado a transmitir tipos específicos de informações. Quando diferentes tipos de diagramas são usados efetivamente em uma solução, sistema ou repositório, é possível fornecer um design coeso e incrementalmente detalhado.

## Diagramas de Design de Exemplo

Esta seção contém material educacional e exemplos dos seguintes diagramas de design:

- [Diagramas de Classes](DesignDiagramsTemplates/classDiagrams.md) - Úteis para documentar o design estrutural de um código, a relação entre classes e seus métodos correspondentes.
- [Diagramas de Componentes](DesignDiagramsTemplates/componentDiagrams.md) - Úteis para documentar uma visão geral estrutural de alto nível de todos os componentes e seus "pontos de contato" diretos com outros componentes.
- [Diagramas de Sequência](DesignDiagramsTemplates/sequenceDiagrams.md) - Úteis para documentar uma visão geral do comportamento do sistema, capturando os vários "casos de uso" ou "ações" que acionam o sistema a realizar alguma lógica de negócios.
- [Diagrama de Implantação](DesignDiagramsTemplates/deploymentDiagrams.md) - Útil para documentar os ambientes de rede e hospedagem nos quais o sistema irá operar.

## Recursos Suplementares

Cada um dos tipos de diagramas acima fornecerá recursos específicos relacionados ao seu tipo. Abaixo estão os recursos genéricos:

- [Diagramas UML Estruturais vs. Comportamentais do Visual Paradigm](https://www.visual-paradigm.com/cn/guide/uml-unified-modeling-language/uml-)
- [PlantUML](https://marketplace.visualstudio.com/items?itemName=jebbs.plantuml) - requer um gerador de código para a sintaxe PlantUML para gerar diagramas
  - [C# para PlantUML](https://marketplace.visualstudio.com/items?itemName=pierre3.csharp-to-plantuml)
  - [Desenhar manualmente](https://towardsdatascience.com/drawing-a-uml-diagram-in-the-vs-code-53c2e67deffe)
