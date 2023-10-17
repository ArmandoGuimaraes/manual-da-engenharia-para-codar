# Separando aplicativos cliente dos serviços que consomem durante o desenvolvimento

Os aplicativos cliente normalmente dependem de serviços remotos para alimentar seus aplicativos. No entanto, os cronogramas de desenvolvimento entre o aplicativo cliente e os serviços nem sempre se alinham completamente. Para um ciclo de desenvolvimento interno de alta velocidade, o desenvolvimento do aplicativo cliente deve ser desacoplado dos serviços backend, permitindo ainda que o aplicativo "chame" os serviços para testes locais.

## Opções

Existem várias opções para desacoplar o desenvolvimento do aplicativo cliente dos serviços backend. As opções variam desde a incorporação de implementações fictícias dos serviços na aplicação até o uso de versões simplificadas dos serviços.

Este documento lista várias opções e discute as compensações.

### Mocks Incorporados

Uma solução de mock incorporada inclui classes que implementam as interfaces de serviço localmente. As interfaces e classes de dados, também chamadas de modelos ou objetos de transferência de dados (DTOs), muitas vezes são geradas a partir das especificações da API dos serviços usando ferramentas como nswag ([RicoSuter/NSwag: The Swagger/OpenAPI toolchain for .NET, ASP.NET Core and TypeScript. (github.com)](https://github.com/RicoSuter/NSwag)) ou autorest ([Azure/autorest: OpenAPI (f.k.a Swagger) Specification code generator. Supports C#, PowerShell, Go, Java, Node.js, TypeScript, Python, Ruby (github.com)](https://github.com/Azure/AutoRest)).

Uma implementação simples do serviço pode retornar uma resposta estática. Para serviços RESTful, as respostas JSON para os stubs podem ser armazenadas como recursos da aplicação ou simplesmente como strings estáticas.

```cs
public Task<UserProfile> GetUserAsync(long userId, CancellationToken cancellationToken)
{
    UserProfile result = Newtonsoft.Json.JsonConvert.DeserializeObject<UserProfile>(
        MockUserProfile.UserProfile, new Newtonsoft.Json.JsonSerializerSettings());

    return Task.FromResult(result);
}
```

Implementações mais sofisticadas podem retornar erros aleatórios para testar os caminhos de código de resiliência do aplicativo.

Os mocks podem ser ativados por meio de compilação condicional ou dinamicamente por meio da configuração do aplicativo. Em ambos os casos, é recomendável garantir que mocks, respostas de serviço e configurações externalizadas não sejam incluídos na versão final para evitar comportamentos confusos e inclusão de vulnerabilidades potenciais.

#### Exemplo: Registrando Mocks via Injeção de Dependência

Contêineres de Injeção de Dependência como o Unity ([Unity Container Introduction \| Unity Container](http://unitycontainer.org/articles/introduction.html)) tornam
fácil alternar entre serviços fictícios e implementações reais de cliente de serviço. Como ambos implementam a mesma interface, as implementações podem ser registradas com o contêiner Unity.

```cs
public static void Bootstrap(IUnityContainer container)
{

#if DEBUG
    container.RegisterSingleton<IUserServiceClient, MockUserService>();
#else
    container.RegisterSingleton<IUserServiceClient, UserServiceClient>();
#endif

}
```

#### Consumindo mocks via Injeção de Dependência

O código que consome as interfaces não perceberá a diferença.

```cs
public class UserPageModel
{
    private readonly IUserServiceClient userServiceClient;

    public UserPageModel(IUserServiceClient userServiceClient)
    {
        this.userServiceClient = userServiceClient;
    }

    // ...
}
```

### Serviços Locais

A abordagem com Serviços Executados Localmente é substituir a chamada no cliente que aponta para o ponto de extremidade real (seja desenvolvimento, QA, produção, etc.) por um ponto de extremidade local.

Essa abordagem também permite injetar proxies de captura de tráfego e modelagem como o Postman ([Postman API Platform \| Sign Up for Free](https://www.postman.com/)) ou o Fiddler ([Fiddler \| Web Debugging Proxy and Troubleshooting Solutions (telerik.com)](https://www.telerik.com/fiddler)).

A vantagem dessa abordagem é que as APIs são desacopladas do cliente e podem ser atualizadas/modificadas independentemente (por exemplo, alterando códigos de resposta, alterando dados) sem exigir alterações no cliente. Isso ajuda a desbloquear novos cenários de desenvolvimento e oferece flexibilidade durante a fase de desenvolvimento.

O desafio com essa abordagem é que ela requer configuração e execução dos

 serviços localmente. Existem ferramentas que ajudam a simplificar esse processo (por exemplo, [JsonServer](https://www.npmjs.com/package/json-server), [Postman Mock Server](https://learning.postman.com/docs/designing-and-developing-your-api/mocking-data/setting-up-mock/)).

#### Serviços Locais de Alta Fidelidade

Um stub de serviço local implementa as APIs esperadas. Assim como o mock incorporado, ele pode ser gerado com base nos contratos de API existentes (por exemplo, OpenAPI).

Uma abordagem de alta fidelidade empacota os serviços reais juntamente com dados simplificados em contêineres Docker que podem ser executados localmente usando docker-compose antes que o aplicativo cliente seja iniciado para depuração e testes locais. Para permitir a execução de serviços completamente locais, a "versão local" substitui serviços em nuvem dependentes por alternativas locais, como armazenamento de arquivos em vez de blobs, SQL Server em execução localmente em vez de SQL AzureDB.

Essa abordagem também permite testes de integração de alta fidelidade sem iniciar implantações distribuídas.

### Stub / Serviços Falsos

Abordagens de menor fidelidade executam serviços de stub, que podem ser gerados a partir das especificações da API, ou executam servidores fictícios como JsonServer ([JsonServer.io: A fake json server API Service for prototyping and testing.](https://www.jsonserver.io/)) ou Postman. Todos esses serviços responderiam com mensagens JSON predefinidas e configuradas.

## Como decidir

|                | Prós                                   | Contras                        | Exemplo ao desenvolver para:    | Exemplo de quando não usar                       |
|----------------|----------------------------------------|-----------------------------|---------------------------------|-----------------------------------------------|
| Mocks Incorporados | Simplifica a experiência do desenvolvedor F5 | Fortemente acoplado ao Cliente | Cenários de dados de tipo mais estático | Testes (por exemplo, testes unitários, testes de integração) |
|| Sem dependências externas para gerenciar | Dados codificados | Integração inicial com serviços |
| | | Mocking via Injeção de Dependência pode ser um esforço não trivial | | |
| Serviços Locais de Alta Fidelidade | Desacoplado do Cliente | Requer ferramentas extras, ou seja, sobrecarga de infraestrutura local | Rotas URL | Quando os contratos da API não estão disponíveis |
| | Mais fácil modificar independentemente a resposta | Configuração extra e configuração de serviços | | |
| | Atualizações independentes dos serviços | | | |
| | Pode utilizar o tráfego HTTP | | | |
| | Mais fácil substituir por serviços reais posteriormente | | | |
| Serviços de Stub/Fake | Desacoplado do cliente | Requer ferramentas extras, ou seja, sobrecarga de infraestrutura local | Códigos de resposta | Quando os contratos da API estão disponíveis |
| | Mais fácil modificar independentemente a resposta | Configuração extra e configuração de serviços | Cenários de dados complexos/variáveis | Quando os contratos da API não estão disponíveis |
| | Atualizações independentes dos serviços | Pode não fornecer a fidelidade completa da API esperada | | |
| | Pode utilizar o tráfego HTTP | | | |
| | Mais fácil substituir por serviços reais posteriormente | | | |
