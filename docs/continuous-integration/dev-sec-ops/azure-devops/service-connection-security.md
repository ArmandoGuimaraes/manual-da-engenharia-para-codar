# Segurança da Conexão de Serviço no Azure DevOps

As Conexões de Serviço são usadas nos Pipelines do Azure DevOps para se conectar a serviços externos, como Azure, GitHub, Docker, Kubernetes e muitos outros. As Conexões de Serviço podem ser usadas para autenticar-se nesses serviços externos e para invocar diversos tipos de comandos, como criar e atualizar recursos no Azure, fazer upload de imagens de contêiner no Docker ou implantar aplicativos no Kubernetes.

Para poder invocar esses comandos, as Conexões de Serviço precisam ter as permissões corretas para fazê-lo. Para a maioria dos tipos de Conexões de Serviço, as permissões podem ser limitadas a um subconjunto de recursos para restringir o acesso que elas têm. Para melhorar o princípio do menor privilégio, é comum ter Conexões de Serviço separadas para diferentes ambientes, como Dev/Teste/QA/Produção.

## Segurança da Conexão de Serviço

A segurança das Conexões de Serviço pode ser alcançada por meio de vários métodos.

- [Permissões de usuário](https://learn.microsoft.com/en-us/azure/devops/pipelines/library/service-endpoints#user-permissions) podem ser configuradas para garantir que apenas os usuários corretos possam criar, visualizar, usar e gerenciar a Conexão de Serviço.
- [Permissões de nível de pipeline](https://learn.microsoft.com/en-us/azure/devops/pipelines/library/service-endpoints#pipeline-permissions) podem ser configuradas para garantir que apenas pipelines YAML aprovados possam usar a Conexão de Serviço.
- [Permissões de projeto](https://learn.microsoft.com/en-us/azure/devops/pipelines/library/service-endpoints#project-permissions---cross-project-sharing-of-service-connections) podem ser configuradas para garantir que apenas determinados projetos do Azure DevOps possam usar a Conexão de Serviço.

Após o uso dos métodos acima, o que está seguro é **quem** pode usar as Conexões de Serviço. O que ainda não está seguro, no entanto, é **o que** pode ser feito com as Conexões de Serviço.

Porque as Conexões de Serviço têm todas as permissões necessárias nos serviços externos, é crucial garantir que elas não possam ser usadas acidentalmente ou por usuários mal-intencionados. Um exemplo disso é um Pipeline do Azure DevOps que usa uma Conexão de Serviço a um Grupo de Recursos do Azure (ou a uma assinatura inteira) para listar todos os recursos e depois excluir esses recursos. Sem a segurança correta em vigor, seria possível executar esse Pipeline sem validação ou revisão.

```yaml
pool:
  vmImage: ubuntu-latest

steps:
- task: AzureCLI@2
  inputs:
    azureSubscription: 'Conexão de Serviço de Produção'
    scriptType: 'pscore'
    scriptLocation: 'inlineScript'
    inlineScript: |
      $resources = az resource list
      foreach ($resource in $resources) {
        az resource delete --ids $resource.id
      }
```

## Aviso de Segurança do Pipeline

Os pipelines YAML podem ser acionados sem a necessidade de um pull request, o que introduz um risco de segurança.

Em boas práticas, [Pull Requests](../../../code-reviews/pull-requests.md) e [Code Reviews](../../../code-reviews/README.md) devem ser usados para garantir que o código que está sendo implantado seja revisado por uma segunda pessoa e, potencialmente, verificado automaticamente em relação a vulnerabilidades e outros problemas de segurança. No entanto, os pipelines YAML podem ser executados sem a necessidade de um Pull Request e Code Reviews. Isso permite ao usuário (malicioso) fazer alterações usando a Conexão de Serviço que normalmente exigiriam um revisor.

A configuração de *quando* um pipeline deve ser acionado é especificada no próprio pipeline YAML e, portanto, um pipeline pode ser configurado para ser executado em alterações em um branch temporário. Nesse branch temporário, quaisquer alterações feitas no pipeline em si serão executadas sem revisão.

Se o pipeline concedeu [Permissões de nível de pipeline](https://learn.microsoft.com/en-us/azure/devops/pipelines/library/service-endpoints#pipeline-permissions) para usar uma Conexão de Serviço específica, qualquer comando pode ser executado usando essa Conexão de Serviço, sem que ninguém revise o comando. Como as Conexões de Serviço podem ter muitas permissões no serviço externo, executar qualquer pipeline sem revisão pode ter potencialmente grandes consequências.

## Verificações de Conexão de Serviço

Para evitar o uso acidental de Conexões de Serviço, existem várias verificações que podem ser configuradas. Essas verificações são configuradas na própria Conexão de Serviço e, portanto, só podem ser configuradas pelo proprietário ou administrador dessa Conexão de Serviço. Um usuário de um determinado pipeline YAML não pode modificar essas verificações, pois elas não são definidas no arquivo YAML em si. A configuração pode ser feita no menu Aprovações e Verificações na Conexão de Serviço.

### Controle de Branch

Configurando o Controle de Branch em uma Conexão de Serviço, você pode controlar que a Conexão de Serviço só pode ser usada em um pipeline YAML se o pipeline estiver sendo executado a partir de um branch específico.

Ao configurar o Controle de Branch para permitir apenas o branch principal (e potencialmente os branches de release), você pode garantir que um pipeline YAML só pode usar a Conexão de Serviço depois que quaisquer alterações nesse pipeline forem mescladas no branch principal e, portanto, tenham passado por verificações de Pull Request e revisões de código.

Com o Controle de Branch em vigor, em combinação com Proteções de Branch, não é mais possível executar comandos contra uma Conexão de Serviço sem que várias pessoas revisem os comandos. Portanto, o uso acidental ou mal-intencionado das permissões de uma Conexão de Serviço não é mais possível.

**Observação: ao definir um curinga para os Branches Permitidos, qualquer pessoa ainda poderia criar um branch que corresponda a esse curinga e usar a Conexão de Serviço. Usando [permissões do Git](https://learn.microsoft.com/en-us/azure/devops/repos/git/require-branch-folders#enforce-permissions), é possível configurar para que apenas administradores possam criar determinados branches, como branches de release.*
 
 ![BranchControl](images/branch-control.png)
