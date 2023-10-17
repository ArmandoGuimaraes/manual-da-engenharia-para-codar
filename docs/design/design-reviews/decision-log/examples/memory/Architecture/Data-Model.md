# Modelo de Dados

## Tabela de Conteúdo

- [Vértices e Arestas do Grafo](#vértices-e-arestas-do-grafo)
- [Propriedades do Grafo](#propriedades-do-grafo)
- [Descrições dos Vértices](#descrições-dos-vértices)
- [Exemplo Completo em JSON de um Papel](#exemplo-completo-em-json-de-um-papel)
- [Modelo de Dados da API](#modelo-de-dados-da-api)

## Modelo de Grafo

## Vértices e Arestas do Grafo

O conjunto de vértices (entidades) e arestas (relacionamentos) do modelo de grafo

| Vértice (Fonte) | Tipo de Aresta   | Tipo de Relacionamento | Vértice (Destino) | Notas                                                                                                | Obrigatório |
|-----------------|-------------------|-------------------------|-------------------|------------------------------------------------------------------------------------------------------|-------------|
| Profissão       | _Aplica_          | 1: muitos               | Disciplina        | Nível mais alto de categorização                                                                     | \*          |
| Disciplina      | _Define_          | 1: muitos               | Papel             | Grupos de papéis relacionados dentro de uma profissão                                                  | \*          |
|                 | _AplicadoPor_     | 1: 1                    | Profissão         |                                                                                                      | 1           |
| Papel           | _Requer_          | 1: muitos               | Responsabilidade  | Papel individual mapeado para um funcionário                                                          | 1+          |
|                 | _Requer_          | 1: muitos               | Competência       |                                                                                                      | 1+          |
|                 | _RequeridoPor_    | 1: 1                    | Disciplina        |                                                                                                      | 1           |
|                 | _Sucessor_        | 1: 1                    | Papel             | Suporta a progressão na carreira entre papéis                                                          | 1           |
|                 | _Predecessor_     | 1: 1                    | Papel             | Suporta a progressão na carreira entre papéis                                                          | 1           |
|                 | _AtribuídoPara_   | 1: muitos               | Perfil de Usuário  |                                                                                                      | \*          |
| Responsabilidade| _Espera_          | 1: muitos               | Resultado-chave    | Um grupo de resultados esperados e resultados-chave para funcionários dentro de um papel              | 1+          |
|                 | _EsperadoPor_     | 1: 1                    | Papel             |                                                                                                      | 1           |
| Competência     | _Descreve_        | 1: muitos               | Comportamento      | Um conjunto de comportamentos que contribuem para o sucesso                                            | 1+          |
|                 | _DescritoPor_     | 1: 1                    | Papel             |                                                                                                      | 1           |
| Resultado-chave | _EsperadoPor_     | 1: 1                    | Responsabilidade  | O resultado esperado da realização de uma responsabilidade                                              | 1           |
| Comportamento   | _ContribuiPara_   | 1: 1                    | Competência       | A maneira como alguém age ou se conduz                                                                | 1           |
| Perfil de Usuário| _Cumpre_         | muitos: 1               | Papel             |                                                                                                      | 1+          |
|                 | _ÉAutorDe_        | 1: muitos               | Entrada           |                                                                                                      | \*          |
|                 | _Lê_              | muitos: muitos          | Entrada           |                                                                                                      | \*          |
| Entrada         | _CompartilhadoCom_| muitos: muitos          | Perfil de Usuário  | A lógica de negócios deve adicionar o gerente a esta lista por padrão. Esses usuários só devem ter acesso de leitura. | \*          |
|                 | _Demonstra_       | muitos: muitos          | Competência       |                                                                                                      | \*          |
|                 | _Demonstra_       | muitos: muitos          | Comportamento      |                                                                                                      | \*          |
|                 | _Demonstra_       | muitos: muitos          | Responsabilidade  |                                                                                                      | \*          |
|                 | _Demonstra_       | muitos: muitos          | Resultado-chave    |                                                                                                      | \*          |
|                 | _AutorizadoPor_   | muitos: 1               | Perfil de Usuário  |                                                                                                      | 1+          |
|                 | _DiscutidoPor_    | 1: muitos               | Comentário        |                                                                                                      | \*          |
|                 | _ReferenciadoPor_ | muitos: muitos          | Artefato          |                                                                                                      | \*          |
| Competência     | _DemonstradoPor_  | muitos: muitos          | Entrada           |                                                                                                      | \*          |
| Comportamento   | _DemonstradoPor_  | muitos: muitos          | Entrada           |                                                                                                      | \*          |
| Responsabilidade| _DemonstradoPor_  | muitos: muitos          | Entrada           |                                                                                                      | \*          |
| Resultado-chave  | _DemonstradoPor_  | muitos: muitos          | Entrada           |                                                                                                      | \*          |
| Comentário      | _Discute_         | muitos: 1               | Entrada           |                                                                                                      | \*          |
| Artefato        | _ReferenciadoPor_ | muitos: muitos          | Entrada           |                                                                                                      | 1+          |

## Propriedades do Grafo

O conjunto completo de propriedades de dados disponíveis em cada vértice e aresta

| Vértice/Aresta   | Propriedade     | Tipo de Dados | Observações                                       | Obrigatório |
|------------------|-----------------|---------------|---------------------------------------------------|------------|
| **(Qualquer)**   | ID              | guid          |                                                   | 1          |
| Profissão        | Título           | String        |                                                   | 1          |
|                  | Descrição       | String        |                                                   | 0          |
| Disciplina       | Título           | String        |                                                   | 1          |
|                  | Descrição       | String        |                                                   | 0          |
| Papel            | Título           | String        |                                                   | 1          |
|                  | Descrição       | String        |                                                   | 0          |
|                  | Nível Band      | String        | SDE, SDE II, Senior, etc                          | 1

          |
| Responsabilidade | Título           | String        |                                                   | 1          |
|                  | Descrição       | String        |                                                   | 0          |
| Competência      | Título           | String        |                                                   | 1          |
|                  | Descrição       | String        |                                                   | 0          |
| Resultado-chave  | Descrição       | String        |                                                   | 1          |
| Comportamento    | Descrição       | String        |                                                   | 1          |
| Perfil de Usuário| Seleção de Tema  | string        | Existem apenas 2: escuro, claro                   | 1          |
|                  | PersonaId       | guid[]        | Existem apenas 2: Usuário, Admin                 | 1+         |
|                  | UserId          | guid          | Aponta para objeto AAD                            | 1          |
|                  | DeploymentRing  | string[]      | É usado para implantar novas versões             | 1          |
|                  | Projeto         | string[]      | lista de projetos criados pelo usuário           | \*         |
| Entrada          | Título           | string        |                                                   | 1          |
|                  | DataCriada      | date          |                                                   | 1          |
|                  | ProntoParaCompartilhar | boolean | falso se rascunho                               | 1          |
|                  | ÁreaDeImpacto   | string[]      | 3 opções: self, contribuir para outros, alavancar outros | \*    |
| Comentário       | Dados           | string        |                                                   | 1          |
|                  | DataCriada      | date          |                                                   | 1          |
| Artefato         | Dados           | string        |                                                   | 1          |
|                  | DataCriada      | date          |                                                   | 1          |
|                  | TipoDeArtefato   | string        | descreve o tipo de artefato: markdown, link blob  | 1          |

## Descrições dos Vértices

### Profissão

Nível mais alto de categorização

```json
{
    "título": "Engenharia de Software",
    "descrição": "Descrição da profissão",
    "disciplinas": []
}
```

### Disciplina

Grupos de papéis relacionados dentro de uma profissão

```json
{
  "título": "Engenharia de Confiabilidade de Sites",
  "descrição": "Descrição da disciplina",
  "papéis": []
}
```

### Papel

Papel individual mapeado para um funcionário

```json
{
  "título": "Engenheiro de Confiabilidade de Sites IC2",
  "descrição": "Descrição detalhada do papel",
  "responsabilidades": [],
  "competências": []
}
```

### Responsabilidade

Um grupo de resultados esperados e resultados-chave para funcionários dentro de um papel

```json
{
  "título": "Conhecimento Técnico e Especialização em Domínio",
  "resultados": []
}
```

### Competência

Um conjunto de comportamentos que contribuem para o sucesso

```json
{
  "título": "Adaptabilidade",
  "comportamentos": []
}
```

### Resultado-chave

O resultado esperado da realização de uma responsabilidade

```json
{
  "descrição": "Desenvolve uma compreensão fundamental do design de sistemas distribuídos..."
}
```

### Comportamento

A maneira como alguém age ou se conduz

```json
{
  "descrição": "Busca ativamente informações e testa suposições."
}
```

### Perfil de Usuário

O objeto de usuário refere-se a quem uma pessoa é.
Não armazenamos os nossos próprios, preferimos usar Azure OIDs.

### Perfil de Usuário

O perfil de usuário contém configurações específicas do usuário e arestas específicas para Memory.

### Persona

Um usuário pode ter várias personas.

### Entrada

O mesmo objeto de entrada pode conter muitos tipos de dados, e nesta fase do projeto, decidimos que não armazenaremos dados externos, portanto, cabe ao usuário fornecer um link para os dados para que o leitor possa clicar e ser redirecionado para uma nova aba.

> **Nota:** Isso significa que no aplicativo da web, precisamos garantir que os links sejam abertos em novas abas.

### Projeto

Projetos são apenas campos de texto para representar como o usuário deseja agrupar suas entradas.

### Área de Impacto

Isso se refere às 3 áreas de impacto no diagrama estilo Venn na ferramenta de RH.
As opções são: próprio, contribuindo para o impacto dos outros, construindo sobre o trabalho dos outros.

### Comentário

Um comentário é essencialmente um pedaço de texto.
No entanto, qualquer pessoa com quem uma entrada seja compartilhada pode adicionar comentários a essa entrada.

### Artefato

O objeto de artefato contém os dados relevantes como markdown ou um link para os dados relevantes.

## Exemplo Completo em JSON de um Papel

```json
{
  "id": "abc123",
  "título": "Engenheiro de Confiabilidade de Sites IC2",
  "descrição": "Descrição detalhada do papel",
  "responsabilidades": [
    {
      "id": "abc123",
      "título": "Conhecimento Técnico e Especialização em Domínio",
      "resultados": [
        {
          "descrição": "Desenvolve uma compreensão fundamental do design de sistemas distribuídos..."
        },
        {
          "descrição": "Desenvolve uma compreensão do código, recursos e operações de produtos específicos..."
        }
      ]
    },
    {
      "id": "abc123",
      "título": "Contribuições para Desenvolvimento e Design",
      "resultados": [
        {
          "descrição": "Desenvolve e testa mudanças básicas para otimizar o código..."
        },
        {
          "descrição": "Dá suporte a engajamentos contínuos com equipes de engenharia de produtos..."
        }
      ]
    }
  ],
  "competências": [
    {
      "id": "abc123",
      "título": "Adaptabilidade",
      "comportamentos": [
        { "descrição": "Busca ativamente informações e testa suposições." },
        {
          "descrição": "Muda sua abordagem em resposta às demandas de uma situação em mudança."
        }
      ]
    },
    {
      "id": "abc123",
      "título": "Colaboração

",
      "comportamentos": [
        {
          "descrição": "Remove barreiras trabalhando com outras pessoas em torno de uma necessidade ou benefício compartilhado do cliente."
        },
        {
          "descrição": "Incorpora perspectivas diversas para abordar completamente questões comerciais complexas."
        }
      ]
    }
  ]
}
```

## Modelo de Dados da API

Como não há vértices internos ou vértices que precisam ser ocultos dos consumidores da API, a API exporá um mapeamento 1:1 do modelo de dados atual para consumo.
Isso está sujeito a alterações se nosso modelo de dados se tornar muito complexo para os usuários finais.
