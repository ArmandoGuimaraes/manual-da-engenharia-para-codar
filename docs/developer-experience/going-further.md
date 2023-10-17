# Expandindo as Possibilidades com Dev Containers

Os Dev Containers permitem que os desenvolvedores compartilhem um ambiente de trabalho comum, garantindo que as versões de tempo de execução e todas as dependências sejam consistentes para todos os desenvolvedores.

Os Dev Containers também nos permitem:

1. Utilizar ferramentas existentes para aprimorar os Dev Containers com mais recursos.
2. Fornecer ferramentas personalizadas (como scripts) para outros desenvolvedores.

## Ferramentas Existentes

Durante a fase de desenvolvimento, você provavelmente precisará usar ferramentas que não estão instaladas por padrão em seu Dev Container. Por exemplo, se o alvo do seu projeto for implantado no Azure, você precisará do [Azure-cli](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli) e talvez do [Terraform](https://www.terraform.io/) para implantação de recursos e aplicativos. Você pode encontrar esses Dev Containers no [repositório de galeria de Dev Containers do VS Code](https://github.com/microsoft/vscode-dev-containers/tree/master/containers).

Algumas outras ferramentas podem ser:

- Linters para arquivos [markdown](https://github.com/DavidAnson/markdownlint).
- Linters para scripts [bash](https://www.shellcheck.net/).
- Etc...

A verificação de arquivos que não são *o código-fonte* pode garantir um formato comum com regras comuns para cada desenvolvedor. Essas verificações também devem ser executadas em um [Pipeline de Integração Contínua](https://learn.microsoft.com/azure/devops/pipelines/architectures/devops-pipelines-baseline-architecture), mas é uma boa prática executá-las antes de abrir uma [Pull Request](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests).

## Limitação de Ferramentas Personalizadas

Se você decidir incluir o [Azure-cli](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli) em seu Dev Container, os desenvolvedores poderão executar comandos em seu ambiente. No entanto, para facilitar a vida dos desenvolvedores, podemos ir além, permitindo que eles preencham suas informações de conexão, como o `ID do locatário` e o `ID da assinatura`, de forma segura e persistente (não se esqueça de que seu Dev Container, sendo um contêiner [Docker](https://www.docker.com/), pode ser excluído ou a imagem pode ser reconstruída, o que resultaria na perda de todas as personalizações *internas*).

Uma maneira de fazer isso é usar variáveis de ambiente, com um arquivo `.env` não rastreado que faz parte da solução e é injetado no Dev Container.

Considere a seguinte estrutura de arquivos:

```bash
Minha Aplicação  # diretório principal do repositório
└───.devcontainer
|       ├───Dockerfile
|       ├───devcontainer.json
└───config
|       ├───.env
|       ├───.env-sample
```

O arquivo `config/.env-sample` é um arquivo rastreado onde qualquer pessoa pode encontrar variáveis de ambiente a serem definidas (sem valores, obviamente):

```bash
TENANT_ID=
SUBSCRIPTION_ID=
```

Em seguida, cada desenvolvedor que clona o repositório pode criar o arquivo `config/.env` e preenchê-lo com os valores apropriados.

Para injetar o arquivo `.env` no contêiner, você pode atualizar o arquivo `devcontainer.json` da seguinte forma:

```json
{
    ...
    "runArgs": ["--env-file","config/.env"],
    ...
}
```

Assim que o Dev Container for iniciado, essas variáveis de ambiente serão enviadas para o contêiner.

Outra abordagem seria usar o Docker Compose, um pouco mais complexo e provavelmente *demais* apenas para variáveis de ambiente. Usar o Docker Compose pode desbloquear outras configurações, como DNS personalizado, encaminhamento de portas ou múltiplos contêineres.

Para fazer isso, você precisa adicionar um arquivo `.devcontainer/docker-compose.yml` com o seguinte conteúdo:

```yaml
version: '3'
services:
  meu-ambiente:
    env_file: ../config/.env
    build:
      context: .
      dockerfile: Dockerfile
```

Para usar o arquivo `docker-compose.yml` em vez do `Dockerfile`, você precisa ajustar o `devcontainer.json` da seguinte maneira:

```json
{
    "name": "Minha Aplicação",
    "dockerComposeFile": ["docker-compose.yml"],
    "service": "meu-ambiente"
    ...
}
```

Essa abordagem pode ser aplicada a muitas outras ferramentas, preparando o que seria necessário. A ideia é simplificar a vida dos desenvolvedores e dos novos desenvolvedores que estão ingressando no projeto.

## Ferramentas Personalizadas

Enquanto trabalha em um projeto, qualquer desenvolvedor pode acabar escrevendo um script para automatizar uma tarefa. Este script pode ser em `bash`, `python` ou qualquer linguagem de script com a qual eles se sintam confortáveis.

Digamos que você queira garantir que todos os arquivos markdown escritos sejam validados de acordo com regras específicas que você configurou. Como vimos acima, você pode incluir a ferramenta [markdownlint](https://github.com/DavidAnson/markdownlint) em seu Dev Container. Ter a ferramenta instalada não significa que o desenvolvedor saberá como usá-la!

Considere a seguinte estrutura da solução:

```bash
Minha Aplicação  # diretório principal do repositório
└───.devcontainer
|       ├───Dockerfile
|       ├───docker-compose.yml
|       ├───devcontainer.json
└───scripts
|

       ├───check-markdown.sh
└───.markdownlint.json
```

O arquivo `.devcontainer/Dockerfile` instala o [markdownlint](https://github.com/DavidAnson/markdownlint):

```dockerfile
...
RUN apt-get update \
    && export DEBIAN_FRONTEND=noninteractive \
    && apt-get install -y nodejs npm

# Adicionar ferramentas NodeJS
RUN npm install -g markdownlint-cli
...
```

O arquivo `.markdownlint.json` contém as regras que você deseja validar em seus arquivos markdown (consulte o site do [markdownlint](https://github.com/DavidAnson/markdownlint) para obter detalhes).

E, finalmente, o script `scripts/check-markdown.sh` contém o seguinte código para executar o `markdownlint`:

```bash
# Obter a raiz do repositório
repoRoot="$( cd "$( dirname "${BASH_SOURCE[0]}" )/.." >/dev/null 2>&1 && pwd )"

# Executar o markdownlint para toda a solução
markdownlint -c "${repoRoot}"/.markdownlint.json
```

Quando o Dev Container é carregado, qualquer desenvolvedor pode executar este script em seu terminal:

```bash
/> ./scripts/check-markdown.sh
```

Este é um pequeno exemplo de uso, existem inúmeras outras possibilidades para capitalizar o trabalho feito pelos desenvolvedores para economizar tempo.

## Outras Considerações

### Arquitetura da Plataforma

Ao instalar ferramentas, você também precisa garantir que sabe qual a arquitetura dos computadores dos desenvolvedores. Todos os computadores baseados em Intel, independentemente de estarem rodando Windows, Linux ou macOS, terão o mesmo comportamento.
No entanto, a arquitetura mais recente da Apple (Apple M1/Silicon) sendo ARM64, significa que o comportamento não é o mesmo ao criar Dev Containers.

Por exemplo, se você deseja instalar o [Azure-cli](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli) em seu Dev Container, não poderá fazê-lo da mesma forma que faria em máquinas Intel. Em computadores Intel, você pode instalar o pacote `deb`. No entanto, esse pacote não está disponível na arquitetura ARM. A única maneira de instalar o [Azure-cli](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli) no Linux ARM é usando o instalador Python `pip`.

Para fazer isso, você precisa verificar a arquitetura do host que está criando o Dev Container, seja no Dockerfile ou chamando um script bash externo para instalar as ferramentas restantes que não possuem uma versão universal.

Aqui está um trecho para chamar do Dockerfile:

```bash
# Se for baseado em Intel, use o arquivo deb
if [[ `dpkg --print-architecture` == "amd64" ]]; then
    sudo curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash;
else
# Baseado em ARM, instale pip (e gcc) e, em seguida, azure-cli
    sudo apt-get -y install gcc
    python3 -m pip install --upgrade pip
    python3 -m pip install azure-cli
fi
```

### Reutilização de Credenciais para o GitHub

Se você desenvolve dentro de um Dev Container, também desejará compartilhar suas credenciais do GitHub entre seu host e o Dev Container. Fazendo isso, você evitaria copiar suas chaves SSH de um lado para o outro (se estiver usando SSH para acessar seus repositórios).

Uma abordagem seria montar sua pasta local `~/.ssh` em seu Dev Container. Você pode usar a opção `mounts` do `devcontainer.json` ou usar o Docker Compose.

- Usando `mounts`:

```json
{
    ...
    "mounts": ["source=${localEnv:HOME}/.ssh,target=/home/vscode/.ssh,type=bind"],
    ...
}
```

Como você pode ver, `${localEnv:HOME}` retorna a pasta `home` do host e a mapeia para a pasta do contêiner.

- Usando Docker Compose:

```yaml
version: '3'
services:
  meu-ambiente:
    env_file: ../configs/.env
    build:
      context: .
      dockerfile: Dockerfile
    volumes:
      - "~/.ssh:/home/alex/.ssh"
    command: sleep infinity
```

Observe que o uso do Docker Compose requer a edição do arquivo `devcontainer.json`, conforme vimos acima.

Agora você pode acessar o GitHub usando as mesmas credenciais do seu host, sem se preocupar com a persistência.

### Permitir Alguma Customização

Como nota final, também é interessante deixar aos desenvolvedores alguma flexibilidade em seu ambiente para personalização.

Por exemplo, alguém pode querer adicionar aliases ao seu ambiente. No entanto, alterar o arquivo `~/.bashrc` no Dev Container não é uma abordagem boa, pois o contêiner pode ser destruído. Existem várias maneiras de definir a persistência. Aqui está uma abordagem.

Considere a seguinte estrutura do projeto:

```bash
Minha Aplicação  # diretório principal do repositório
└───.devcontainer
|       ├───Dockerfile
|       ├───docker

-compose.yml
|       ├───devcontainer.json
└───me
|       ├───bashrc_extension
```

A pasta `me` não está rastreada no repositório, deixando aos desenvolvedores a flexibilidade de adicionar recursos pessoais. Um desses recursos pode ser uma extensão `.bashrc` contendo personalizações. Por exemplo:

```bash
# Exemplo de alias
alias gaa="git add --all"
```

Agora podemos adaptar nosso `Dockerfile` para carregar essas alterações quando a imagem Docker é criada (e, é claro, não fazer nada se não houver nenhum arquivo):

```dockerfile
...
RUN echo "[ -f PATH_TO_WORKSPACE/me/bashrc_extension ] && . PATH_TO_WORKSPACE/me/bashrc_extension" >> ~/.bashrc;
...
```
