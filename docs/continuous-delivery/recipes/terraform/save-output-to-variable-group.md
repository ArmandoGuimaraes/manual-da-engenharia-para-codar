# Salvar saída do Terraform em um grupo de variáveis (Azure DevOps)

Esta receita se aplica apenas ao uso do [Terraform](https://www.terraform.io/) com o Azure DevOps. Ela pressupõe que você está familiarizado com os comandos do Terraform e com os pipelines do Azure.

## Contexto

Quando o [Terraform](https://www.terraform.io/) é usado para automatizar o provisionamento da infraestrutura, geralmente é dedicado um [Pipeline do Azure](https://learn.microsoft.com/pt-br/azure/devops/pipelines/?view=azure-devops) para aplicar os arquivos de configuração do Terraform. Isso criará, atualizará, excluirá recursos do Azure para provisionar as alterações em sua infraestrutura.

Depois que os arquivos são aplicados, algumas [Valores de Saída](https://developer.hashicorp.com/terraform/language/values/outputs) (por exemplo, nome do grupo de recursos, nome do serviço de aplicativo) podem ser referenciados e retornados pelo Terraform. Esses valores geralmente precisam ser recuperados posteriormente e usados como variáveis de entrada para a implantação de serviços em pipelines separados.

```tf
output "core_resource_group_name" {
  description = "O nome do grupo de recursos"
  value       = module.core.resource_group_name
}

output "core_key_vault_name" {
  description = "O nome do Key Vault."
  value       = module.core.key_vault_name
}

output "core_key_vault_url" {
  description = "A URL do Key Vault."
  value       = module.core.key_vault_url
}
```

O objetivo desta receita é responder à seguinte declaração: Como tornar os valores de saída do Terraform disponíveis em vários pipelines?

## Solução

Uma solução sugerida é armazenar os valores de saída na Biblioteca com um [Grupo de Variáveis](https://learn.microsoft.com/pt-br/azure/devops/pipelines/library/variable-groups?view=azure-devops&tabs=yaml). Grupos de variáveis são uma maneira conveniente de armazenar valores que você deseja passar para um pipeline YAML. Além disso, todos os ativos definidos na Biblioteca compartilham um modelo de segurança comum. Você pode controlar quem pode definir novos itens em uma biblioteca e quem pode usar um item existente.

Para esse fim, estamos usando os seguintes comandos:

- [terraform output](https://developer.hashicorp.com/terraform/cli/commands/output) para extrair o valor de uma variável de saída do arquivo de estado (fornecido pelo [Terraform CLI](https://developer.hashicorp.com/terraform/cli))
- [az pipelines variable-group](https://learn.microsoft.com/pt-br/cli/azure/pipelines/variable-group?view=azure-cli-latest) para gerenciar grupos de variáveis (fornecido pelo [Azure DevOps CLI](https://learn.microsoft.com/pt-br/azure/devops/cli/?view=azure-devops))

Você pode usar o seguinte script após a conclusão do `terraform apply` para criar/atualizar o grupo de variáveis.

### Script (update-variablegroup.sh)

#### Parâmetros

| Nome                | Descrição                                        |
|---------------------|--------------------------------------------------|
| DEVOPS_ORGANIZATION | A URI da organização do Azure DevOps.            |
| DEVOPS_PROJECT      | O nome ou ID do projeto do Azure DevOps.         |
| GROUP_NAME          | O nome do grupo de variáveis alvo.               |

Escolhas de implementação:

- Se um grupo de variáveis já existe, uma opção válida pode ser excluí-lo e reconstruir o grupo do zero. No entanto, como a autorização pode ter sido atualizada no nível do grupo, preferimos evitar essa opção. O script remove todos os valores das variáveis no grupo alvo e os adiciona novamente com os valores mais recentes. As permissões não são afetadas.
- Um grupo de variáveis não pode estar vazio. Ele deve conter pelo menos uma variável. Um valor temporário UUID é criado para mitigar esse problema e é removido assim que as variáveis são atualizadas.

```bash
#!/bin/bash

set -e

export DEVOPS_ORGANIZATION=$1
export DEVOPS_PROJECT=$2
export GROUP_NAME=$3

# Configurar o CLI do Azure DevOps
az devops configure --defaults organization=${DEVOPS_ORGANIZATION} project=${DEVOPS_PROJECT} --use-git-aliases true

# Obter o ID do grupo de variáveis (se já existir)
group_id=$(az pipelines variable-group list --group-name ${GROUP_NAME} --query '[0].id' -o json)

if [ -z "${group_id}" ]; then
    # Criar um novo grupo de variáveis
    tf_output=$(terraform output -json | jq -r 'to_entries[] | "\(.key)=\(.value.value)"')
    az pipelines variable-group create --name ${GROUP_NAME} --variables ${tf_output} --authorize true
else
    # Obter variáveis existentes
    var_list=$(az pipelines variable-group variable list --group-id ${group_id})

    # Adicionar variável UUID temporária (um grupo de variáveis não pode estar vazio)
    uuid=$(cat /proc/sys/kernel/random/uuid)
    az pipelines variable-group variable create --group-id ${group_id} --name ${uuid}

    # Excluir variáveis existentes
    for row in $(echo ${var_list} | jq -r 'to_entries[] | "\(.key)"'); do
        az pipelines variable-group variable delete --group-id ${group_id} --name ${row} --yes
    done

    # Criar variáveis com os valores mais recentes (do Terraform)
    for row in $(terraform output -json | jq -c 'to_entries[]'); do
        _jq()
        {
            echo ${row} | jq -r ${1}
        }

        az pipelines variable-group variable create --group-id ${group_id} --name $(_jq '.key') --value $(_jq '.value.value') --secret $(_jq '.value.sensitive') 
    done

    # Excluir variável UUID temporária
    az pipelines variable-group variable delete --group-id ${group_id} --name ${uuid} --yes
fi
```

## Autenticação no Azure DevOps

A maioria dos comandos usados no script anterior interage com o Azure DevOps e requer autenticação. Você pode autenticar usando o token de segurança `System.AccessToken` usado pelo pipeline em execução, atribuindo-o a uma variável de ambiente chamada `AZURE_DEVOPS_EXT_PAT`, como mostrado no exemplo a seguir (consulte [Azure DevOps CLI in Azure Pipeline YAML](https://learn.microsoft.com/pt-br/azure/devops/cli/azure-devops-cli-in-yaml?view=azure-devops#authenticate-with-azure-devops) para obter informações adicionais).



Além disso, você pode notar que também estamos usando [variáveis predefinidas](https://learn.microsoft.com/pt-br/azure/devops/pipelines/build/variables) para direcionar a organização e o projeto do Azure DevOps (respectivamente `System.TeamFoundationCollectionUri` e `System.TeamProjectId`).

```yaml
  - task: Bash@3
    displayName: 'Atualizar grupo de variáveis usando saídas do Terraform'
    inputs:
      targetType: filePath
      arguments: $(System.TeamFoundationCollectionUri) $(System.TeamProjectId) "Platform-VG"
      workingDirectory: $(terraformDirectory)
      filePath: $(scriptsDirectory)/update-variablegroup.sh
    env:
      AZURE_DEVOPS_EXT_PAT: $(System.AccessToken)
```

| Variáveis do sistema                                                                                                                                                      | Descrição                                                                                                        |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------|
| [System.AccessToken](https://learn.microsoft.com/pt-br/azure/devops/pipelines/build/variables?view=azure-devops&tabs=yaml#systemaccesstoken)                              | Variável especial que carrega o token de segurança usado pela compilação em execução.                           |
| [System.TeamFoundationCollectionUri](https://learn.microsoft.com/pt-br/azure/devops/pipelines/build/variables?view=azure-devops&tabs=yaml#system-variables-devops-services) | A URI da organização do Azure DevOps.                                                                           |
| [System.TeamProjectId](https://learn.microsoft.com/pt-br/azure/devops/pipelines/build/variables?view=azure-devops&tabs=yaml#system-variables-devops-services)             | O ID do projeto ao qual esta compilação pertence.                                                               |

## Segurança da Biblioteca

Funções são definidas para os itens da Biblioteca, e a associação a essas funções governa as operações que você pode executar nesses itens.

| Função para item da biblioteca | Descrição                                                                                                                                                                                                                                                                                                           |
|-------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Leitor                        | Pode visualizar o item.                                                                                                                                                                                                                                                                                                                                                            |
| Usuário                       | Pode usar o item ao criar pipelines de compilação ou liberação. Por exemplo, você deve ser um 'Usuário' de um grupo de variáveis para usá-lo em um pipeline de liberação.                                                                                                                                                                                                       |
| Administrador                  | Também pode gerenciar a associação de todas as outras funções para o item. O usuário que criou um item é automaticamente adicionado à função de Administrador desse item. Por padrão, os seguintes grupos são adicionados à função de Administrador da biblioteca: Administradores de Compilação, Administradores de Liberação e Administradores de Projeto. |
| Criador                        | Pode criar novos itens na biblioteca, mas essa função não inclui permissões de Leitura ou Usuário. A função de Criador não pode gerenciar permissões para outros usuários.                                                                                                                                                                                                      |

Ao usar `System.AccessToken`, a identidade da conta de serviço `<NomeDoProjeto> Build Service` será usada para acessar a Biblioteca.

Certifique-se de que, na seção `Pipelines > Biblioteca > Segurança`, esta conta de serviço tenha a função de `Administrador` no nível da `Biblioteca` ou do `Grupo de Variáveis` para criar/atualizar/excluir variáveis (consulte [Biblioteca de ativos](https://learn.microsoft.com/pt-br/azure/devops/pipelines/library/?view=azure-devops) para informações adicionais).
