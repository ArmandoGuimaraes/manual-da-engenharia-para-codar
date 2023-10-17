# Diretrizes para Estruturar a Configuração do Terraform

## Contexto
Ao criar uma configuração de infraestrutura, é importante seguir uma estrutura consistente e organizada para garantir a manutenibilidade, escalabilidade e reutilização do código. O objetivo desta seção é descrever brevemente como estruturar sua configuração do Terraform para alcançar esses objetivos.

## Estruturando a Configuração do Terraform

A estrutura recomendada é a seguinte:

1. Coloque cada componente que deseja configurar em sua própria pasta de módulo. Analise o código de sua infraestrutura e identifique os componentes lógicos que podem ser separados em módulos reutilizáveis. Isso dará a você uma clara separação de preocupações e tornará fácil incluir novos recursos, atualizar os existentes ou reutilizá-los no futuro. Para obter mais detalhes sobre módulos e quando usá-los, consulte o [guia do Terraform](https://developer.hashicorp.com/terraform/language/modules/develop#when-to-write-a-module).

2. Coloque os arquivos de módulo `.tf` na raiz de cada pasta e certifique-se de incluir um arquivo [`README`](#gerando-a-documentação) em formato markdown que pode ser gerado automaticamente com base no código do módulo. É recomendável seguir esta abordagem, pois essa estrutura de arquivo será automaticamente reconhecida pelo [Terraform Registry](https://registry.terraform.io/browse/modules).

3. Use um conjunto consistente de arquivos para estruturar seus módulos. Embora isso possa variar dependendo das necessidades específicas do projeto, um bom exemplo pode ser o seguinte:
   - **provider.tf**: define a lista de provedores de acordo com os plugins usados
   - **data.tf**: define informações lidas de diferentes fontes de dados
   - **main.tf**: define os objetos de infraestrutura necessários para sua configuração (por exemplo, grupo de recursos, atribuição de função, registro de contêiner)
   - **backend.tf**: arquivo de configuração de backend
   - **outputs.tf**: define dados estruturados que são exportados
   - **variables.tf**: define valores estáticos e reutilizáveis
4. Inclua em cada módulo subpastas para documentação, exemplos e testes.
A documentação inclui informações básicas sobre o módulo: o que ele está instalando, quais são as opções, um exemplo de caso de uso, e assim por diante. Você também pode adicionar aqui quaisquer outros detalhes relevantes que possa ter.
A pasta de exemplos pode incluir um ou mais exemplos de como usar o módulo, cada exemplo tendo o mesmo conjunto de arquivos de configuração decidido na etapa anterior. É recomendável também incluir um README que forneça uma compreensão clara de como ele pode ser usado na prática.
A pasta de testes inclui um ou mais arquivos para testar o módulo de exemplo junto com um arquivo de documentação com instruções sobre como esses testes podem ser [executados](https://www.hashicorp.com/blog/testing-hashicorp-terraform).
5. Coloque o módulo raiz em uma pasta separada chamada `main`: este é o ponto de entrada principal para a configuração. Assim como nos outros módulos, ele conterá seus arquivos de configuração correspondentes.

Uma estrutura de configuração de exemplo obtida usando as diretrizes acima é:

```console
modules
├── mlops
│   ├── doc
│   ├── example
│   ├── test
│   ├── backend.tf
│   ├── data.tf
│   ├── main.tf
│   ├── outputs.tf
│   ├── provider.tf
│   ├── variables.tf
│   ├── README.md
├── common
├── main
```


## Convenção de Nomenclatura

Ao nomear variáveis do Terraform, é essencial usar convenções de nomenclatura claras e consistentes que sejam fáceis de entender e seguir. A convenção geral é usar letras minúsculas e números, com underscores em vez de traços, por exemplo: "azurerm_resource_group".
Ao nomear recursos, comece com o nome do provedor, seguido pelo recurso de destino, separado por underscores. Por exemplo, "azurerm_postgresql_server" é um nome apropriado para um recurso de provedor Azure. Quando se trata de fontes de dados, use uma convenção de nomenclatura semelhante, mas certifique-se de usar nomes no plural para listas de itens. Por exemplo, "azurerm_resource_groups" é um bom nome para uma fonte de dados que representa uma lista de grupos de recursos.
Nomes de variáveis e saídas devem ser descritivos e refletir o propósito ou uso da variável. Também é útil agrupar itens relacionados usando um prefixo comum. Por exemplo, todas as variáveis relacionadas a contas de armazenamento podem começar com "storage_". Tenha em mente que as saídas devem ser compreensíveis fora de seu escopo. Um padrão de nomenclatura útil a seguir é "{nome}_{atributo}", onde "nome" representa o nome de um recurso ou fonte de dados, e "atributo" é o atributo retornado pela saída. Por exemplo, "storage_primary_connection_string" poderia ser um nome de saída válido.

Certifique-se de incluir uma descrição para saídas e variáveis, além de marcar os valores como 'default' ou 'sensitive' quando necessário. Essas informações serão capturadas na documentação gerada.

## Gerando a Documentação

A documentação pode ser gerada automaticamente com base no código de configuração em seus módulos com a ajuda do [terraform-docs](https://terraform-docs.io/). Para gerar a documentação do módulo Terraform, vá para a pasta do módulo e insira este comando:

```sh
terraform-docs markdown table --output-file README.md --output-mode inject .
```

Em seguida, a documentação será gerada dentro do diretório raiz do componente.

## Conclusão

A abordagem apresentada nesta seção foi projetada para ser flexível e fácil de usar, tornando simples adicionar novos recursos ou atualizar os existentes. A separação de preocupações também facilita a reutilização de componentes existentes em outros projetos, com todas as informações (módulos, exemplos, documentação e testes) localizadas em um só lugar.

## Referências e Leituras Adicionais

- [Terraform-docs](https://github.com/terraform-docs/terraform-docs)
- [Terraform Registry](https://registry.terraform.io/browse/modules)
- [Terraform Module Guidance](https://developer.hashicorp.com/terraform/language/modules/develop#when-to-write-a-module)
- [Terratest](https://terratest.gruntwork.io/)
- [Testing HashiCorp Terraform](https://www.hashicorp.com/blog/testing-hashicorp-terraform)
- [Build Infrastructure - Terraform Azure Example](https://developer.hashicorp.com/terraform/tutorials/azure-get-started/azure-build)
