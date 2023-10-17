# Documentação

Todo projeto de desenvolvimento de software requer documentação. O [Desenvolvimento Ágil de Software](https://agilemanifesto.org/) valoriza *software funcionando sobre documentação abrangente*. Ainda assim, os projetos devem incluir as informações-chave necessárias para entender o desenvolvimento e o uso do software gerado.

A documentação não deve ser uma reflexão tardia. Diferentes documentos escritos e materiais devem ser criados durante todo o ciclo de vida do projeto, conforme as necessidades do projeto.

## Sumário

- [Objetivos](#objetivos)
- [Desafios](#desafios)
- [Que documentação deve existir?](#que-documentação-deve-existir)
- [Melhores práticas](#melhores-práticas)
- [Ferramentas](#ferramentas)
- [Receitas](#receitas)
- [Recursos](#recursos)

## Objetivos

- Facilitar a integração de novos membros da equipe.
- Melhorar a comunicação e colaboração entre equipes (especialmente quando distribuídas em fusos horários diferentes).
- Melhorar a transição do projeto para outra equipe.

## Desafios

Ao trabalhar em um projeto de engenharia, geralmente nos deparamos com um ou mais desses desafios relacionados à documentação (incluindo alguns exemplos):

- **Inexistente**.
  - Nenhuma documentação de integração, então leva muito tempo para configurar o ambiente quando você entra no projeto.
  - Nenhum documento na wiki explicando os repositórios existentes, então você não sabe qual dos 10 repositórios disponíveis deve clonar.
  - Nenhum README principal, então você não sabe por onde começar quando clona um repositório.
  - Nenhuma seção "como contribuir", então você não sabe qual é a política de branch, onde adicionar novos documentos, etc.
  - Nenhuma diretriz de código, então todos seguem convenções de nomenclatura diferentes, etc.
- **Oculto**.
  - Impossível encontrar documentação útil, pois está espalhada por todo lugar. Por exemplo, sem ideia de como compilar, executar e testar o código, pois o README está escondido em uma pasta dentro de outra pasta dentro de outra pasta.
  - Processos úteis (por exemplo, processo de grooming) explicados fora da ferramenta de gerenciamento de backlog e não vinculados a nenhum lugar.
  - Decisões tomadas em canais diferentes que não a ferramenta de gerenciamento de backlog e não registradas em nenhum outro lugar.
- **Incompleta**.
  - Nenhuma política de branch clara, então todos nomeiam suas branches de maneira diferente.
  - Configurações ausentes no documento "como executar isso" que são necessárias para executar a aplicação.
- **Inexata**.
  - Documentos não atualizados junto com o código, então eles não mencionam as pastas corretas, configurações, etc.
- **Obsoleta**.
  - Documentos de design que não se aplicam mais, ao lado de documentos válidos. Qual deles mostra as decisões mais recentes?
- **Desorganizada (por assunto/data)**.
  - Documentos não organizados por assunto/workstream, portanto, não é fácil encontrar informações relevantes quando você muda para um novo workstream.
  - Registros de decisões de design fora de ordem e sem uma data que ajude a determinar qual é a decisão final sobre algo.
- **Duplicada**.
  - Nenhum arquivo de configurações disponível em um local centralizado como fonte única da verdade, então os desenvolvedores precisam continuar compartilhando suas próprias versões, e acabamos com muitos arquivos que podem ou não funcionar.
- **Reflexão tardia**.
  - Documentos-chave criados várias semanas após o início do projeto: integração, como executar o aplicativo, etc.
  - Documentos criados de última hora, pouco antes do término de um projeto, esquecendo que também ajudam a equipe durante o trabalho no projeto.

## Que documentação deve existir

- [Projeto e Repositórios](./orientacao/projeto-e-repositorios.md)
- [Mensagens de Commit](../controle-de-fonte/git-orientacao/README.md#melhores-praticas-de-commit)
- [Pull Requests](./orientacao/pull-requests.md)
- [Código](./orientacao/codigo.md)
- [Itens de Trabalho](./orientacao/itens-de-trabalho.md)
- [APIs REST](./orientacao/apis-rest.md)
- [Feedback de Engenharia](./orientacao/feedback-de-engenharia.md)

## Melhores práticas

- [Estabelecendo e gerenciando documentação](./melhores-praticas/estabelecer-e-gerenciar.md)
- [Criando boa documentação](./melhores-praticas/boa-documentacao.md)
- [Substituindo documentação por automação](./melhores-praticas/automacao.md)

## Ferramentas

- [Wikis](./ferramentas/wikis.md)
- [Linguagens](./ferramentas/linguagens.md)
  - [markdown](./ferramentas/linguagens.md#markdown)
  - [mermaid](./ferramentas/linguagens.md#mermaid)
- [Como automatizar verificações simples](./ferramentas/automacao.md)
- [Integração com Teams/Slack](./ferramentas/integracoes.md)

## Receitas

- [Como sincronizar uma wiki entre repositórios](./receitas/sincronizar-wiki-entre-repositorios.md)
- [Usando DocFx e Ferramentas Complementares para gerar um site de Documentação](./receitas/usar-docfx-e-ferramentas.md)
- [Implantar automaticamente o site de Documentação DocFx em um site Azure](./receitas/implantar-docfx-site-azure.md)
- [Como criar um site estático para sua documentação com base em MkDocs e Material for MkDocs](./receitas/site-estatico-com-mkdocs.md)

## Recursos

- [Tipos de Documentação de Software e Melhores Práticas](https://blog.prototypr.io/software-documentation-types-and-best-practices-1726ca595c7f)
