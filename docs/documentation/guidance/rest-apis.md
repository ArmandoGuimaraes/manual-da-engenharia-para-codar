# REST APIs

Ao criar [REST APIs](https://en.wikipedia.org/wiki/Representational_state_transfer), você pode aproveitar a [Especificação OpenAPI (OAI)](https://github.com/OAI/OpenAPI-Specification/) (originalmente conhecida como Swagger Specification) para descrevê-las:

> A Especificação OpenAPI (OAS) define uma interface padrão independente de linguagem de programação para APIs HTTP, o que permite que tanto humanos quanto computadores descubram e entendam as capacidades de um serviço sem exigir acesso ao código-fonte, documentação adicional ou inspeção do tráfego de rede. Quando devidamente definida via OpenAPI, um consumidor pode compreender e interagir com o serviço remoto com uma quantidade mínima de lógica de implementação.
>
> Os casos de uso para documentos de definição de API legíveis por máquina incluem, mas não se limitam a: documentação interativa; geração de código para documentação, clientes e servidores; e automação de casos de teste. Os documentos OpenAPI descrevem os serviços de uma API e são representados em formatos YAML ou JSON. Esses documentos podem ser produzidos e servidos estaticamente ou gerados dinamicamente a partir de um aplicativo.

Existem implementações disponíveis para muitas linguagens, incluindo C#, que incluem ferramentas de baixo nível, editores, interfaces de usuário, geradores de código, etc. Aqui você pode encontrar uma lista de ferramentas conhecidas para diferentes linguagens: [OpenAPI-Specification/IMPLEMENTATIONS.md](https://github.com/OAI/OpenAPI-Specification/blob/main/IMPLEMENTATIONS.md).

## Usando o Microsoft TypeSpec

Embora a [Especificação OpenAPI (OAI)](https://github.com/OAI/OpenAPI-Specification/) seja um método popular para definir e documentar APIs RESTful, existem outras linguagens disponíveis que podem simplificar e acelerar o processo de documentação. O Microsoft TypeSpec é uma dessas linguagens que permite a descrição de APIs de serviços em nuvem e a geração de linguagens de descrição de API, código de cliente e serviço, documentação e outros recursos.

O [Microsoft TypeSpec](https://github.com/Microsoft/typespec) é uma linguagem altamente extensível que oferece um conjunto de primitivos principais que podem descrever formatos comuns de API entre REST, OpenAPI, GraphQL, gRPC e outros protocolos. Isso o torna uma opção versátil para desenvolvedores que precisam trabalhar com uma variedade de estilos e tecnologias de API diferentes.

O [Microsoft TypeSpec](https://github.com/Microsoft/typespec) é uma ferramenta amplamente adotada nas equipes da Azure, especialmente para gerar Especificações OpenAPI em APIs complexas e interconectadas que abrangem várias equipes. Para garantir consistência em diferentes partes da API, as equipes comumente aproveitam bibliotecas compartilhadas que contêm padrões reutilizáveis. Isso torna mais fácil seguir as melhores práticas em vez de desviá-las. Ao promover designs de API altamente regulares que aderem às melhores práticas desde o início, o TypeSpec pode ajudar a melhorar a qualidade e a consistência das APIs desenvolvidas em uma organização.

## Referências

- [Documentação da API da ASP.NET Core com Swagger / OpenAPI](https://learn.microsoft.com/en-us/aspnet/core/tutorials/web-api-help-pages-using-swagger?view=aspnetcore-5.0).
- [Microsoft TypeSpec](https://github.com/Microsoft/typespec).
- [Padrões de Design - Orientações para Design de API REST](https://microsoft.github.io/code-with-engineering-playbook/design/design-patterns/rest-api-design-guidance/)
