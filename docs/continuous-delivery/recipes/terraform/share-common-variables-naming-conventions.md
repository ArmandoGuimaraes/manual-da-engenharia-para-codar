# Compartilhando Variáveis Comuns / Convenções de Nomenclatura Entre Módulos do Terraform

## O que estamos tentando resolver?

Ao implantar infraestrutura por meio de código, é prática comum dividir o código em diferentes módulos responsáveis pela implantação de uma parte ou componente da infraestrutura. No Terraform, isso pode ser feito usando [módulos](https://www.terraform.io/language/modules/develop).

Neste caso, é útil ser capaz de compartilhar algumas variáveis comuns, bem como centralizar as convenções de nomenclatura dos diferentes recursos, para garantir que seja fácil refatorar quando necessário, apesar das dependências entre módulos.

Por exemplo, considere 2 módulos:

- Módulo de rede, responsável pela implantação de Rede Virtual, Sub-redes, Grupos de Segurança de Rede (NSGs) e Zonas DNS Privadas
- Módulo Azure Kubernetes Service responsável pela implantação do cluster AKS

Existem dependências entre esses módulos, como o cluster Kubernetes que será implantado na rede virtual do Módulo de Rede. Para fazer isso, ele deve fazer referência ao nome da rede virtual, bem como ao grupo de recursos no qual está implantado. E idealmente, gostaríamos que essas dependências fossem fracamente acopladas, o máximo possível, para manter a agilidade na forma como os módulos são implantados e manter o ciclo de vida independente.

Esta página explica uma maneira de resolver isso com o Terraform.

## Como fazer isso?

### Contexto

Vamos considerar a seguinte estrutura para nossos módulos:

```console
modules
├── kubernetes
│   ├── main.tf
│   ├── provider.tf
│   └── variables.tf
├── network
│   ├── main.tf
│   ├── provider.tf
│   └── variables.tf
```

Agora, suponha que você implanta uma rede virtual para o ambiente de desenvolvimento, com as seguintes propriedades:

- Nome: vnet-dev
- Grupo de recursos: rg-dev-network

Em algum momento, você precisa injetar esses valores no módulo Kubernetes, para obter uma referência a ele por meio de uma fonte de dados, por exemplo:

```hcl
data "azurem_virtual_network" "vnet" {
    name                = var.vnet_name
    resource_group_name = var.vnet_rg_name
}
```

No trecho acima, o nome da rede virtual e o grupo de recursos são definidos por meio de variáveis. Isso é ótimo, mas se isso mudar no futuro, os valores dessas variáveis também devem ser alterados. Em todos os módulos em que são usados.

Ser capaz de gerenciar a nomenclatura em um local central garantirá que o código possa ser facilmente refatorado no futuro, sem atualizar todos os módulos.

### Sobre Variáveis do Terraform

No Terraform, cada [variável de entrada](https://www.terraform.io/language/values/variables) deve ser definida no nível de configuração (ou módulo), usando o bloco `variable`. Por convenção, isso é frequentemente feito em um arquivo `variables.tf`, no módulo. Este arquivo contém a declaração de variáveis e seus valores padrão. Valores podem ser definidos usando arquivos de configuração de variáveis (.tfvars), variáveis de ambiente ou argumentos CLI ao usar os comandos `terraform plan` ou `terraform apply`.

Uma das limitações da declaração de variáveis é que não é possível compor variáveis, [locals](https://www.terraform.io/language/values/locals) ou [funções incorporadas do Terraform](https://www.terraform.io/language/functions) são usadas para isso.

### Módulo Comum do Terraform

Uma maneira de contornar essas limitações é introduzir um módulo "comum" que não implantará recursos, mas apenas calculará/solucionará e produzirá os nomes de recursos e variáveis compartilhadas e será usado por todos os outros módulos, como uma dependência.

```console
modules
├── common
│   ├── output.tf
│   └── variables.tf
├── kubernetes
│   ├── main.tf
│   ├── provider.tf
│   └── variables.tf
├── network
│   ├── main.tf
│   ├── provider.tf
│   └── variables.tf
```

*variables.tf:*

```hcl
variable "environment_name" {
  type        = string
  description = "O nome do ambiente."
}

variable "location" {
  type        = string
  description = "A região do Azure onde os recursos serão criados. O padrão é oeste da Europa."
  default     = "westeurope"
}
```

*output.tf:*

```hcl
# Variáveis compartilhadas
output "location" {
  value = var.location
}

output "subscription" {
  value = var.subscription
}

# Nomenclatura de Rede Virtual
output "vnet_rg_name" {
  value = "rg-network-${var.environment_name}"
}

output "vnet_name" {
  value = "vnet-${var.environment_name}"
}

# Nomenclatura do AKS
output "aks_rg_name" {
  value = "rg-aks-${var.environment_name}"
}

output "aks_name" {
  value = "aks-${var.environment_name}"
}
```

Agora, se você executar o `terraform apply` para o módulo comum, você obterá todas as variáveis comuns compartilhadas nas saídas:

```console
$ terraform plan -var environment_name="dev" -var subscription="$(az account show --query id -o tsv)"

Mudanças nas Saídas:
  + aks_name     = "aks-dev"
  + aks_rg_name  = "rg-aks-dev"
  + location     = "westeurope"
  + subscription = "01010101-1010-0101-1010-010101010101"
  + vnet_name    = "vnet-dev"
  + vnet_rg_name = "rg-network-dev"

Você pode aplicar este plano para salvar esses novos valores de saída no estado do Terraform, sem alterar nenhuma infraestrutura real.
```

### Usando o Módulo Comum do Terraform

Usar o módulo comum do Terraform em qualquer outro módulo é muito fácil. Por exemplo, isso é o que você pode fazer no arquivo `main.tf` do módulo Azure Kubernetes:

```hcl
module "common" {
  source           = "../common"
  environment_name = var.environment_name
  subscription     = var.subscription
}

data "azurerm_subnet" "aks_subnet" {
  name                 = "AksSubnet"
  virtual_network

_name = module.common.vnet_name
  resource_group_name  = module.common.vnet_rg_name
}

resource "azurerm_kubernetes_cluster" "aks" {
  name                = module.common.aks_name
  resource_group_name = module.common.aks_rg_name
  location            = module.common.location
  dns_prefix          = module.common.aks_name

  identity {
    type = "SystemAssigned"
  }

  default_node_pool {
    name           = "default"
    vm_size        = "Standard_DS2_v2"
    vnet_subnet_id = data.azurerm_subnet.aks_subnet.id
  }
}
```

Em seguida, você pode executar os comandos `terraform plan` e `terraform apply` para implantar!

```console
terraform plan -var environment_name="dev" -var subscription="$(az account show --query id -o tsv)"
data.azurerm_subnet.aks_subnet: Reading...
data.azurerm_subnet.aks_subnet: Read complete after 1s [id=/subscriptions/01010101-1010-0101-1010-010101010101/resourceGroups/rg-network-dev/providers/Microsoft.Network/virtualNetworks/vnet-dev/subnets/AksSubnet]

O Terraform usou os provedores selecionados para gerar o seguinte plano de execução. As ações de recursos são indicadas pelos seguintes símbolos:
  + criar

O Terraform executará as seguintes ações:

  # azurerm_kubernetes_cluster.aks será criado
  + resource "azurerm_kubernetes_cluster" "aks" {
      + dns_prefix                          = "aks-dev"
      + fqdn                                = (conhecido após a aplicação)
      + id                                  = (conhecido após a aplicação)
      + kube_admin_config                   = (conhecido após a aplicação)
      + kube_admin_config_raw               = (valor sensível)
      + kube_config                         = (conhecido após a aplicação)
      + kube_config_raw                     = (valor sensível)
      + kubernetes_version                  = (conhecido após a aplicação)
      + location                            = "westeurope"
      + name                                = "aks-dev"
      + node_resource_group                 = (conhecido após a aplicação)
      + portal_fqdn                         = (conhecido após a aplicação)
      + private_cluster_enabled             = (conhecido após a aplicação)
      + private_cluster_public_fqdn_enabled = false
      + private_dns_zone_id                 = (conhecido após a aplicação)
      + private_fqdn                        = (conhecido após a aplicação)
      + private_link_enabled                = (conhecido após a aplicação)
      + public_network_access_enabled       = true
      + resource_group_name                 = "rg-aks-dev"
      + sku_tier                            = "Free"

      [...] truncado

      + default_node_pool {
          + kubelet_disk_type    = (conhecido após a aplicação)
          + max_pods             = (conhecido após a aplicação)
          + name                 = "default"
          + node_count           = (conhecido após a aplicação)
          + node_labels          = (conhecido após a aplicação)
          + orchestrator_version = (conhecido após a aplicação)
          + os_disk_size_gb      = (conhecido após a aplicação)
          + os_disk_type         = "Managed"
          + os_sku               = (conhecido após a aplicação)
          + type                 = "VirtualMachineScaleSets"
          + ultra_ssd_enabled    = false
          + vm_size              = "Standard_DS2_v2"
          + vnet_subnet_id       = "/subscriptions/01010101-1010-0101-1010-010101010101/resourceGroups/rg-network-dev/providers/Microsoft.Network/virtualNetworks/vnet-dev/subnets/AksSubnet"
        }

      + identity {
          + principal_id = (conhecido após a aplicação)
          + tenant_id    = (conhecido após a aplicação)
          + type         = "SystemAssigned"
        }

      [...] truncado
    }

Plano: 1 para adicionar, 0 para alterar, 0 para destruir.
```

Observação: o uso de um módulo comum também é válido se você decidir implantar todos os seus módulos na mesma operação a partir de um arquivo de configuração principal do Terraform, como:

```hcl
module "common" {
  source           = "./common"
  environment_name = var.environment_name
  subscription     = var.subscription
}

module "network" {
  source           = "./network"
  vnet_name        = module.common.vnet_name
  vnet_rg_name     = module.common.vnet_rg_name
}

module "kubernetes" {
  source           = "./kubernetes"
  aks_name         = module.common.aks_name
  aks_rg           = module.common.aks_rg_name
}
```

### Centralizando as Definições de Variáveis de Entrada

No caso de você optar por definir os valores das variáveis diretamente no controle de origem (por exemplo, cenário gitops) usando [arquivos de definição de variáveis](https://www.terraform.io/language/values/variables#variable-definitions-tfvars-files) (`.tfvars`), ter um módulo comum também ajudará a não duplicar as definições de variáveis comuns em todos os módulos. Na verdade, é possível ter um arquivo global que seja definido uma vez, no nível do módulo comum, e mesclá-lo com arquivos de definições de variáveis específicas do módulo no momento do `plan` ou `apply` do Terraform.

Considere a seguinte estrutura:

```console
modules
├── common
│   ├── dev.tfvars
│   ├── prod.tfvars
│   ├── output.tf
│   └── variables.tf
├── kubernetes
│   ├── dev.tfvars
│   ├── prod.tfvars
│   ├── main.tf
│   ├── provider.tf
│   └── variables.tf
├── network
│   ├── dev.tfvars
│   ├── prod.tfvars
│   ├── main.tf
│   ├── provider.tf
│   └── variables.tf
```

O módulo comum, assim como todos os outros módulos, contém arquivos de variáveis para os ambientes `dev` e `prod`. Os arquivos `tfvars` do módulo comum definirão todas as variáveis globais que serão compartilhadas com outros módulos (como assinatura, nome do ambiente, etc.), e os arquivos `.tfvars` de cada módulo definirão apenas os valores específicos do módulo.

Em seguida, é possível mesclar esses arquivos ao executar os comandos `terraform apply` ou `terraform plan`, usando a seguinte sintaxe:

```bash
terraform plan -var-file=<(cat ../common/dev.tfvars ./dev.tf

vars)
```

*Observação: ao usar isso, é realmente importante garantir que você não tenha os mesmos nomes de variáveis em ambos os arquivos, caso contrário, isso gerará um erro.*

## Conclusão

Ao ter um módulo comum que possua variáveis compartilhadas, bem como convenções de nomenclatura, agora é mais fácil refatorar o código de configuração do Terraform. Imagine que, por algum motivo, você precisa alterar o padrão usado para o nome da rede virtual: você o altera nos arquivos de saída do módulo comum e só precisa reaplicar todos os módulos!
