# Pipeline de Ciência de Dados

Como o Azure DevOps não permite que revisores de código comentem diretamente em Notebooks Jupyter, os Cientistas de Dados (DSs) precisam converter os notebooks em scripts antes de fazerem commit e enviar esses arquivos para o repositório.

Este documento tem como objetivo automatizar esse processo no Azure DevOps, para que os DSs não precisem executar nada localmente.

## Declaração do Problema

Um repositório de Ciência de Dados possui a seguinte estrutura de pastas:

```bash
.
├── notebooks
│   ├── Experimentos de Aprendizado de Máquina - 00.ipynb
│   ├── Experimentos de Aprendizado de Máquina - 01.ipynb
│   ├── Experimentos de Aprendizado de Máquina - 02.ipynb
│   ├── Experimentos de Aprendizado de Máquina - 03.ipynb
└── scripts
    ├── Experimentos de Aprendizado de Máquina - 00.py
    ├── Experimentos de Aprendizado de Máquina - 01.py
    ├── Experimentos de Aprendizado de Máquina - 02.py
    └── Experimentos de Aprendizado de Máquina - 03.py
```

Os arquivos Python são necessários para permitir que revisores de Pull Request adicionem comentários aos notebooks. Eles podem adicionar comentários aos scripts em Python e aplicamos esses comentários aos notebooks.

Como temos que executar este processo manualmente antes de adicionar arquivos a um commit, esse processo manual é propenso a erros. Por exemplo, se criarmos um notebook, gerarmos o script a partir dele, mas depois fizermos algumas alterações e esquecermos de gerar um novo script para as alterações.

## Solução

Uma maneira de evitar isso é criar os scripts no repositório a partir do commit. Este documento descreverá esse processo.

Podemos adicionar um pipeline com as seguintes etapas ao repositório para executar em arquivos `ipynb`:

1. Acesse as *Configurações do Projeto* -> *Repositórios* -> *Segurança* -> *Permissões de Usuário*.
2. Adicione o *Serviço de Build* nas permissões dos *Usuários* para *Contribuir*.
3. Crie um novo pipeline.

No pipeline recém-criado, adicione:

1. Desencadeador para executar em arquivos ipynb:

```yml
trigger:
  paths:
  include:
    - '*.ipynb'
    - '**/*.ipynb'
```

2. Defina o ambiente como Linux:

```yml
pool:
  vmImage: ubuntu-latest
```

3. Configure o diretório onde deseja armazenar os scripts:

```yml
variables:
  REPO_URL: # URL do Azure DevOps no formato: dev.azure.com/<Organização>/<Projeto>/_git/<NomeRepo>
```

4. Agora, iniciaremos o núcleo do pipeline:
    1. Atualize o pip:

```yml
- script: |
    python -m pip install --upgrade pip
  displayName: 'Atualizar pip'
```

    2. Instale `nbconvert` e `ipython`:

```yml
- script: |
    pip install nbconvert ipython
  displayName: 'Instalar nbconvert e ipython'
```

    3. Instale o `pandoc`:

```yml
- script: |
    sudo apt install -y pandoc
  displayName: "Instalar o pandoc"
```

    4. Encontre os arquivos de notebook (`ipynb`) no último commit do repositório e converta-os em scripts (`py`):

```yml
- task: Bash@3
    inputs:
      targetType: 'inline'
      script: |
        IPYNB_PATH=($(git diff-tree --no-commit-id --name-only -r $(Build.SourceVersion) | grep '[.]ipynb$'))
        echo $IPYNB_PATH
        [ -z "$IPYNB_PATH" ] && echo "Nada para converter" || jupyter nbconvert --to script $IPYNB_PATH
    displayName: "Converter Notebook em script"
```

    5. Faça commit dessas alterações no repositório:

```yml
- bash: |
    git config --global user.email "build@dev.azure.com"
    git config --global user.name "build"
    git add .
    git commit -m 'Converter notebooks Jupyter' || echo "Sem alterações para fazer commit" && NO_CHANGES=1
    [ -z "$NO_CHANGES" ] || git push https://$(System.AccessToken)@$(REPO_URL) HEAD:$(Build.SourceBranchName)
  displayName: "Commit de notebook para o repositório"
```

Agora temos um pipeline que gerará os scripts à medida que fazemos commit em nossos notebooks.
