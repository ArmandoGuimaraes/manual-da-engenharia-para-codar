# Variáveis de Tempo de Execução no GitHub Actions

## Objetivo

Embora o GitHub Actions seja uma escolha popular para escrever e executar pipelines de CI/CD, especialmente para projetos de código aberto hospedados no GitHub, ele carece de recursos específicos de qualidade de vida encontrados em outros ambientes de CI/CD. Um recurso-chave que o GitHub Actions ainda não implementou é a capacidade de **simular e injetar variáveis de tempo de execução** em um fluxo de trabalho, a fim de testar o pipeline em si.

Isso cria uma ponte entre um recurso existente no Azure DevOps e um recurso que ainda não foi lançado no GitHub Actions.

## Público-Alvo

Este guia assume que você está familiarizado com CI/CD e compreende as implicações de segurança das pipelines de CI/CD. Também pressupõe conhecimento básico do GitHub Actions, incluindo como escrever e executar uma pipeline de CI/CD básica, fazer checkout de repositórios dentro da ação, usar Ações do Marketplace com controle de versão, etc.

Assumimos que você, como engenheiro de CI/CD, deseja injetar variáveis de ambiente ou flags de ambiente em suas pipelines e fluxos de trabalho para testá-los e está usando o GitHub Actions para fazer isso.

## Cenário de Uso

Muitos fluxos de trabalho de integração ou de ponta a ponta requerem variáveis de ambiente específicas que só estão disponíveis durante a execução. Por exemplo, um fluxo de trabalho pode estar fazendo o seguinte:

![Diagrama do Fluxo de Trabalho](images/workflow-diagram.png)

Nessa situação, testar o pipeline é extremamente difícil sem a necessidade de fazer chamadas externas para o recurso. Em muitos casos, fazer chamadas externas ao recurso pode ser caro ou demorado, retardando significativamente o desenvolvimento em loop interno.

O Azure DevOps, como exemplo, oferece uma maneira de definir variáveis de pipeline em um acionador manual:

![Exemplo do AzDo](images/AzDoExample.PNG)

O GitHub Actions ainda não faz isso.

## Solução

Para contornar isso, a solução mais simples é adicionar variáveis de tempo de execução às mensagens de commit ou ao corpo do PR e usar o `grep` para encontrar a variável. O GitHub Actions fornece a funcionalidade de `grep` nativamente usando uma função `contains`, que é o que estaremos usando especificamente.

Dentro do escopo:

- Vamos limitar isso à injeção de uma única variável de ambiente em uma pipeline, com uma chave e valor previamente conhecidos.

Fora do escopo:

- Embora a solução seja obviamente extensível usando scripts de shell ou qualquer outro meio de criar variáveis, essa solução serve bem como prova do conceito básico. Nenhum script desse tipo é fornecido neste guia.
- Além disso, as equipes podem desejar formalizar esse processo usando um Modelo de PR que tenha uma seção adicional para as variáveis fornecidas. No entanto, isso não está incluído neste guia.

