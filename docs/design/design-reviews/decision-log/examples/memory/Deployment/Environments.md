# Implantação da Aplicação

A aplicação Memory utiliza o [Azure DevOps](https://learn.microsoft.com/en-gb/azure/devops/index?view=azure-devops) para rastreamento de itens de trabalho, bem como integração contínua (CI) e implantação contínua (CD).

---

## Ambientes

O projeto Memory utiliza vários ambientes para isolar e testar alterações antes de promover lançamentos para a base de usuários global.

A implantação de novos ambientes é acionada automaticamente com base em uma implantação bem-sucedida do estágio / ambiente anterior.

Os ambientes de **desenvolvimento**, **staging** e **produção** utilizam a implantação de slot durante uma implantação de ambiente.
Após a implantação de um novo lançamento em um slot de staging, ele é validado por meio de uma série de testes de integração funcional.
Após uma taxa de aprovação de 100% de todos os testes, os slots de staging e produção são trocados, tornando efetivamente as atualizações no ambiente disponíveis.

Quaisquer erros ou testes falhados interrompem a implantação no estágio atual e impedem alterações em estágios posteriores.

Cada ambiente implantado é completamente isolado e não compartilha nenhum componente.
Cada um possui instâncias de recursos exclusivos, como Azure Traffic Manager, Cosmos DB, etc.

### Dependências de Implantação

| Desenvolvimento | Staging     | Produção      |
|------------------|-------------|-----------------|
| Portões de Qualidade da CI | Desenvolvimento | Staging         |
|                  |             | Aprovação Manual |

### Local

O ambiente local é usado por engenheiros de software individuais durante o desenvolvimento de novos recursos e componentes.

Os engenheiros utilizam alguns componentes do ambiente de desenvolvimento implantado que não estão disponíveis em determinadas plataformas ou não podem ser executados localmente.

- CosmosDB
  - O emulador só existe para Windows

O ambiente local também não utiliza o Azure Traffic Manager.
O aplicativo web frontend se comunica diretamente com a API REST de backend, normalmente em uma porta localhost separada.

### Desenvolvimento

O ambiente de desenvolvimento é usado como o primeiro portão de qualidade.
Todo o código verificado no branch `main` é implantado automaticamente neste ambiente após a passagem de todos os portões de qualidade da CI.

#### Regiões de Desenvolvimento

- West US (westus)

### Staging

O ambiente de staging é usado para validar novos recursos, componentes e outras alterações antes da implantação em produção.
Este ambiente é usado principalmente por desenvolvedores, QA e outros stakeholders da empresa.

#### Regiões de Staging

- West US (westus)
- East US (eastus)

### Produção

O ambiente de produção é usado pela base de usuários em todo o mundo.
As alterações neste ambiente são controladas por aprovação manual pela equipe de liderança do produto, além de outros portões de qualidade automáticos.

#### Regiões de Produção

- West US (westus)
- Central US (centralus)
- East US (eastus)

---

## Grupo de Variáveis de Ambiente

### Configuração de Infraestrutura (memory-common)

- appName
- businessUnit
- serviceConnection
- subscriptionId

### Configuração de Desenvolvimento (memory-dev)

- environmentName (espaço reservado)

### Configuração de Staging (memory-staging)

- environmentName (espaço reservado)

### Configuração de Produção (memory-prod)

- environmentName (espaço reservado)
