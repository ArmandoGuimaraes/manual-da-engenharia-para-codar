# Ciclo de Desenvolvimento Interno com Serviços Falsos

## Introdução

Os consumidores de serviços remotos muitas vezes encontram dificuldades em sincronizar seu ciclo de desenvolvimento com o desenvolvimento dos serviços remotos, deixando os desenvolvedores desses consumidores aguardando os serviços remotos "alcançarem" seus requisitos. Uma abordagem para mitigar esse problema e melhorar o ciclo de desenvolvimento interno é a desacoplagem e o uso de Serviços Falsos (Fake Services). Várias opções de Serviços Falsos são detalhadas [aqui](client-app-inner-loop.md).

Este documento se concentrará em fornecer um exemplo usando a abordagem de Serviços Falsos.

## API

Para nosso exemplo de API, trabalharemos com um endpoint `/User` e as propriedades para `User` serão:

1. id - int
2. username - string
3. firstName - string
4. lastName - string
5. email - string
6. password - string
7. phone - string
8. userStatus - int

## Ferramentas

Para a abordagem de Serviços Falsos, estaremos usando o [Json-Server](https://github.com/typicode/json-server). Json-Server é uma ferramenta que fornece a capacidade de simular completamente APIs REST e executar o servidor localmente. Ele é projetado para criar APIs REST com funcionalidade CRUD com configuração mínima. O Json-Server requer o NodeJS e é instalado via NPM.

```bash
npm install -g json-server
```

## Configuração

Para executar o Json-Server, ele requer apenas uma fonte de dados e inferirá as rotas com base no arquivo de dados. Observe que personalizações adicionais podem ser feitas para cenários mais avançados (por exemplo, rotas personalizadas). Detalhes podem ser encontrados [aqui](https://github.com/typicode/json-server#add-custom-routes).

Para nosso exemplo, usaremos o seguinte arquivo de dados, `db.json`:

```text
{
  "user": [
    {
      "id": 0,
      "username": "user1",
      "firstName": "Kobe",
      "lastName": "Bryant",
      "email": "kobe@example.com",
      "password": "superSecure1",
      "phone": "(123) 123-1234",
      "userStatus": 0
    },
    {
      "id": 1,
      "username": "user2",
      "firstName": "Shaquille",
      "lastName": "O'Neal",
      "email": "shaq@example.com",
      "password": "superSecure2",
      "phone": "(123) 123-1235",
      "userStatus": 0
    }
  ]
}
```

## Execução

A execução do Json-Server pode ser realizada simplesmente executando:

```bash
json-server --watch src/db.json
```

Uma vez em execução, o endpoint de Usuário pode ser acessado na porta localhost padrão: `http:/localhost:3000/user`

Observe que o Json-Server pode ser configurado para usar outras portas usando a seguinte sintaxe:

```bash
json-server --watch db.json --port 3004
```

## Endpoint

O endpoint pode ser testado executando o comando curl contra ele, e podemos especificar qual objeto de usuário obter com o seguinte comando:

```bash
curl http://localhost:3000/user/1
```

o que, como esperado, retornará:

```text
{
  "id": 1,
  "username": "user2",
  "firstName": "Shaquille",
  "lastName": "O'Neal",
  "email": "shaq@example.com",
  "password": "superSecure2",
  "phone": "(123) 123-1235",
  "userStatus": 0
}
```
