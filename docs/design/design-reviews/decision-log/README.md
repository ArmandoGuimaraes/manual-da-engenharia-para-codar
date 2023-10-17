# Registro de Decisões de Design

 Nem todos os requisitos podem ser capturados no início de um projeto ágil durante uma ou mais sessões de design. O design inicial da arquitetura pode evoluir ou mudar durante o projeto, especialmente se houver várias escolhas tecnológicas possíveis. Acompanhar essas mudanças em um documento extenso geralmente não é o ideal, pois você pode perder a visão geral das mudanças de design feitas em qual ponto no tempo. Ter que percorrer um documento grande para encontrar um conteúdo específico leva tempo e, na maioria dos casos, as consequências de uma decisão não são documentadas.

## Por que é importante rastrear decisões de design

 Rastrear uma decisão de design de arquitetura pode ter muitas vantagens:

- Os desenvolvedores e partes interessadas do projeto podem ver o registro de decisões e acompanhar as alterações, mesmo quando a composição da equipe muda ao longo do tempo.
- O registro é mantido atualizado.
- O contexto de uma decisão, incluindo as consequências para a equipe, é documentado junto com a decisão.
- É mais fácil encontrar a decisão de design em um registro do que ter que ler um documento grande.

## Qual é o formato recomendado para rastrear decisões

 Além de incorporar uma decisão de design como uma atualização na documentação de design geral do projeto, as decisões podem ser rastreadas como [Registros de Decisões de Arquitetura](http://thinkrelevance.com/blog/2011/11/15/documenting-architecture-decisions) conforme Michael Nygard propôs em seu blog.

O esforço investido em revisões e discussões de design pode ser diferente ao longo do projeto. Às vezes, decisões são tomadas rapidamente sem a necessidade de fazer uma comparação detalhada entre tecnologias concorrentes. Em alguns casos, é necessário realizar um estudo mais elaborado das vantagens e desvantagens, conforme descrito na documentação dos [Estudos de Troca](../trade-studies/README.md). Em outros casos, pode ser útil realizar [Spikes de Viabilidade de Engenharia](../recipes/engineering-feasibility-spikes.md). Um ADR pode incorporar cada uma dessas abordagens diferentes.

### Registro de Decisão de Arquitetura (ADR)

 Um registro de decisão de arquitetura tem a estrutura

- **[Número Crescente]. [Título da decisão]**

    *O título deve fornecer ao leitor informações sobre o que foi decidido.*

    Exemplo:

    > *001. Registro de log no nível do aplicativo com Serilog e Application Insights*

- **Data:**

    *A data em que a decisão foi tomada.*

- **Status:**
    Proposto/Aceito/Descontinuado/Substituído

    *Um design proposto pode ser revisado pela equipe de desenvolvimento antes de ser aceito. Uma decisão anterior pode ser substituída por uma nova ou o registro ADR pode ser marcado como descontinuado caso não seja mais válido.*

- **Contexto:**

    *O texto deve fornecer ao leitor uma compreensão do problema ou, como Michael Nygard coloca, uma descrição [objetiva] e neutra em valor das forças em jogo.*

    Exemplo:

    > *Devido ao design de microsserviços da plataforma, precisamos garantir a consistência do registro em cada serviço para que o rastreamento de uso, desempenho, erros etc. possa ser realizado de ponta a ponta. Deve ser usado um único framework de registro/monitoramento, onde possível, para alcançar isso, permitindo ao mesmo tempo a flexibilidade para integração/exportação em outras ferramentas em uma etapa posterior. Os desenvolvedores devem ter uma interface simples para registrar mensagens e métricas.*

    *Se a equipe de desenvolvimento adotou uma abordagem baseada em dados para apoiar a decisão, ou seja, um estudo que avalia as escolhas potenciais em relação a um conjunto de critérios objetivos seguindo a orientação em [Estudos de Troca](../trade-studies/README.md), o estudo deve ser mencionado nesta seção.*

- **Decisão:**

    *A decisão tomada, deve começar com 'Vamos...' ou 'Concordamos em...'.*

    Exemplo:

    > *Concordamos em utilizar o Serilog como o framework de registro do Dotnet de escolha no nível do aplicativo, com integração ao Log Analytics e Application Insights para análise.*

- **Consequências:**

   

 *O contexto resultante após a aplicação da decisão.*

    Exemplo:

    > *A amostragem precisará ser configurada no Application Insights para que ela não fique excessivamente cara ao receber milhões de mensagens, mas também não impeça a captura de informações essenciais. A equipe precisará registrar apenas o que foi acordado como essencial para monitoramento durante as revisões de design, a fim de reduzir o ruído e os níveis desnecessários de amostragem.*

### Onde armazenar ADRs

 ADRs podem ser armazenados e rastreados em qualquer sistema de controle de versão, como o Git. Como prática recomendada, ADRs podem ser adicionados como pull requests no status *proposto* para serem discutidos pela equipe até que sejam atualizados para *aceitos* e mesclados com o ramo principal. Eles geralmente são armazenados em uma estrutura de pasta *doc/adr* ou *doc/arch*. Além disso, pode ser útil rastrear ADRs em um `decision-log.md` para fornecer metadados úteis em um formato óbvio.

#### Registros de Decisão

Um registro de decisão é um arquivo Markdown que contém uma tabela que fornece resumos executivos das decisões contidas nos ADRs, bem como alguns outros metadados. Você pode ver um modelo de tabela em [`doc/decision-log.md`](doc/decision-log.md).

### Quando rastrear ADRs

 As decisões de design de arquitetura geralmente são rastreadas sempre que decisões significativas são tomadas que afetam a estrutura e as características da solução ou framework que estamos construindo. ADRs também podem ser usados para documentar resultados de spikes ao avaliar diferentes escolhas tecnológicas.

## Exemplos de ADRs

 O primeiro ADR poderia ser a decisão de usar ADRs para rastrear decisões de design,

- [0001-registrar-decisoes-arquitetura.md](doc/adr/0001-registrar-decisoes-arquitetura.md),

seguido por decisões reais no engajamento, como no exemplo usado acima,

- [0002-registro-log-nivel-aplicativo.md](doc/adr/0002-registro-log-nivel-aplicativo.md).
