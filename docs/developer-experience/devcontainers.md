# Dev Containers: Iniciando

Se você é um desenvolvedor com experiência no Visual Studio Code (VS Code) ou Docker, provavelmente é hora de dar uma olhada nos [containers de desenvolvimento](https://code.visualstudio.com/docs/remote/containers) (dev containers). Este guia destina-se a ajudar os desenvolvedores no processo de tomada de decisão necessário para criar dev containers. A orientação fornecida deve ser especialmente útil se você estiver experimentando os dev containers do VS Code pela primeira vez.

> **Observação:** Este guia não trata da configuração de um arquivo Docker para implantar um programa Python em execução para CI/CD.

## Pré-requisitos

- Experiência com o VS Code
- Experiência com o Docker

## O que são dev containers?

Os containers de desenvolvimento são um recurso do VS Code que permite aos desenvolvedores empacotar uma pilha de ferramentas de desenvolvimento local dentro de um contêiner Docker, ao mesmo tempo em que trazem a experiência da interface de usuário do VS Code com eles. Você já configurou um ponto de interrupção dentro de um contêiner Docker? Talvez não. Os dev containers tornam isso possível. Isso é feito por meio de uma extensão do VS Code chamada [Remote Development Extension Pack](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack) que trabalha em conjunto com o Docker para iniciar um servidor do VS Code dentro de um contêiner Docker. O componente de interface de usuário do VS Code permanece local, mas seus arquivos de trabalho são montados no contêiner. O diagrama abaixo, retirado diretamente da [documentação oficial do VS Code](https://code.visualstudio.com/docs/remote/containers), ilustra isso:

![imagem](https://user-images.githubusercontent.com/10041279/93239062-e1b9a480-f747-11ea-94fb-3d50b14fd9b1.png)

Se o diagrama acima não estiver claro, uma analogia básica que pode ajudá-lo a entender intuitivamente os dev containers é pensar neles como uma união entre o modo interativo do Docker (`docker exec -it 987654e0ff32`) e a experiência da interface de usuário do VS Code com a qual você está acostumado.

Para se configurar para a experiência do dev container descrita acima, use o Mercado de Extensões do VS Code para instalar o [Remote Development Extension Pack](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack).

## Como os dev containers podem melhorar a colaboração em projetos?

Os dev containers do VS Code melhoraram a colaboração em projetos recentes da equipe, abordando dois problemas muito específicos:

- Experiências locais de desenvolvimento inconsistentes dentro de uma equipe.
- Onboarding lento de desenvolvedores que estão ingressando em um projeto.

Os problemas listados acima foram resolvidos configurando e compartilhando uma definição de dev container. Os dev containers são definidos por sua imagem base e os artefatos que suportam essa imagem base. A imagem base e os artefatos que a acompanham ficam no diretório .devcontainer. Este diretório é onde a configuração começa. Um artefato central para a definição do dev container é um arquivo de configuração chamado `devcontainer.json`. Este arquivo orquestra os artefatos necessários para dar suporte à imagem base do dev container e ao ciclo de vida do dev container. A instalação do [Remote Development Extension Pack](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack) é necessária para habilitar essa orquestração dentro de um repositório de projeto.

Espera-se que todos os desenvolvedores da equipe compartilhem e usem a definição de dev container (.diretório .devcontainer) para criar um contêiner. Essa definição fornece ferramentas consistentes para desenvolver localmente uma aplicação em toda a equipe.

Os trechos de código abaixo demonstram a localização comum de um diretório .devcontainer e do arquivo devcontainer.json dentro de um repositório de projeto. Eles também destacam a maneira correta de fazer referência a um arquivo Docker.

```bash
$ tree vs-code-remote-try-python  # diretório principal do repositório
└───.devcontainers
        ├───Dockerfile
        ├───devcontainer.json
```

```json
# devcontainer.json
{
    "name": "Python 3",
    "build": {
        "dockerfile": "Dockerfile",
        "context": "..",
        // Atualize 'VARIANT' para escolher uma versão do Python: 3, 3.6, 3.7, 3.8
        "args": {"VARIANT": "3.8"}
    },
}
```

Para obter uma lista das propriedades de configuração do devcontainer.json, visite a documentação do VS Code sobre [propriedades de contêineres de desenvolvimento](https://code.visualstudio.com/docs/remote/devcontainerjson-reference).

## Como decidir qual dev container é o certo para o meu caso de uso?

Felizmente, o VS Code possui uma galeria de pastas específicas da plataforma que hospedam definições de dev container (.diretórios .devcontainer) para facilitar o início com dev containers. O trecho de código abaixo mostra uma lista de pastas de galeria que vêm diretamente do [repositório de galeria de dev containers do VS Code](https://github.com/microsoft/vscode-dev-containers/tree/master/containers):

```bash
$ tree vs-code-dev-containers  # diretório principal do repositório
└───containers
        ├───dotnetcore
        |   └───.devcontainers # dev container
        ├───python-3
        |   └───.devcontainers # dev container
        ├───ubuntu
        |   └───.devcontainers # dev container
        └───....
```

Aqui estão as etapas gerais finais para criar um dev container:

1. Decida qual plataforma você deseja usar para criar uma pilha de ferramentas de desenvolvimento local.
2. Navegue pela galeria de dev containers fornecida pelo VS Code de pastas de projetos que visam sua plataforma e escolha a mais apropriada.
3. Inspecione as definições de dev container (.diretório .devcontainer) de um projeto quanto à imagem base e aos artefatos que a suportam.
4. Use o que você descobriu para começar a configurar o dev container como ele é, estendê-lo ou construir o seu próprio do zero.

## Indo mais fundo

Existem

 casos de uso em que você gostaria de ir mais fundo na configuração do seu Dev Container. [Mais detalhes aqui](going-further.md)
