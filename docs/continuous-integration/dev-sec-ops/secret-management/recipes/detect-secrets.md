# Ferramenta de Verificação de Credenciais: detect-secrets

## Contexto

A ferramenta [`detect-secrets`](https://github.com/Yelp/detect-secrets) é um projeto de código aberto que usa heurísticas e regras para escanear [uma ampla gama](https://github.com/Yelp/detect-secrets#currently-supported-plugins) de segredos. Podemos estender a ferramenta com regras e heurísticas personalizadas por meio de uma simples [API de plug-in Python](https://github.com/Yelp/detect-secrets/blob/a9dff60/detect_secrets/plugins/base.py#L27-L49).

Ao contrário de outras ferramentas de verificação de credenciais, o `detect-secrets` não tenta verificar todo o histórico git de um projeto quando é invocado, mas sim escaneia o estado atual do projeto. Isso significa que a ferramenta é rápida, o que a torna ideal para uso em pipelines de integração contínua.

O `detect-secrets` utiliza o conceito de um "arquivo de base", ou seja, uma lista de segredos conhecidos já presentes no repositório, e podemos configurá-lo para ignorar qualquer um desses segredos pré-existentes ao ser executado. Isso torna fácil introduzir gradualmente a ferramenta em um projeto já existente.

O arquivo de base também fornece uma maneira simples e conveniente de lidar com falsos positivos. Podemos incluir na lista branca o falso positivo no arquivo de base para ignorá-lo em futuras invocações da ferramenta.

## Configuração

```sh
# instale as dependências do sistema: diff, jq, python3 (se estiver em um sistema operacional baseado em Linux)
apt-get install -y diffutils jq python3 python3-pip

# instale as dependências do sistema: diff, jq, python3 (se estiver no Windows)
winget install Python.Python.3
choco install diffutils jq -y

# instale a ferramenta detect-secrets
python3 -m pip install detect-secrets

# execute a ferramenta para estabelecer uma lista de segredos conhecidos
# revise este arquivo cuidadosamente e faça check-in no repositório
detect-secrets scan > .secrets.baseline
```

## Gancho pré-commit

É recomendável usar o `detect-secrets` em seu ambiente de desenvolvimento como um gancho pré-commit do Git.

Primeiro, siga as [instruções de instalação do `pre-commit`](https://pre-commit.com/#install) para instalar a ferramenta em seu ambiente de desenvolvimento.

Em seguida, adicione o seguinte ao seu `.pre-commit-config.yaml`:

```yaml
repos:
-   repo: https://github.com/Yelp/detect-secrets
    rev: v1.4.0
    hooks:
    -   id: detect-secrets
        args: ['--baseline', '.secrets.baseline']
```

## Uso em pipelines de CI

```sh
# faça backup da lista de segredos conhecidos
cp .secrets.baseline .secrets.new

# encontre todos os segredos no repositório
detect-secrets scan --baseline .secrets.new $(find . -type f ! -name '.secrets.*' ! -path '*/.git*')

# se houver alguma diferença entre os segredos conhecidos e os recém-detectados, quebre o build
list_secrets() { jq -r '.results | keys[] as $key | "\($key),\(.[$key] | .[] | .hashed_secret)"' "$1" | sort; }

if ! diff <(list_secrets .secrets.baseline) <(list_secrets .secrets.new) >&2; then
  echo "Foram detectados novos segredos no repositório" >&2
  exit 1
fi
```

