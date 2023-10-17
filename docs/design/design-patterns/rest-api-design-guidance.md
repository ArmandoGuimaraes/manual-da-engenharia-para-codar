# Orientações de Design de REST API

## Objetivos

* Elevar as [diretrizes de design de REST API publicadas pela Microsoft](https://github.com/microsoft/api-guidelines).
* Destacar decisões de design comuns e fatores a serem considerados ao projetar.
* Fornecer recursos adicionais para informar o design de API em áreas não abordadas diretamente pelas diretrizes da Microsoft.

## Decisões Comuns de Design de API

As [diretrizes da Microsoft para REST API](https://github.com/microsoft/api-guidelines) fornecem orientações de design que abrangem uma variedade de casos de uso. As seguintes seções são um bom ponto de partida, pois são considerações provavelmente necessárias em qualquer design de REST API:

* [Estrutura de URL](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#71-url-structure)
* [Métodos HTTP](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods)
* [Códigos de Status HTTP](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#711-http-status-codes)
* [Coleções](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#9-collections)
* [Padronizações JSON](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#11-json-standardizations)
* [Versionamento](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#12-versioning)
* [Nomenclatura](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#17-naming-guidelines)

## Criando Contratos de API

À medida que diferentes equipes de desenvolvimento expõem APIs para acessar vários serviços baseados em REST, é importante ter um contrato de API para compartilhar entre os produtores e consumidores de APIs. O formato [Open API](https://www.openapis.org/) é um dos formatos de descrição de API mais populares. Este documento Open API pode ser produzido de duas maneiras:

* Abordagem Design-First - A equipe começa a desenvolver APIs descrevendo primeiro os designs da API como um documento Open API e posteriormente gera o código de boilerplate do lado do servidor com a ajuda deste documento.
* Abordagem Code-First - A equipe começa escrevendo o código da interface da API do lado do servidor, como controladores, DTOs, etc., e posteriormente gera um documento Open API a partir dele.

### Abordagem Design-First

Uma abordagem Design-First significa que as APIs são tratadas como "cidadãos de primeira classe" e tudo em torno de um projeto gira em torno da ideia de que, no final, essas APIs serão consumidas por clientes. Com base nos requisitos de negócios, a equipe de desenvolvimento de API começa descrevendo primeiro os designs da API como um documento Open API e colabora com as partes interessadas para obter feedback.

Essa abordagem é bastante útil se um projeto envolver o desenvolvimento de um conjunto de APIs externamente expostas que serão consumidas por parceiros. Nessa abordagem, concordamos primeiro com um contrato de API (documento Open API), criando expectativas claras tanto para o produtor quanto para o consumidor da API, para que ambas as equipes possam começar a trabalhar em paralelo de acordo com o design da API pré-acordado.

Principais benefícios desta abordagem:

* Feedback precoce sobre o design da API.
* Expectativas claramente estabelecidas tanto para o consumidor quanto para o produtor, já que ambos concordaram com um contrato de API.
* As equipes de desenvolvimento podem trabalhar em paralelo.
* A equipe de testes pode usar os contratos de API para escrever testes antecipados, mesmo antes que a lógica de negócios esteja implementada. Ao examinar modelos, caminhos, atributos e outros aspectos da API, os testadores podem fornecer seu feedback, o que pode ser muito valioso.
* Durante um ciclo de desenvolvimento ágil, as definições de API não são impactadas por alterações incrementais.
* O design da API não é influenciado por limitações de implementação real e estrutura de código.
* O código de boilerplate do lado do servidor, como controladores, DTOs, etc., pode ser gerado automaticamente a partir dos contratos de API.
* Pode melhorar a colaboração entre as equipes de produtor e consumidor da API.

Planejando um Desenvolvimento Design-First:

1. Identifique casos de uso e serviços-chave que a API deve oferecer.
2. Identifique as principais partes interessadas da API e tente incluí-las na fase de design da API para obter feedback contínuo.
3. Escreva definições de contrato de API.
4. Mantenha um estilo consistente para códigos de status da API, versionamento, respostas de erro, etc.
5. Incentive revisões por pares por meio de solicitações de pull.
6. Gere o código de boilerplate do lado do servidor e os SDKs do cliente a partir das definições de contrato de API.

Pontos importantes a considerar:

* Se os requisitos da API mudarem com frequência durante a fase inicial de desenvolvimento, a abordagem Design-First pode não ser adequada, pois isso introduzirá uma sobrecarga adicional, exigindo atualizações e manutenção repetidas das definições de contrato da API.
* Pode ser útil primeiro experimentar o gerador de código específico da plataforma e avaliar quanto trabalho adicional será necessário para atender aos requisitos e diretrizes de codificação do projeto, pois é possível que um gerador de código específico da plataforma não consiga gerar uma implementação flexível e mantida do código real. Por exemplo, se o seu framework web exigir que anotações estejam presentes em suas classes de controlador (por exemplo, para versionamento de API ou autenticação), certifique-se de que a ferramenta de geração de código que você usa as suporta totalmente.
* [Microsoft TypeSpec](https://github.com/Microsoft/typespec) é uma ferramenta valiosa para desenvolvedores que trabalham com APIs complexas. Fornecendo padrões reutilizáveis, ele pode simplificar o desenvolvimento de APIs e promover as melhores práticas. Criamos alguns [exemplos de como aplicar uma abordagem de desenvolvimento Design-First em um pipeline de CI/CD do GitHub](https://github.com/cse-labs/typespec-workflow-samples/) para ajudar a acelerar sua adoção em um Desenvolvimento Design-First.

### Abordagem Code-First

Uma abordagem Code-First

 significa que as equipes de desenvolvimento primeiro implementam o código da interface da API do lado do servidor, como controladores, DTOs, etc., e depois geram as definições de contrato de API a partir dele. Nos tempos atuais, essa abordagem é mais amplamente popular na comunidade de desenvolvedores do que a Abordagem Design-First.

Essa abordagem tem a vantagem de permitir que a equipe implemente rapidamente as APIs e também fornece a flexibilidade de reagir muito rapidamente a quaisquer mudanças inesperadas nos requisitos da API.

Principais benefícios desta abordagem:

* Desenvolvimento rápido de APIs, pois a equipe de desenvolvimento pode começar a implementar as APIs muito mais rapidamente após entender os requisitos e casos de uso-chave.
* A equipe de desenvolvimento tem melhor controle e flexibilidade para implementar interfaces de API do lado do servidor da maneira que melhor se adapte à estrutura do projeto.
* Mais popular entre as equipes de desenvolvimento, tornando mais fácil obter consenso sobre tópicos relacionados e também possui mais exemplos de código prontos para uso disponíveis em vários blogs ou fóruns de desenvolvedores sobre como gerar definições Open API a partir do código real.

Pontos importantes a considerar:

* Uma definição Open API gerada pode ficar desatualizada, portanto, é importante ter verificações automatizadas para evitar isso, caso contrário, os SDKs de cliente gerados estarão desatualizados e podem causar problemas para os consumidores da API.
* Com o desenvolvimento ágil, é difícil garantir que as definições incorporadas no código em tempo de execução permaneçam estáveis, especialmente em rodadas de refatoração e ao atender várias versões concorrentes da API.
* Pode ser útil gerar regularmente a definição Open API e armazená-la em um sistema de controle de versão; caso contrário, gerar a definição Open API em tempo de execução pode tornar mais complexo em cenários em que essa definição é necessária durante o desenvolvimento/tempo de CI.

## Como Interpretar e Aplicar as Diretrizes

O documento de diretrizes de API inclui uma seção sobre [como aplicar as diretrizes](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#4-interpreting-the-guidelines), dependendo se a API é nova ou existente. Em particular, ao trabalhar em um ecossistema de API existente, certifique-se de alinhar com as partes interessadas uma definição do que constitui uma [mudança quebra](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#123-definition-of-a-breaking-change) para entender o impacto da implementação de determinadas melhores práticas.

> Não recomendamos fazer uma mudança quebra em um serviço que antecede estas diretrizes simplesmente para cumprir.

## Recursos Adicionais

* [Lista de Leitura Recomendada da Microsoft para REST APIs](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#31-recommended-reading)
* [Documentação - Orientação - REST APIs](https://microsoft.github.io/code-with-engineering-playbook/documentation/guidance/rest-apis/)
* [Definições detalhadas de códigos de status HTTP](https://www.restapitutorial.com/httpstatuscodes.html)
* [Versionamento Semântico](https://semver.org/)
* [Outras Diretrizes Públicas de API](http://apistylebook.com/design/guidelines/)
* [Práticas de Design OpenAPI](https://oai.github.io/Documentation/best-practices.html)
* [Microsoft TypeSpec](https://github.com/Microsoft/typespec)
* [Exemplos de Fluxo de Trabalho do GitHub Microsoft TypeSpec](https://github.com/cse-labs/typespec-workflow-samples/)
