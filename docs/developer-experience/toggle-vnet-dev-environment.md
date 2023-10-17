# Alternativa para mudar a VNet para ambiente de produção e desenvolvimento

## Declaração do Problema

Ao implantar recursos no Azure em um ambiente seguro, os recursos geralmente são criados atrás de uma Rede Privada (VNet), sem acesso público e com pontos de extremidade privados para consumir recursos. Essa é a abordagem recomendada para ambientes de pré-produção ou produção.

Acessar recursos protegidos a partir de uma máquina local implica em uma das seguintes opções:

- Usar uma VPN
- Usar um **jump box**
  - Com SSH ativado (menos seguro)
  - [Com Bastion (abordagem recomendada)](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/scenarios/cloud-scale-analytics/architectures/connect-to-environments-privately#about-azure-bastion-host-and-jumpboxes)

No entanto, um desenvolvedor pode desejar implantar um ambiente de teste (em uma assinatura não de produção) para seus testes durante a fase de desenvolvimento, sem a complexidade de redes.

Além disso, o código de infraestrutura não deve ser duplicado: ele deve ser o mesmo, quer os recursos sejam implantados em um ambiente semelhante ao de produção ou em um ambiente de desenvolvimento.

## Opção

A ideia é oferecer, **por meio de uma única variável booleana**, a opção de implantar recursos atrás de uma VNet ou não, usando um único código de infraestrutura. Proteger recursos atrás de uma VNet geralmente implica que acessos públicos são desativados e pontos de extremidade privados são criados. Isso é algo a se ter em mente porque, como desenvolvedor, o acesso público deve ser ativado para se conectar a esse ambiente.

O pipeline de implantação definirá esses recursos atrás de uma VNet e os protegerá removendo os acessos públicos. Os desenvolvedores poderão executar o mesmo script de implantação, especificando que os recursos não estarão atrás de uma VNet e não terão os acessos públicos desabilitados.

Considere o seguinte caso de uso: queremos implantar uma VNet, uma sub-rede, uma conta de armazenamento sem acesso público e um ponto de extremidade privado para a tabela.

A variável "mágica" que ajudará a alternar a segurança será chamada de `behind_vnet`, do tipo booleano.

Vamos implementar esse caso de uso usando o `Terraform`.

> O código abaixo não contém tudo, o objetivo é mostrar o padrão e não como implantar esses recursos. Para obter mais informações sobre o Terraform, consulte a [documentação oficial](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs).

Não existe um `if` *per se* no Terraform para definir se um recurso específico deve ser implantado ou não com base no valor de uma variável. No entanto, podemos usar o argumento meta [`count`](https://developer.hashicorp.com/terraform/language/meta-arguments/count). A força desse argumento meta é que, se seu valor for `0`, o bloco será ignorado.

Aqui estão trechos de código para essa implantação:

- variables.tf

    ```terraform
    variable "behind_vnet" {
        type    = bool
    }
    ```

- main.tf

    ```terraform
    resource "azurerm_virtual_network" "vnet" {
        count = var.behind_vnet ? 1 : 0

        name                = "MyVnet"
        address_space       = [x.x.x.x/16]
        resource_group_name = "MyResourceGroup"
        location            = "WestEurope"

        ...

        subnet {
            name              = "subnet_1"
            address_prefix    = "x.x.x.x/24"
        }
    }

    resource "azurerm_storage_account" "storage_account" {
        name                = "storage"
        resource_group_name = "MyResourceGroup"
        location            = "WestEurope"
        tags                = var.tags

        ...

        public_network_access_enabled = var.behind_vnet ? false : true
    }

    resource "azurerm_private_endpoint" "storage_account_table_private_endpoint" {
        count = var.behind_vnet ? 1 : 0

        name                = "pe-storage"
        subnet_id           = azurerm_virtual_network.vnet[0].subnet[0].id

        ...

        private_service_connection {
            name                           = "psc-storage"
            private_connection_resource_id = azurerm_storage_account.storage_account.id
            subresource_names              = [ "table" ]
            ...
        }

        private_dns_zone_group {
            name = "privateDnsZoneGroup"
            ...
        }
    }
    ```

Se executarmos

```bash
terraform apply -var behind_vnet=true
```

então todos os recursos acima serão implantados, e é o que queremos em um ambiente semelhante ao de produção ou de pré-produção. A instrução `count = var.behind_vnet ? 1 : 0` definirá `count` com o valor `1`, portanto, os blocos serão executados.

No entanto, se executarmos

```bash
terraform

 apply -var behind_vnet=false
```

os recursos `azurerm_virtual_network` e `azurerm_private_endpoint` serão ignorados (porque `count` será `0`). O recurso `azurerm_storage_account` será criado, com pequenas diferenças em algumas propriedades: por exemplo, aqui, `public_network_access_enabled` será definido como `true` (e este é o objetivo para um desenvolvedor poder acessar recursos criados).

O mesmo padrão pode ser aplicado repetidamente para todo o código de infraestrutura.

## Conclusão

Com essa abordagem, a mesma base de código de infraestrutura pode ser usada para direcionar um ambiente semelhante ao de produção com recursos seguros atrás de uma VNet sem acessos públicos e também um ambiente de desenvolvimento mais permissivo.

No entanto, há algumas compensações com essa abordagem:

- Se um recurso tiver o argumento `count`, ele precisará ser tratado como uma lista e não como um único item. No exemplo acima, se houver a necessidade de fazer referência ao recurso `azurerm_virtual_network` posteriormente no código,

    ```terraform
    azurerm_virtual_network.vnet.id
    ```

    não funcionará. O seguinte deve ser usado

    ```terraform
    azurerm_virtual_network.vnet[0].id # Primeiro (e único) item da coleção
    ```

- O argumento meta `count` não pode ser usado com `for_each` para um bloco inteiro. Isso significa que o uso de loops para implantar vários pontos de extremidade, por exemplo, não funcionará. Cada ponto de extremidade privado precisará ser implantado individualmente.
