# Revisão de Código em Bash

## Guia de Estilo

Os desenvolvedores devem seguir o [Guia de Estilo Bash do Google](https://google.github.io/styleguide/shell.xml).

## Análise de Código / Linting

Projetos devem verificar o código Bash com o [shellcheck](https://github.com/koalaman/shellcheck) como parte do [processo de CI](../../continuous-integration/README.md).
Além do linting, [shfmt](https://github.com/mvdan/sh) pode ser usado para formatar automaticamente scripts shell. Existem algumas extensões de código do vscode baseadas no shfmt, como shell-format, que podem ser usadas para formatar automaticamente scripts shell.

## Configuração do Projeto

### vscode-shellcheck

A extensão Shellcheck deve ser usada no VS Code, ela fornece capacidades de análise de código estático e correção automática de problemas de linting. Para usar o vscode-shellcheck no VS Code, faça o seguinte:

#### Instale o shellcheck em sua máquina

Para macOS

```bash
brew install shellcheck
```

Para Ubuntu:

```bash
apt-get install shellcheck
```

#### Instale o shellcheck no vscode

Encontre a extensão vscode-shellcheck no vscode e instale-a.

## Formatação Automática de Código

### shell-format

A extensão shell-format faz a formatação automática de seus scripts bash, arquivos Docker e vários arquivos de configuração. Ela depende do shfmt, que pode aplicar verificações do guia de estilo do Google para bash.
Para usar o shell-format no vscode, siga os passos abaixo:

#### Instale o shfmt (requer Go 1.13 ou posterior) em sua máquina

```bash
GO111MODULE=on go get mvdan.cc/sh/v3/cmd/shfmt
```

#### Instale o shell-format no vscode

Encontre a extensão shell-format no vscode e instale-a.

## Validação de Build

Para automatizar esse processo no Azure DevOps, você pode adicionar o seguinte trecho ao seu arquivo `azure-pipelines.yaml`. Isso fará a verificação de linting em qualquer script na pasta `./scripts/`.

```yaml
- bash: |
    echo "Isso verifica a formatação e erros comuns do Bash. Consulte o wiki para detalhes sobre erros e opções de ignorar: https://github.com/koalaman/shellcheck/wiki/SC1000"
    export scversion="stable"
    wget -qO- "https://github.com/koalaman/shellcheck/releases/download/${scversion?}/shellcheck-${scversion?}.linux.x86_64.tar.xz" | tar -xJv
    sudo mv "shellcheck-${scversion}/shellcheck" /usr/bin/
    rm -r "shellcheck-${scversion}"
    shellcheck ./scripts/*.sh
  displayName: "Validar Scripts: Shellcheck"
```

Além disso, seus scripts shell podem ser formatados em sua pipeline de build usando a ferramenta `shfmt`. Para integrar o `shfmt` em sua pipeline de build, faça o seguinte:

```yaml
- bash: |
    echo "Esta etapa faz a formatação automática de scripts shell"
    shfmt -l -w ./scripts/*.sh
  displayName: "Formatar Scripts: shfmt"
```

A realização de testes unitários usando [shunit2](https://github.com/kward/shunit2) também pode ser adicionada à pipeline de build, usando o seguinte bloco:

```yaml
- bash: |
    echo "Esta etapa realiza testes unitários em scripts shell usando shunit2"
    ./shunit2
  displayName: "Formatar Scripts: shfmt"
```

## Hooks Pré-Commit

Todos os desenvolvedores devem executar o shellcheck e o shfmt como hooks pré-commit.

### Etapa 1 - Instalar o pre-commit

Execute `pip install pre-commit` para instalar o pre-commit.
Alternativamente, você pode executar `brew install pre-commit` se estiver usando o Homebrew.

### Etapa 2 - Adicione o shellcheck e o shfmt

Adicione o arquivo .pre-commit-config.yaml à raiz do projeto em Go. Execute o shfmt no pré-commit adicionando-o ao arquivo .pre-commit-config.yaml como abaixo.

```yaml
-   repo: git://github.com/pecigonzalo/pre-commit-fmt
    sha: master
    hooks:
      -   id: shell-fmt
          args:
            - --indent=4
```

```yaml


-   repo: https://github.com/shellcheck-py/shellcheck-py
    rev: v0.7.1.1
    hooks:
    -   id: shellcheck
```

### Etapa 3

Execute `$ pre-commit install` para configurar os scripts de hook do git.

## Dependências

Scripts Bash são frequentemente usados para 'ligar' outros sistemas e ferramentas. Portanto, os scripts Bash podem ter muitas e/ou complicadas dependências. Considere usar contêineres Docker para garantir que os scripts sejam executados em um ambiente portátil e reproduzível que contenha todas as dependências corretas. Para garantir que os scripts com Docker sejam fáceis de executar, considere tornar o uso do Docker transparente para quem chama o script, envolvendo o script em um 'bootstrap' que verifica se o script está sendo executado no Docker e o reexecuta em Docker se não for o caso. Isso proporciona o melhor dos dois mundos: execução fácil de script e ambientes consistentes.

```bash
if [[ "${DOCKER}" != "true" ]]; then
  docker build -t my_script -f my_script.Dockerfile . > /dev/null
  docker run -e DOCKER=true my_script "$@"
  exit $?
fi

# ... implementação do my_script aqui pode assumir que todas as suas dependências existem, pois ele sempre está sendo executado no Docker ...
```

## Checklist de Revisão de Código

Além do [Checklist de Revisão de Código](../process-guidance/reviewer-guidance.md), você também deve verificar esses itens específicos de revisão de código em Bash

* [ ] Esse código usa [Opções Internas do Shell](https://www.gnu.org/software/bash/manual/html_node/The-Set-Builtin.html) como set -o, set -e, set -u para controle de execução de scripts shell?
* [ ] O código está modularizado? Scripts shell podem ser modularizados como módulos Python. Partes de scripts Bash devem ser importadas em projetos Bash complexos.
* [ ] Todas as exceções são tratadas corretamente? Exceções devem ser tratadas corretamente usando códigos de saída ou capturando sinais.
* [ ] O código passa em todas as verificações de linting conforme shellcheck e em todos os testes unitários conforme shunit2?
* [ ] O código usa caminhos relativos ou caminhos absolutos? Caminhos relativos devem ser evitados, pois são vulneráveis a ataques de ambiente. Se for necessário um caminho relativo, verifique se a variável `PATH` está definida.
* [ ] O código aceita credenciais como entrada do usuário? As credenciais estão ocultas ou criptografadas no script?
