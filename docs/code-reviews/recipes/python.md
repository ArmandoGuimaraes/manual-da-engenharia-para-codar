# Revisão de Código em Python

## Guia de Estilo

Os desenvolvedores devem seguir o [guia de estilo PEP8](https://pep8.org/) com [dicas de tipo](https://www.python.org/dev/peps/pep-0484/). O uso de dicas de tipo em conjunto com a lintagem e verificação de dicas de tipo evita erros comuns que são difíceis de depurar.

Projetos devem verificar o código Python com ferramentas automatizadas.

A lintagem deve ser adicionada à validação de build, e tanto a lintagem quanto a formatação de código podem ser adicionadas aos seus ganchos de pré-compromisso e ao VS Code.

## Análise de Código / Lintagem

Os dois linters Python mais populares são o [Pylint](https://pypi.org/project/pylint/) e o [Flake8](https://pypi.org/project/flake8/). Ambos verificam a aderência ao `PEP8`, mas variam um pouco no que diz respeito a outras regras que verificam. Em geral, o `Pylint` tende a ser um pouco mais rigoroso e pode gerar mais falsos positivos, mas ambos são boas opções para a lintagem de código Python.

Tanto o `Pylint` quanto o `Flake8` podem ser configurados no VS Code usando a extensão Python do VS Code.

### Flake8

O Flake8 é um wrapper simples e rápido em torno do [`Pyflakes`](https://github.com/PyCQA/pyflakes) (para detecção de erros de codificação) e [`pycodestyle`](https://github.com/PyCQA/pycodestyle) (para o `PEP8`).

Instale o `Flake8`

```bash
pip install flake8
```

Adicione uma extensão para a ferramenta [`pydocstyle`](https://github.com/PyCQA/pydocstyle) (para [strings de documentação](https://www.python.org/dev/peps/pep-0257/)) ao `Flake8`.

```bash
pip install flake8-docstrings
```

Adicione uma extensão para o [`pep8-naming`](https://github.com/PyCQA/pep8-naming) (para [convenções de nomenclatura](https://www.python.org/dev/peps/pep-0008/#naming-conventions) no `PEP8`) ao `Flake8`.

```bash
pip install pep8-naming
```

Execute o `Flake8`

```bash
flake8 .    # Lintagem do projeto inteiro
```

### Pylint

Instale o `Pylint`

```bash
pip install pylint
```

Execute o `Pylint`

```bash
pylint src  # Lintagem do diretório de código-fonte
```

## Formatação Automática de Código

### Black

O [`Black`](https://github.com/psf/black) é uma ferramenta de formatação de código sem desculpas. Ele remove todas as reclamações do `pycodestyle` sobre formatação, para que a equipe possa se concentrar no conteúdo em vez do estilo. Não é possível configurar o Black de acordo com suas próprias necessidades de estilo.

```bash
pip install black
```

Formate o código Python

```bash
black [arquivo/pasta]
```

### Autopep8

O [`Autopep8`](https://github.com/hhatto/autopep8) é mais flexível e permite mais configuração se você deseja uma formatação menos rigorosa.

```bash
pip install autopep8
```

Formate o código Python

```bash
autopep8 [arquivo/pasta] --in-place
```

### yapf

[yapf](https://github.com/google/yapf) Yet Another Python Formatter é um formatador Python da Google baseado em ideias do gofmt. Ele também é mais configurável e uma boa opção para formatação automática de código.

```bash
pip install yapf
```

Formate o código Python

```bash
yapf [arquivo/pasta] --in-place
```

## Extensões do VS Code

### Python

A [extensão da linguagem Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python) é a extensão base que você deve ter instalada para desenvolvimento Python com o VS Code. Ela permite intellisense, depuração, lintagem (com os linters mencionados acima), testes com pytest ou unittest e formatação de código com os formatadores mencionados acima.

### Pyright

A [extensão Pyright](https://marketplace.visualstudio.com/items?itemName=ms-pyright.pyright) aprimora o VS Code com verificação estática de tipos quando você usa dicas de tipo.

```python
def add(primeiro_valor: int, segundo_valor: int) -> int:
    return primeiro_valor + segundo_valor
```

## Validação de Build

Para automatizar a lintagem com o `flake8` e os testes com o `pytest` no Azure Devops, você pode adicionar o seguinte trecho ao seu arquivo `azure-pipelines.yaml`.

```yaml
trigger:
  branches:
    include:
    - develop
    - master
  paths:
    include:
    - src/*

pool:
  vmImage: 'ubuntu-latest'

jobs:
- job: LintAndTest
  displayName: Lintagem e Teste

  steps:

  - checkout: self
    lfs: true

  - task: UsePythonVersion@0
    displayName: 'Definir a versão do Python para 3.6'
    inputs:
      versionSpec: '3.6'

  - script: pip3 install --user -r requirements.txt
    displayName: 'Instalar dependências'

  - script: |
      # Instalar o Flake8
      pip3 install --user flake8
      # Instalar o PyTest
      pip3 install --user pytest
    displayName: 'Instalar o Flake8 e o PyTest'

  - script: |
      python3 -m flake8
    displayName: 'Executar o linter Flake8'

  - script: |
      # Executar o testador PyTest
      python3 -m pytest --junitxml=./test-results.xml
    displayName: 'Executar o testador PyTest'

  - task: PublishTestResults@2
    displayName: 'Publicar resultados do PyTest'
    condition: succeededOrFailed()
    inputs:
      testResultsFiles: '**/test-*.xml'
      testRunTitle: 'Publicar resultados do teste para Python $(python.version)'
```

Para realizar uma validação de PR no GitHub, você pode usar uma configuração YAML semelhante com [GitHub Actions](https://help.github.com/en/actions/language-and-framework-guides/using-python-with-github-actions).

## Pre-commit hooks

Os Pre-commit hooks permitem que você formate e lint o código localmente antes de enviar a solicitação de pull.

Adicionar Pre-commit hookso para o seu repositório Python é fácil usando o pacote pre-commit.

1. Instale o pre-commit e adicione-o ao requirements.txt

```bash
pip install pre-commit
```

2. Adicione um arquivo `.pre-commit-config.yaml` na raiz do repositório, com as ações de pré-compromisso desejadas

```yaml
repos:
-   repo: https://github.com/ambv/black
    rev: stable
    hooks:
    - id: black
    language_version: python3.6
-   repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v1.2.3
    hooks:
    - id: flake8
```

3. Cada desenvolvedor individual que deseja configurar ganchos de pré-compromisso pode então executar

```bash
pre-commit install
```

Na próxima tentativa de commit, quaisquer falhas de lint bloquearão o commit.

> Observação: A instalação de ganchos de pré-compromisso é voluntária e feita por cada desenvolvedor individualmente. Portanto, não substitui a validação de build no servidor.

## Lista de Verificação de Revisão de Código

Além da [Lista de Verificação de Revisão de Código](../process-guidance/reviewer-guidance.md), você também deve procurar por estes itens específicos de revisão de código em Python:

* [ ] Todos os novos pacotes usados estão incluídos no requirements.txt?
* [ ] O código passa em todas as verificações de lint?
* [ ] As funções usam dicas de tipo e há erros de dica de tipo?
* [ ] O código é legível e utiliza construções pythonicas sempre que possível.
