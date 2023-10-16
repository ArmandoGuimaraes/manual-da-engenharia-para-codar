# Ferramentas de Revisão de Código

## Personalizar o ADO (Azure DevOps)

### Quadros de Tarefas

- AzDO: [Personalizar cartões](https://learn.microsoft.com/pt-br/azure/devops/boards/boards/customize-cards?view=azure-devops)
- AzDO: [Adicionar colunas no quadro de tarefas](https://learn.microsoft.com/pt-br/azure/devops/boards/sprints/customize-taskboard?view=azure-devops#add-columns)

### Políticas de Revisor

- Configurar um grupo de revisores obrigatórios no AzDO - [Incluir automaticamente revisores de código](https://learn.microsoft.com/pt-br/azure/devops/repos/git/branch-policies?view=azure-devops#automatically-include-code-reviewers)

## Configurando Políticas de Ramificação

1. AzDO: [Configurar políticas de ramificação](https://learn.microsoft.com/pt-br/azure/devops/repos/git/branch-policies?view=azure-devops#configure-branch-policies)
1. AzDO: Configurar políticas de ramificação com a ferramenta CLI:
   2. [Criar um arquivo de configuração de política](https://learn.microsoft.com/pt-br/azure/devops/cli/policy-configuration-file?view=azure-devops#create-a-policy-configuration-file)
   2. [Política de contagem de aprovação](https://learn.microsoft.com/pt-br/rest/api/azure/devops/policy/configurations/create?view=azure-devops-rest-5.1#approval-count-policy)
1. GitHub: [Configurando ramificações protegidas](https://help.github.com/pt/github/administering-a-repository/about-protected-branches)

## Visual Studio Code

### GitHub: [GitHub Pull Requests](https://marketplace.visualstudio.com/items?itemName=GitHub.vscode-pull-request-github)

Suporta o processamento de pull requests do GitHub dentro do VS Code.

1. Abra o plugin na **Barra de Atividades**
1. Selecione **Atribuído a Mim**
1. Selecione um PR
1. Sob **Descrição**, você pode escolher **Fazer Check-out** do ramo e entrar no **Modo de Revisão** para obter uma experiência mais integrada

### Azure DevOps: [Azure DevOps Pull Requests](https://marketplace.visualstudio.com/items?itemName=ankitbko.vscode-pull-request-azdo)

Suporta o processamento de pull requests do Azure DevOps dentro do VS Code.

1. Abra o plugin na **Barra de Atividades**
1. Selecione **Atribuído a Mim**
1. Selecione um PR
1. Sob **Descrição**, você pode escolher **Fazer Check-out** do ramo e entrar no **Modo de Revisão** para obter uma experiência mais integrada

## Visual Studio

As seguintes extensões podem ser usadas para criar uma experiência integrada de revisão de código no Visual Studio, trabalhando com GitHub ou Azure DevOps.

### GitHub: [GitHub Extension para Visual Studio](https://marketplace.visualstudio.com/items?itemName=GitHub.GitHubExtensionforVisualStudio)

Fornece funcionalidades estendidas para trabalhar com pull requests no GitHub diretamente no Visual Studio.

1. View -> Other Windows -> GitHub
1. Clique no ícone de **Pull Requests** na barra de tarefas
1. Clique duas vezes em um pull request pendente

### Azure DevOps: [Pull Requests para Visual Studio](https://marketplace.visualstudio.com/items?itemName=VSIDEVersionControlMSFT.pr4vs)

Trabalhe com pull requests no Azure DevOps diretamente no Visual Studio.

1. Abra o Team Explorer
1. Clique em **Pull Requests**
1. Clique duas vezes em um pull request - os **Detalhes do Pull Request** serão abertos
1. Clique em **Checkout** se você quiser ter a mudança completa localmente e ter uma experiência mais integrada
1. Revise as mudanças e faça comentários

## Web

### Reviewable: [Revisões de Código GitHub multi-round sem interrupções](https://home.reviewable.io/)

Suporta revisões de código GitHub multi-round, com atalhos de teclado e muito mais. A extensão para o VS Code está em andamento.

1. Visite o [Painel de Revisões](https://reviewable.io/reviews) para ver revisões aguardando sua ação, que têm novos comentários para você e muito mais.
1. Selecione um Pull Request dessa lista.
1. Abra qualquer arquivo no seu navegador, no Visual Studio Code ou em qualquer editor que você tenha configurado, clicando na sua foto de perfil no canto superior direito.
1. Selecione um editor em "Modelo de link externo". O VS Code é uma opção, mas também qualquer editor que suporte URI.
1. Revise a diferença globalmente ou por arquivo, deixando comentários, sugestões de código e muito mais.