> Aviso de Segurança:  
> **Isso NÃO é para injetar segredos**, pois as mensagens de commit e o corpo do PR podem ser recuperados por terceiros, são armazenados no `git log` e podem ser lidos por um indivíduo mal-intencionado usando várias ferramentas. Em vez disso, isso é para testar um fluxo de trabalho que precisa de variáveis simples para serem injetadas, conforme descrito acima.  
> **Se você precisa recuperar segredos ou informações sensíveis**, use a [Ação do GitHub para o Azure Key Vault](https://github.com/marketplace/actions/get-secrets-from-azure-key-vault) ou algum outro serviço de armazenamento e recuperação de segredos semelhante.

## Variáveis de Mensagem de Commit

Como injetar uma única variável no ambiente para uso, com uma **chave e valor especificados**. Neste exemplo, a chave é `COMMIT_VAR` e o valor é `[commit var]`.

Pré-requisitos:

- Os acionadores da pipeline estão configurados corretamente para serem acionados por commits enviados (aqui usaremos `actions-test-branch` como a branch de escolha).

Trecho de Código:

```yaml
on:
  push:
    branches:
      - actions-test-branch

jobs:
  Echo-On-Commit:
    runs-on: ubuntu-latest
    steps:
      - name: "Checkout do Repositório"
        uses: actions/checkout@v2

      - name: "Definir sinalizador a partir do Commit"
        env:
          COMMIT_VAR: ${{ contains(github.event.head_commit.message, '[commit var]') }}
        run: |
          if ${COMMIT_VAR} == true; then
            echo "flag=true" >> $GITHUB_ENV
            echo "sinalizador definido como verdadeiro"
          else
            echo "flag=false" >> $GITHUB_ENV
            echo "sinalizador definido como falso"
          fi

      - name: "Usar sinalizador se verdadeiro"
        if: env.flag
        run: echo "O sinalizador está disponível e é verdadeiro"
```

Disponível como um arquivo .YAML [aqui](examples/commit-example.yaml).

Explicação do Código:

A primeira parte do código configura os acionadores de Push na branch de trabalho e faz o checkout do repositório, então não exploraremos isso em detalhes.

```yaml
- name: "Definir sinalizador a partir do Commit"
  env:
    COMMIT_VAR: ${{ contains(github.event.head_commit.message, '[commit var]') }}
```

Esta é uma etapa nomeada dentro do único Job em nossa pipeline do GitHub Actions. Aqui, definimos uma variável de ambiente para a etapa: qualquer código ou ação que a etapa chamar agora terá a variável de ambiente disponível.

`contains` é uma função do GitHub Actions que está disponível por padrão em todos os fluxos de trabalho. Ela retorna um valor booleano `true` ou `false`. Nesta situação, ela verifica se a mensagem de commit no último envio, acessada por `github.event.head_commit.message`, contém a string `${{...}}` é necessário para usar o Contexto do GitHub e tornar as funções e as variáveis `github.event` disponíveis para o comando.

```yaml
run: |
  if ${COMMIT_VAR} == true; then
    echo "flag=true" >> $GITHUB_ENV
    echo "sinalizador definido como verdadeiro"
  else
    echo "flag=false" >> $GITHUB_ENV
    echo "sinalizador definido como

 falso"
  fi
```

O comando `run` aqui verifica se a variável `COMMIT_VAR` foi definida como `true` e, se for o caso, define uma segunda flag como verdadeira e faz um echo disso. Ele faz o mesmo se a variável for `false`.

A razão específica para fazer isso é permitir que a variável `flag` seja usada em etapas posteriores em vez de ter que reutilizar a `COMMIT_VAR` em cada etapa. Além disso, permite que a flag seja usada na etapa `if` de uma ação, como na próxima parte do trecho de código.

```yaml
- name: "Usar sinalizador se verdadeiro"
  if: env.flag
  run: echo "O sinalizador está disponível e é verdadeiro"
```

Nesta parte do trecho, a próxima etapa no mesmo job agora está usando o `flag` que foi definido na etapa anterior. Isso permite ao usuário:

1. Reutilizar o sinalizador em vez de acessar repetidamente o Contexto do GitHub
2. Definir o sinalizador usando várias condições, em vez de apenas uma. Por exemplo, uma etapa diferente também pode definir o sinalizador como `true` ou `false` por diferentes motivos.
3. Alterar a variável em um único local em vez de ter que alterá-la em vários lugares

Alternativa mais curta:

A etapa "Definir sinalizador a partir do Commit" pode ser simplificada da seguinte forma para tornar o código muito mais curto, embora não necessariamente mais legível:

```yaml
- name: "Definir sinalizador a partir do Commit"
  env:
    COMMIT_VAR: ${{ contains(github.event.head_commit.message, '[commit var]') }}
  run: |
  echo "flag=${COMMIT_VAR}" >> $GITHUB_ENV
  echo "sinalizador definido como ${COMMIT_VAR}"
```

Uso:

Incluindo a Variável

1. Faça push na branch `master`:

   ```cmd
   > git add.
   > git commit -m "Running GitHub Actions Test [commit var]"
   > git push
   ```

2. Isso aciona o fluxo de trabalho (como qualquer push). Como `[commit var]` está na mensagem de commit, a variável `${COMMIT_VAR}` no fluxo de trabalho será definida como `true` e resultará no seguinte:

   ![Cenário de Verdadeiro no Commit](images/CommitTrue.PNG)

Não Incluindo a Variável

1. Faça push na branch `master`:

   ```cmd
   > git add.
   > git commit -m "Running GitHub Actions Test"
   > git push
   ```

2. Isso aciona o fluxo de trabalho (como qualquer push). Como `[commit var]` **não** está na mensagem de commit, a variável `${COMMIT_VAR}` no fluxo de trabalho será definida como `false` e resultará no seguinte:

   ![Cenário de Falso no Commit](images/CommitFalse.PNG)

## Variáveis no Corpo do PR

Quando um PR é feito, o Corpo do PR também pode ser usado para configurar variáveis. Essas variáveis podem estar disponíveis para todas as execuções de fluxo de trabalho que derivam desse PR, o que pode ajudar a garantir que as mensagens de commit sejam mais informativas e menos poluídas, além de reduzir o trabalho do desenvolvedor.

Mais uma vez, isso é para uma chave e valor esperados. Neste caso, a chave é `PR_VAR` e o valor é `[pr var]`.

Pré-requisitos:

- Os acionadores da pipeline estão configurados corretamente para acionar um pull request para uma branch específica. (Aqui usaremos `master` como a branch de destino.)

Trecho de Código:

```yaml
on:
  pull_request:
    branches:
      - master

jobs:
  Echo-On-PR:
    runs-on: ubuntu-latest
    steps:
      - name: "Checkout do Repositório"
        uses: actions/checkout@v2

      - name: "Definir sinalizador a partir do PR"
        env:
          PR_VAR: ${{ contains(github.event.pull_request.body, '[pr var]') }}
        run: |
          if ${PR_VAR} == true; then
            echo "flag=true" >> $GITHUB_ENV
            echo "sinalizador definido como verdadeiro"
          else
            echo "flag=false" >> $GITHUB_ENV
            echo "sinalizador definido como falso"
          fi

      - name: "Usar sinalizador se verdadeiro"
        if: env.flag
        run: echo "O sinalizador está disponível e é verdadeiro"
```

Disponível como um arquivo .YAML [aqui](examples/pr-example.yaml).

Explicação do Código:

A primeira parte do arquivo YAML simplesmente configura o Acionador de Pull Request. A maior parte do código a seguir é idêntica, então explicaremos apenas as diferenças.

```yaml
- name: "Definir sinalizador a partir do PR"
  env:
    PR_VAR: ${{ contains(github.event.pull_request.body, '[pr var]') }}
```

Nesta seção, a variável de ambiente `PR_VAR` é definida como `true` ou `false`, dependendo se a string `[pr var]` está no Corpo do PR.

Alternativa mais curta:

Da mesma forma que o acima, a etapa YAML pode ser simplificada da seguinte forma para tornar o código muito mais curto, embora não necessariamente mais legível:

```yaml
- name: "Definir sinalizador a partir do PR"
  env:
    PR_VAR: ${{ contains(github.event.pull_request.body, '[pr var]') }}
  run: |
  echo "flag=${PR_VAR}" >> $GITHUB_ENV
  echo "

sinalizador definido como ${PR_VAR}"
```

Uso:

1. Crie um Pull Request para `master` e inclua a variável esperada em algum lugar do corpo:

   ![Exemplo de PR](images/PRExample.PNG)

2. A Ação do GitHub será acionada automaticamente e, como `[pr var]` está presente no Corpo do PR, ela definirá o `flag` como verdadeiro, como mostrado abaixo:

   ![PR Verdadeiro](images/PRTrue.PNG)

## Cenários do Mundo Real

Existem muitos cenários do mundo real em que o controle de variáveis de ambiente pode ser extremamente útil. Alguns são destacados abaixo:

### Evitar Chamadas Externas Caras

O Desenvolvedor A está escrevendo e testando um pipeline de integração. O pipeline de integração precisa fazer uma chamada a um serviço externo, como o Azure Data Factory ou o Databricks, aguardar um resultado e depois exibir esse resultado. O fluxo de trabalho pode ser semelhante a este:

![Fluxo de Trabalho A](images/DevAWorkflow.png)

O fluxo de trabalho inerentemente leva tempo e é caro de ser executado, pois envolve a manutenção de um cluster Databricks e a espera pela resposta. Essa dependência externa pode ser removida essencialmente simulando a resposta durante a escrita e teste de outras partes do fluxo de trabalho e simulando a resposta em situações em que a resposta real não importa ou não está sendo testada diretamente.

### Pular Processos de CI Longos

O Desenvolvedor B está escrevendo e testando uma pipeline de CI/CD. A pipeline tem várias etapas de CI, cada uma das quais é executada sequencialmente. O fluxo de trabalho pode ser assim:

![Fluxo de Trabalho B](images/DevBWorkflow.png)

Neste caso, cada etapa de CI precisa ser executada antes que a próxima comece, e erros no meio do processo podem fazer com que a pipeline inteira falhe. Embora esse possa ser um comportamento pretendido para a pipeline em algumas situações (talvez você não queira executar uma construção mais envolvente ou uma suíte de cobertura de testes demorada se o processo de CI estiver falhando), significa que as etapas precisam ser comentadas ou excluídas ao testar a própria pipeline.

Em vez disso, uma etapa adicional poderia verificar a presença de uma tag `[skip ci $N]` nas mensagens de commit ou no Corpo do PR e pular uma etapa específica da compilação do CI. Isso garante que a pipeline final não tenha alterações confirmadas que a tornem quebrada, como acontece às vezes ao comentar/excluir etapas. Além disso, permite um mecanismo para testar repetidamente etapas individuais pulando as outras, o que facilita significativamente o desenvolvimento da pipeline.
