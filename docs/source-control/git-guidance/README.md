# Orientações sobre o Git

## O que é o Git?

O Git é um sistema de controle de versão distribuído. Isso significa que, ao contrário do SVN ou CVS, ele não utiliza um servidor central para sincronização. Em vez disso, cada participante possui uma cópia local do código-fonte e do histórico associado, que é mantido sincronizado comparando os hashes de commit (hashes SHA das alterações entre cada comando git commit) que compõem a versão mais recente (chamada `HEAD`).

Por exemplo:

```plain
repo 1: A -> B -> C -> D -> HEAD
repo 2: A -> B -> HEAD
repo 3: X -> Y -> Z -> HEAD
repo 4: A -> J -> HEAD
```

Como eles compartilham um histórico comum, repo 1 e repo 2 podem ser sincronizados com relativa facilidade, repo 4 _pode_ ser capaz de sincronizar também, mas terá que adicionar um commit (J, e talvez um commit de merge) ao repo 1. Repo 3 não pode ser facilmente sincronizado com os outros. Tudo relacionado a esses commits é armazenado em um diretório .git local na raiz do repositório.

Em outras palavras, ao usar o Git, você está criando simplesmente históricos de arquivos imutáveis que identificam de forma única o estado atual e, portanto, permitem compartilhar o que vem depois. É uma [árvore Merkle](https://en.wikipedia.org/wiki/Merkle_tree).

Certifique-se de executar `git help` após a instalação do Git para encontrar explicações detalhadas de tudo.

## Instalação

O Git é uma ferramenta que deve ser instalada. [Instale o Git](https://git-scm.com/downloads) e siga a [Configuração do Git pela Primeira Vez](https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup).

Uma instalação recomendada é a [extensão Git Lens para o Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens). Visualize a autoria do código em um piscar de olhos por meio de anotações de autoria do Git e lentes de código, navegue e explore repositórios do Git de forma contínua, obtenha insights valiosos por meio de comandos de comparação poderosos e muito mais.

Você também pode usar esses comandos para configurar seu Git para o Visual Studio Code como um editor para conflitos de mesclagem e ferramenta de diferença.

```cmd
git config --global user.name [SEU NOME E SOBRENOME]
git config --global user.email [SEU ENDEREÇO DE E-MAIL]

git config --global merge.tool vscode
git config --global mergetool.vscode.cmd "code --wait $MERGED"

git config --global diff.tool vscode
git config --global difftool.vscode.cmd "code --wait --diff $LOCAL $REMOTE"
```

## Fluxo de trabalho básico

Um fluxo de trabalho básico com o Git é o seguinte; você pode encontrar mais informações sobre as etapas específicas abaixo.

```cmd
# puxar as últimas alterações
git pull

# iniciar um novo branch de funcionalidade com base no branch develop
git checkout -b feature/123-adicionar-instrucoes-git develop

# editar alguns arquivos

# adicionar e fazer commit dos arquivos
git add <arquivo>
git commit -m "adicionar instruções básicas"

# editar alguns arquivos

# adicionar e fazer commit dos arquivos
git add <arquivo>
git commit -m "adicionar instruções mais avançadas"

# verificar suas alterações
git status

# enviar o branch para o repositório remoto
git push --set-upstream origin feature/123-adicionar-instrucoes-git
```

### Clonagem

Sempre que você quiser fazer uma alteração em um repositório, você precisa cloná-lo primeiro. Clonar um repositório puxa uma cópia completa de todos os dados do repositório, para que você possa trabalhar nele localmente. Esta cópia inclui todas as versões de cada arquivo e pasta do projeto.

```cmd
git clone https://github.com/username/repo-name
```

Você só precisa clonar o repositório na primeira vez. Antes de qualquer ramificação subsequente, você pode sincronizar quaisquer alterações do repositório remoto usando `git pull`.

### Ramificação

Para evitar adicionar código que não foi revisado por pares ao branch principal (por exemplo, `develop`), geralmente trabalhamos em branches de funcionalidade e mesclamos esses branches de volta ao tronco principal com um Pull Request. Às vezes, o branch `main` ou `develop` de um repositório é bloqueado para que você não possa fazer alterações sem um Pull Request. Portanto, é útil criar um branch separado para o seu trabalho local/da funcionalidade, para que você possa trabalhar e rastrear suas alterações neste branch.

Puxe as últimas alterações e crie um novo branch para o seu trabalho com base no tronco (neste caso, `develop`).

```cmd
git pull
git checkout -b feature/nome-da-funcionalidade develop
```

A qualquer momento, você pode alternar entre os branches com `git checkout <branch>` desde que você tenha feito commit ou ocultado seu trabalho. Se você esquecer o nome do seu branch, use `git branch --all` para listar todos os branches.

### Commit

Para evitar perder trabalho, é bom fazer commits frequentes em pequenos pedaços. Isso permite reverter apenas as últimas alterações se você descobrir um problema e também explica claramente quais alterações foram feitas e por quê.

1. Faça alterações no seu branch.

2. Verifique quais arquivos foram alterados

    ```cmd
    > git status
    On branch feature/271-basic-commit-info
    Changes not staged for commit:
      (use "git add <file>..." to update what will be committed)
      (use "git restore <file>..." to discard changes in working directory)
            modified:   source-control/git-guidance/README.md
    ```

3. Rastreie os arquivos que deseja incluir no commit. Para rastrear todos os arquivos modificados:

    ```cmd
    git add --all
    ```

   Ou para rastrear apenas arquivos específicos:

    ```cmd
    git add source-control/git-guidance/README.md
    ```

4. Faça o commit das alterações no seu branch local com uma mensagem de commit descritiva [práticas recomendadas para commits](

#práticas-recomendadas-para-commits).

    ```cmd
    git commit -m "adicionar instruções básicas do Git"
    ```

### Envio (Push)

Quando você terminar de trabalhar, envie suas alterações para um branch no repositório remoto usando:

```cmd
git push
```

Na primeira vez que você enviar, será necessário definir um branch upstream da seguinte forma. Após o primeiro envio, o parâmetro --set-upstream e o nome do branch não serão mais necessários.

```cmd
git push --set-upstream origin feature/nome-da-funcionalidade
```

Depois que o branch da funcionalidade for enviado para o repositório remoto, ele será visível para qualquer pessoa com acesso ao código.

### Mesclagem

Encorajamos o uso de Pull Request (PR) para mesclar código no repositório principal, para garantir que todo o código no produto final seja [revisado por código](../../code-reviews/README.md).

O processo de Pull Request no [Azure DevOps](https://learn.microsoft.com/en-us/azure/devops/repos/git/pull-requests?view=azure-devops), [GitHub](https://docs.github.com/en/github/collaborating-with-issues-and-pull-requests/creating-a-pull-request) e outras ferramentas semelhantes torna fácil iniciar um PR, revisar um PR e mesclar um PR.

#### Conflitos de Mesclagem

Se várias pessoas fizerem alterações nos mesmos arquivos, você pode precisar resolver quaisquer conflitos que tenham ocorrido antes de poder mesclar.

```cmd
# verificar o branch develop e obter as últimas alterações
git checkout develop
git pull

# verificar seu branch
git checkout <seu branch>

# mesclar o branch develop no seu branch
git merge develop

# se ocorrerem conflitos de mesclagem, o comando acima falhará com uma mensagem informando que existem conflitos a serem resolvidos

# descubra quais arquivos precisam ser resolvidos
git status
```

Você pode iniciar um processo interativo que mostrará quais arquivos têm conflitos. Às vezes, você removeu um arquivo onde ele foi alterado no branch develop. Ou você fez alterações em algumas linhas em um arquivo onde outro desenvolvedor também fez alterações. Se você seguiu as etapas de instalação mencionadas antes, o Visual Studio Code está configurado como uma ferramenta de mesclagem. Você também pode usar uma ferramenta de mesclagem como o [kdiff3](https://github.com/KDE/kdiff3). Quando ocorrem conflitos de edição, o processo abrirá automaticamente o Visual Studio Code, onde as partes conflitantes são destacadas em verde e azul, e você deve fazer uma escolha:

* Aceitar suas alterações (atuais)
* Aceitar as alterações do branch develop (entrantes)
* Aceitar ambos e corrigir o código (provavelmente necessário)

```text
Aqui estão as linhas que não foram alteradas desde o ancestral comum
<<<<<<< yours:sample.txt
A resolução de conflitos é difícil;
vamos fazer compras.
=======
O Git torna a resolução de conflitos fácil.
>>>>>>> theirs:sample.txt
E aqui está outra linha que foi resolvida ou não foi modificada.
```

Quando esse processo for concluído, verifique se você testou o resultado executando compilação, verificações e testes para validar esse resultado mesclado.

```cmd
# concluir a mesclagem
git merge --continue

# verifique se tudo correu bem
git log

# envie as alterações para o branch remoto
git push
```

Se nenhum outro conflito aparecer, o PR pode ser mesclado e seu branch pode ser excluído. Use `squash` para reduzir suas alterações a um único commit, para que o histórico de commits fique em um tamanho aceitável.

### Ocultando Alterações

`git stash` é muito útil se você tiver alterações não comprometidas em seu diretório de trabalho, mas quiser trabalhar em um branch diferente. Você pode executar `git stash`, salvar o trabalho não comprometido e reverter para o commit HEAD. Você pode recuperar as alterações salvas executando `git stash pop`:

```cmd
git stash
…
git stash pop
```

Ou você pode mover o estado atual para um novo branch:

```cmd
git stash branch <novo_branch_para_salvar_as_alterações>
```

### Recuperando Commits Perdidos

Se você "perdeu" um commit ao qual deseja retornar, por exemplo, para reverter um `git rebase` no qual seus commits foram agrupados, você pode usar `git reflog` para encontrar o commit:

```cmd
git reflog
```

Em seguida, você pode usar a referência reflog (`HEAD@{}`) para redefinir para um commit específico antes do

 rebase:

```cmd
git reset HEAD@{2}
```

## Práticas Recomendadas para Commits

Um commit combina alterações em uma unidade lógica. Adicionar uma mensagem de commit descritiva pode ajudar a compreender as alterações no código e entender a justificativa das modificações. Considere o seguinte ao fazer seus commits:

* Faça commits pequenos. Isso torna as alterações mais fáceis de revisar e, se precisarmos reverter um commit, perdemos menos trabalho. Considere dividir o commit em commits separados com `git add -p` se ele incluir mais de uma alteração lógica ou correção de bug.
* Não misture alterações de espaçamento em branco com alterações de código funcional. É difícil determinar se a linha tem uma alteração funcional ou apenas remove espaçamento em branco, então alterações funcionais podem passar despercebidas.
* Faça commit de código completo e bem testado. Nunca faça commit de código incompleto; adquira o hábito de testar seu código antes de fazer commit.
* Escreva boas mensagens de commit.
  * Por que é necessário? Pode corrigir um bug, adicionar uma funcionalidade, melhorar o desempenho ou apenas ser uma alteração para corrigir a correção.
  * Quais efeitos essa alteração tem? Além dos óbvios, isso pode incluir benchmarks, efeitos colaterais etc.

Você pode especificar o editor git padrão, que permite escrever suas mensagens de commit usando seu editor favorito. O seguinte comando torna o Visual Studio Code o seu editor git padrão:

```bash
git config --global core.editor "code --wait"
```

### Estrutura da Mensagem de Commit

As partes essenciais de uma mensagem de commit são:
* linha de assunto: uma breve descrição do commit, com no máximo 50 caracteres
* corpo (opcional): uma descrição mais longa do commit, com quebras de linha a cada 72 caracteres, separada da linha de assunto por uma linha em branco

Você é livre para estruturar mensagens de commit como desejar; no entanto, comandos git como `git log` utilizam a estrutura acima.
Portanto, pode ser útil seguir uma convenção dentro da sua equipe e utilizar as melhores práticas do git.

Por exemplo, [Conventional Commits](https://www.conventionalcommits.org/) é uma convenção leve que complementa o [SemVer](https://semver.org/), descrevendo as funcionalidades, correções e alterações incompatíveis feitas nas mensagens de commit. Veja [Component Versioning](../component-versioning.md) para obter mais informações sobre versionamento.

Para obter mais informações sobre convenções de mensagens de commit, consulte:

* [A Note About Git Commit Messages](https://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html)
* [Conventional Commits](https://www.conventionalcommits.org)
* [Melhores práticas para commits do Git](https://medium.com/@nawarpianist/git-commit-best-practices-dab8d722de99)
* [Como Escrever uma Mensagem de Commit do Git](https://cbea.ms/git-commit)
* [Como Escrever Melhores Mensagens de Commit do Git](https://www.freecodecamp.org/news/how-to-write-better-git-commit-messages)
* [Informações em mensagens de commit](https://wiki.openstack.org/wiki/GitCommitMessages#Information_in_commit_messages)
* [Sobre mensagens de commit](http://who-t.blogspot.com/2009/12/on-commit-messages.html)

## Gerenciando repositórios remotos

Um repositório Git local pode ter um ou mais repositórios remotos de suporte. Você pode listar os repositórios remotos usando `git remote` - por padrão, o repositório remoto do qual você fez a clonagem será chamado de origem.

```cmd
> git remote -v
origin  https://github.com/microsoft/code-with-engineering-playbook.git (fetch)
origin  https://github.com/microsoft/code-with-engineering-playbook.git (push)
```

### Trabalhando com forks

Você pode configurar vários repositórios remotos. Isso é útil, por exemplo, se você deseja trabalhar com uma versão bifurcada do repositório. Para obter mais informações sobre como definir repositórios remotos e sincronizar repositórios ao trabalhar com bifurcações, consulte a documentação [Trabalhando com forks do GitHub](https://docs.github.com/en/github/collaborating-with-issues-and-pull-requests/working-with-forks).

### Atualizando o

 repositório remoto

Acesse seu repositório local e verifique se você está na branch que deseja enviar ao repositório remoto. Por exemplo:

```cmd
> git branch
* feature/my-feature
  master
  dev
```

Suponha que você deseja enviar as alterações da `feature/my-feature` para o repositório remoto chamado `origin`. Para atualizar o repositório remoto com as alterações locais, você pode executar:

```cmd
> git push origin feature/my-feature
```

Você também pode configurar seu repositório local para definir o branch remoto padrão. Isso significa que, quando você executa `git push`, ele envia automaticamente para o branch remoto correspondente:

```cmd
> git push --set-upstream origin feature/my-feature
```

### Sincronizando com o repositório remoto

Antes de enviar as alterações para o repositório remoto, é aconselhável sincronizar suas alterações locais com as do repositório remoto. Isso ajuda a evitar conflitos de mesclagem. Use o comando `git pull` para puxar as alterações do repositório remoto para o seu branch local:

```cmd
> git pull origin feature/my-feature
```

Após sincronizar, você pode enviar suas alterações usando `git push`, conforme descrito anteriormente.

### Definindo o repositório remoto padrão

Se você deseja definir um repositório remoto padrão para o seu repositório local, para que você não precise especificá-lo ao usar comandos como `git push` e `git pull`, pode fazê-lo usando o comando `git remote`:

```cmd
> git remote set-url origin https://github.com/seu-usuario/seu-repositorio.git
```

Substitua `https://github.com/seu-usuario/seu-repositorio.git` pelo URL do seu repositório remoto.

## Resolvendo Conflitos

Conflitos ocorrem quando duas ou mais pessoas fazem alterações nas mesmas partes de um arquivo ou em arquivos conflitantes. O Git não pode determinar automaticamente qual versão do código deve ser mantida, portanto, é necessário resolver conflitos manualmente.

Ao executar um comando que cria um conflito, como `git pull` ou `git merge`, você verá mensagens indicando os conflitos e os arquivos que estão em conflito.

Para resolver conflitos, siga estas etapas:

1. Abra o arquivo em um editor de texto. Você verá marcadores de conflito que indicam as áreas em conflito, como:

   ```text
   <<<<<<< HEAD
   Este é o código do HEAD (sua versão local).
   =======
   Este é o código do branch que você está mesclando (ou pull request).
   >>>>>>> branch-que-está-sendo-mesclado
   ```

2. Edite o arquivo para manter as partes que deseja manter. Você pode remover as partes indesejadas (incluindo os marcadores de conflito).

3. Após editar o arquivo, salve-o.

4. Adicione o arquivo resolvido aos commits usando `git add <arquivo>`.

5. Faça um commit das alterações usando `git commit`.

6. Continue com o processo de mesclagem ou pull request.

É importante garantir que o arquivo esteja livre de marcadores de conflito antes de fazer o commit. Certifique-se de revisar todas as alterações e testá-las para garantir que o código ainda funcione conforme o esperado.

## Referências

Aqui estão algumas referências adicionais sobre o Git e o uso eficaz dele:

1. [Pro Git Book](https://git-scm.com/book/pt-br/v2) - Um livro abrangente sobre o Git.
2. [Atlassian Git Tutorials](https://www.atlassian.com/git) - Tutoriais detalhados da Atlassian sobre o Git.
3. [GitHub Learning Lab](https://lab.github.com/) - Lições interativas para aprender a usar o Git e o GitHub.
4. [Try Git](https://try.github.io/) - Um tutorial interativo que permite aprender o Git no navegador.
5. [Git Cheat Sheet](https://github.github.com/training-kit/downloads/pt_BR/github-git-cheat-sheet.pdf) - Um guia de referência rápida do GitHub para comandos Git.
6. [Oh Shit, Git!?!](https://ohshitgit.com/) - Um guia útil para solucionar problemas comuns com o Git.

## Gerenciamento de Remotos

Um repositório local do Git pode ter um ou mais repositórios remotos de suporte. Você pode listar os repositórios remotos usando `git remote` - por padrão, o repositório remoto do qual você clonou será chamado de origin.

```cmd
> git remote -v
origin  https://github.com/microsoft/code-with-engineering-playbook.git (fetch)
origin  https://github.com/microsoft/code-with-engineering-playbook.git (push)
```

### Trabalhando com Forks

Você pode definir vários remotos. Isso é útil, por exemplo, se você deseja trabalhar com uma versão bifurcada (forked) do repositório. Para obter mais informações sobre como configurar remotos upstream e sincronizar repositórios ao trabalhar com forks, consulte a [documentação Trabalhando com Forks do GitHub](https://docs.github.com/en/github/collaborating-with-issues-and-pull-requests/working-with-forks).

### Atualização do Remoto se um Repositório Mudar de Nome

Se o repositório mudar de alguma forma, por exemplo, uma alteração de nome, ou se você deseja alternar entre HTTPS e SSH, você precisará atualizar o remoto.

```cmd
# listar os remotos existentes
> git remote -v
origin  https://hostname/username/repository-name.git (fetch)
origin  https://hostname/username/repository-name.git (push)

# alterar a URL do remoto
git remote set-url origin https://hostname/username/new-repository-name.git

# verificar se a URL do remoto foi alterada
> git remote -v
origin  https://hostname/username/new-repository-name.git (fetch)
origin  https://hostname/username/new-repository-name.git (push)
```

## Revertendo Alterações

### Reverter e Excluir Commits

Para "desfazer" um commit, execute os seguintes dois comandos: `git revert` e `git reset`. `git revert` cria um novo commit que desfaz commits, enquanto `git reset` permite excluir commits completamente do histórico de commits.

> Se você tiver cometido segredos/chaves, `git reset` os removerá do histórico de commits!

Para **excluir** o commit mais recente, use `HEAD~`:

```bash
git reset --hard HEAD~1
```

Para excluir commits até um commit específico, use o ID de commit respectivo:

```bash
git reset --hard <ID-de-commit-sha1>
```

Depois de excluir os commits indesejados, faça push usando `force`:

```bash
git push origin HEAD --force
```

Rebase interativo para desfazer commits:

```bash
git rebase -i HEAD~N
```

O comando acima abrirá uma sessão interativa em um editor (por exemplo, vim) com os últimos N commits classificados do mais antigo para o mais recente. Para desfazer um commit, exclua a linha correspondente ao commit e salve o arquivo. O Git reescreverá os commits na ordem listada no arquivo e, como um (ou muitos) commits foram excluídos, o commit não fará mais parte do histórico.

A execução do rebase modificará localmente o histórico, após o qual você pode usar `force` para enviar as alterações para o remoto sem o commit excluído.

## Usando Submódulos

Os submódulos podem ser úteis em cenários de implantação e/ou desenvolvimento mais complexos.

Adicionando um submódulo ao seu repositório:

```bash
git submodule add -b master <seu_submódulo>
```

Inicialize e puxe um repositório com submódulos:

```bash
git submodule init
git submodule update --init --remote
git submodule foreach git checkout master
git submodule foreach git pull origin
```

## Trabalhando com Imagens, Vídeos e Outros Conteúdos Binários

Evite cometer arquivos binários frequentemente alterados, como imagens grandes, vídeos ou código compilado em seu repositório Git. O conteúdo binário não é comparado como o conteúdo de texto, portanto, clonar ou puxar do repositório pode trazer cada revisão do arquivo binário.

Uma solução para esse problema é o `Git LFS (Git Large File Storage)` - uma extensão Git de código aberto para versionar arquivos grandes. Você pode encontrar mais informações sobre o Git LFS no [documento Git LFS e VFS](git-lfs-and-vfs.md).

## Trabalhando com Repositórios Grandes

Ao trabalhar com um repositório muito grande do qual você não precisa de todos os arquivos, você pode usar o `VFS for Git` - uma extensão Git de código aberto que virtualiza o sistema de arquivos sob o seu repositório Git, para que você pareça estar trabalhando em um diretório de trabalho regular, mas o VFS for Git apenas faz o download de objetos conforme

 necessário. Você pode encontrar mais informações sobre o VFS for Git no [documento Git LFS e VFS](git-lfs-and-vfs.md).

## Ferramentas

* O Visual Studio Code é um poderoso editor de código-fonte multiplataforma com comandos Git integrados. Dentro do editor Visual Studio Code, você pode revisar diferenças, preparar alterações, fazer commits, puxar e enviar para seus repositórios Git. Você pode consultar o [Suporte Git do Visual Studio Code](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support) para obter documentação.

* Use um shell/terminal para trabalhar com comandos Git em vez de depender de [clientes GUI](https://git-scm.com/downloads/guis/).

* Se você estiver trabalhando no Windows, o [posh-git](https://github.com/dahlbyk/posh-git) é um ótimo ambiente PowerShell para o Git. Outra opção é usar o [Git bash para Windows](http://www.techoism.com/how-to-install-git-bash-on-windows/). No Linux/Mac, instale o Git e use o seu shell/terminal favorito.


Lembre-se de que o Git é uma ferramenta poderosa, mas pode ser complexa. Não se preocupe se você cometer erros no início; todos cometem. A prática constante ajudará você a se tornar um usuário mais habilidoso do Git.
