# Revisão de Código em Terraform

## Guia de Estilo

Os desenvolvedores devem seguir o [guia de estilo do Terraform](https://github.com/jonbrouse/terraform-style-guide/blob/master/README.md).

Projetos devem verificar os scripts do Terraform com ferramentas automatizadas.

## Análise de Código / Linting

### TFLint

O [`TFLint`](https://github.com/terraform-linters/tflint) é um linter do Terraform focado em possíveis erros, melhores práticas, etc. Uma vez instalado o TFLint no ambiente, ele pode ser invocado usando a extensão do VS Code [`terraform`](https://marketplace.visualstudio.com/items?itemName=mauve.terraform).

## Extensões do VS Code

As seguintes extensões do VS Code são amplamente utilizadas.

### [`Extensão Terraform`](https://marketplace.visualstudio.com/items?itemName=mauve.terraform)

Esta extensão fornece realce de sintaxe, lintagem, formatação e capacidades de validação.

### [`Extensão Azure Terraform`](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureterraform)

Esta extensão oferece suporte aos comandos do Terraform, visualização de gráfico de recursos e integração com o CloudShell dentro do VS Code.

## Validação de Build

Certifique-se de aplicar os guias de estilo durante a construção. O seguinte script de exemplo pode ser usado para instalar o Terraform e um linter que verifica a formatação e erros comuns.

```shell
#! /bin/bash
set -e

SCRIPT_DIR=$(dirname "$BASH_SOURCE")
cd "$SCRIPT_DIR"

TF_VERSION=0.12.4
TF_LINT_VERSION=0.9.1

echo -e "\n\n>>> Instalando Terraform 0.12"
# Instale as ferramentas do Terraform para a verificação de formatação
wget -q https://releases.hashicorp.com/terraform/${TF_VERSION}/terraform_${TF_VERSION}_linux_amd64.zip -O /tmp/terraform.zip
sudo unzip -q -o -d /usr/local/bin/ /tmp/terraform.zip

echo ""
echo -e "\n\n>>> Instalando o TFLint (terceiros)"
wget -q https://github.com/wata727/tflint/releases/download/v${TF_LINT_VERSION}/tflint_linux_amd64.zip -O /tmp/tflint.zip
sudo unzip -q -o -d /usr/local/bin/ /tmp/tflint.zip

echo -e "\n\n>>> Versão do Terraform"
terraform -version

echo -e "\n\n>>> Formato do Terraform (se falhar, use o comando 'terraform fmt -recursive' para resolver"
terraform fmt -recursive -diff -check

echo -e "\n\n>>> TFLint"
tflint

echo -e "\n\n>>> Terraform init"
terraform init

echo -e "\n\n>>> Terraform validate"
terraform validate
```

## Lista de Verificação de Revisão de Código

Além da [Lista de Verificação de Revisão de Código](../process-guidance/reviewer-guidance.md), você também deve procurar por esses itens específicos de revisão de código do Terraform.

### Provedores

* [ ] Todos os provedores usados nos scripts do Terraform estão [versionados](https://www.terraform.io/language/providers/requirements#best-practices-for-provider-versions) para evitar alterações quebradas no futuro?

### Organização do Repositório

* [ ] O código foi dividido em módulos reutilizáveis?
* [ ] Os módulos foram divididos em arquivos `.tf` separados quando apropriado?
* [ ] O repositório contém um `README.md` descrevendo a arquitetura provisionada?
* [ ] Se o código do Terraform está misturado com o código-fonte da aplicação, o código do Terraform foi isolado em uma pasta dedicada?

### Estado do Terraform

* [ ] O projeto do Terraform está configurado usando o Azure Storage como backend de estado remoto?
* [ ] A chave do backend de estado remoto está armazenada em um local seguro (por exemplo, Azure Key Vault)?
* [ ] O projeto está configurado para usar arquivos de estado com base no ambiente, e o pipeline de implantação está configurado para fornecer dinamicamente o nome do arquivo de estado?

### Variáveis

* [ ] Se a infraestrutura será diferente dependendo do ambiente (por exemplo, Dev, UAT, Produção), os parâmetros específicos do ambiente são fornecidos por meio de um arquivo `.tfvars`?
* [ ] Todas as variáveis têm informações de `type`. Por exemplo, `list(string)` ou `string`.
* [ ] Todas as variáveis têm uma `description` que indica o propósito da variável e seu uso.
* [ ] Valores `default` não são fornecidos para variáveis que devem ser fornecidas por um usuário.

### Testes

* [ ] Existem testes unitários e de integração que cobrem o código do Terraform (por exemplo, [`Terratest`](https://terratest.gruntwork.io/), [`terratest-abstraction`](https://github.com/microsoft/terratest-abstraction))?

### Nomeação e Estrutura do Código

* [ ] As definições de recursos e fontes de dados são usadas corretamente nos scripts do Terraform?
  * **resource:** Indica ao Terraform que a configuração atual é responsável por gerenciar o ciclo de vida do objeto.
  * **data:** Indica ao Terraform que você só deseja obter uma referência ao objeto existente, mas não deseja gerenciá-lo como parte desta configuração.
* [ ] Os nomes dos recursos começam com o nome do provedor que os contém, seguido de um sublinhado? Por exemplo, um recurso do provedor `postgresql` pode ser nomeado como `postgresql_database`?
* [ ] A função `try` só é usada com referências de atributos simples e funções de conversão de tipo? O uso excessivo da função `try` para suprimir erros levará a uma configuração difícil de entender e manter.
* [ ] As funções de conversão de tipo explícito usadas para normalizar tipos só são retornadas nas saídas do módulo? Conversões de tipo explícitas raramente são necessárias no Terraform, pois ele converterá tipos automaticamente quando necessário.
* [ ] A propriedade `Sensitive` no esquema está definida como `true` para os campos que contêm informações confidenciais? Isso evitará que os valores do campo apareçam na saída da CLI.

### Recomendações Gerais

* Tente evitar o aninhamento de configurações secundárias dentro de recursos. Crie uma seção de recursos separada para recursos, mesmo que possam ser declarados como subelementos de um recurso. Por exemplo, declarar sub-redes dentro da rede virtual versus declarar sub-redes como recursos separados em comparação com a rede virtual no Azure.
* Nunca codifique valores estáticos na configuração. Declare-os na seção `locals` se uma variável for necessária várias vezes como um valor estático e for interna à configuração.
* Os `names` dos recursos criados no Azure não devem ser codificados ou estáticos. Esses nomes devem ser dinâmicos e fornecidos pelo usuário usando o bloco `variable`. Isso é útil especialmente nos testes de unidade quando vários testes são executados em paralelo tentando criar recursos no Azure, mas precisam de nomes diferentes (alguns recursos no Azure precisam ter nomes exclusivos, por exemplo, contas de armazenamento).
* É uma boa prática `output` o ID dos recursos criados no Azure a partir da configuração. Isso é especialmente útil ao adicionar blocos dinâmicos para subelementos/elementos filhos ao recurso pai.
* Use o bloco `required_providers` para estabelecer a dependência de provedores juntamente com a versão predefinida.
* Use o bloco `terraform` para declarar a dependência do provedor com a versão exata e também a versão do terraform CLI necessária para a configuração.
* Valide os valores das variáveis fornecidos com base no uso e no tipo da variável. A validação pode ser feita nas variáveis adicionando o bloco `validation`.
