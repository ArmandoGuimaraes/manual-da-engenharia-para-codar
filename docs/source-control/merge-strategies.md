# Estratégias de Mesclagem

Decida se você deseja uma história de commits linear ou não linear. Existem prós e contras para ambas as abordagens:

* Pró Linear: [Evite histórico de Git bagunçado, use histórico linear](https://dev.to/bladesensei/avoid-messy-git-history-3g26)
* Contra Linear: [Por que você deve parar de usar Git rebase](https://medium.com/@fredrikmorken/why-you-should-stop-using-git-rebase-5552bee4fed1)

## Abordagem para História de Commits Não Linear

Mesclando `topic` em `main`

```md
  A---B---C topic
 /         \
D---E---F---G---H main

git fetch origin
git checkout main
git merge topic
```

## Duas abordagens para obter uma história de commits linear

### Rebase na branch `topic` antes de mesclar com `main`

Antes de mesclar `topic` em `main`, fazemos um rebase de `topic` com a branch `main`:

```bash
          A---B---C topic
         /         \
D---E---F-----------G---H main

git checkout main
git pull
git checkout topic
git rebase origin/main
```

Crie um PR `topic` --> `main` no Azure DevOps e aprove usando a opção de mesclagem "squash".

### Rebase na branch `topic` antes de fazer um squash merge com `main`

[O merge de squash](https://learn.microsoft.com/en-us/azure/devops/repos/git/merging-with-squash?view=azure-devops) é uma opção de mesclagem que permite condensar o histórico Git das branches de tópicos quando você conclui uma solicitação pull. Em vez de adicionar cada commit em `topic` ao histórico de `main`, um squash merge pega todas as alterações de arquivo e as adiciona a um único novo commit em `main`.

```bash
          A---B---C topic
         /
D---E---F-----------G---H main
```

Crie um PR `topic` --> `main` no Azure DevOps e aprove usando a opção de mesclagem "squash".
