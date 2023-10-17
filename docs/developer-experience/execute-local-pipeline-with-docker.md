# Executando pipelines localmente

## Resumo

A capacidade de executar atividades de pipeline localmente foi identificada como uma oportunidade para promover uma experiência positiva para desenvolvedores. Neste documento, exploraremos uma solução que nos permitirá ter a experiência local de CI o mais semelhante possível ao processo remoto no servidor de CI.

Usar o método sugerido nos permitirá:

- Construir
- Lintar
- Testar unitariamente
- Testar E2E (End-to-End)
- Executar a solução
- Ser agnóstico em relação ao sistema operacional e ao ambiente.

## Usando o Docker Compose

O [Docker Compose](https://docs.docker.com/compose/) permite construir, publicar ou executar aplicativos Docker com vários contêineres.

### Método de trabalho

1. Crie imagens Docker para suas aplicações, incluindo uma etapa de compilação, se possível.
2. Adicione uma etapa em seu Dockerfile para executar testes unitários.
3. Adicione uma etapa no Dockerfile para a verificação de estilo (linting).
4. Crie um novo Dockerfile, possivelmente em uma pasta diferente, que execute testes de ponta a ponta (E2E) contra o cluster. Certifique-se de que os pontos de extremidade padrão sejam configuráveis (isso será útil em seu servidor de CI remoto, onde você poderá testar em um ambiente ao vivo, se desejar).
5. Crie um arquivo docker-compose que permita escolher quais dos serviços executar. O padrão executará todas as aplicações e testes, e um parâmetro opcional pode executar serviços específicos, por exemplo, apenas a aplicação sem os testes.

### Pré-requisitos

1. [Docker](https://www.docker.com/products/docker-desktop)
2. Opcional: se você clonar o aplicativo de exemplo, precisará ter o [dotnet core](https://dotnet.microsoft.com/download) instalado.

### Passo a passo com exemplos

Para este tutorial, vamos usar um [aplicativo de exemplo .NET Core API](https://github.com/itye-msft/cse-engagement-template). Aqui está o Dockerfile para o aplicativo de exemplo:

```sh
# https://hub.docker.com/_/microsoft-dotnet
FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /app

# copia o csproj e restaura em camadas distintas
COPY ./ ./
RUN dotnet restore

RUN dotnet test

# copia tudo o mais e constrói a aplicação
COPY SampleApp/. ./
RUN dotnet publish -c release -o out --no-restore

# estágio/fase final/imagem
FROM mcr.microsoft.com/dotnet/aspnet:5.0
WORKDIR /app
COPY --from=build /app/out .
ENTRYPOINT ["dotnet", "SampleNetApi.dll"]
```

Este script restaura todas as dependências, constrói e executa os testes. O aplicativo .NET Core inclui o `stylecop`, que falha na compilação em caso de problemas de linting.

A seguir, criaremos um Dockerfile para executar um teste de ponta a ponta. Normalmente, isso se parecerá com um conjunto de scripts ou um aplicativo dedicado que faz chamadas HTTP reais para uma aplicação em execução. Para simplificar, o próprio Dockerfile executará um comando curl simples:

```sh
FROM alpine:3.7
RUN apk --no-cache add curl
ENTRYPOINT ["curl", "0.0.0.0:8080/weatherforecast"]
```

Agora estamos prontos para combinar ambos os Dockerfiles em um script docker-compose:

```sh
version: '3'
services:
  app:
    image: app:0.01
    build:
      context: .
    ports:
      - "8080:80"
  e2e:
    image: e2e:0.01
    build:
      context: ./E2E
```

O script docker-compose lançará os 2 Dockerfiles e os construirá se ainda não tiverem sido construídos. O comando a seguir executará o docker-compose:

```sh
docker-compose up --build -d
```

Assim que as imagens estiverem ativas, você poderá fazer chamadas para o serviço. A imagem de teste de ponta a ponta executará o conjunto de testes de ponta a ponta. Se você quiser pular os testes, pode simplesmente dizer ao compose para executar um serviço específico, conforme a seguir:

```sh
docker-compose up --build -d app
```

Agora você tem um script local que constrói e testa sua aplicação. O próximo passo seria fazer com que seu CI execute o script docker-compose.

Aqui está um exemplo de um arquivo yaml usado pelas pipelines do Azure DevOps:

```sh
trigger:
- master

pool:
  vmImage: 'ubuntu-latest'

variables:
  solution: '**/*.sln'
  buildPlatform: 'Any CPU'
  buildConfiguration: 'Release'

steps:
- task: DockerCompose@0
  displayName: Build, Test, E2E
  inputs:
    action: Run services
    dockerComposeFile: docker-compose.yml
- script: dotnet restore SampleApp
- script: dotnet build --configuration $(buildConfiguration) SampleApp
  displayName: 'dotnet build $(buildConfiguration)'
```

Neste script, o primeiro passo é docker-compose, que usa o mesmo arquivo que criamos nos passos anteriores. Os próximos passos fazem a mesma coisa usando scripts, e estão aqui para fins de comparação. No final deste passo, seu CI efetivamente executa os mesmos comandos de construção e teste que você executa localmente.
