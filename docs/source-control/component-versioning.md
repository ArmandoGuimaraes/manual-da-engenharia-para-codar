# Versionamento de Componentes

## Objetivo

Aplicativos maiores consistem em vários componentes que se referenciam e dependem da compatibilidade das interfaces/contratos desses componentes.

Para alcançar o objetivo de aplicativos fracamente acoplados, cada componente deve ser versionado independentemente, permitindo que os desenvolvedores detectem mudanças que quebram a compatibilidade ou atualizações sem problemas apenas olhando para o número da versão.

## Números de Versão e Esquemas de Versionamento

Para que os desenvolvedores ou outros componentes detectem mudanças que quebram a compatibilidade, o número da versão de um componente é importante.

Existem diferentes esquemas de números de versão, como:

`major.minor[.build[.revision]]`

ou

`major.minor[.maintenance[.build]]`.

Após a compilação/CI, esses números de versão são gerados. Durante a implantação/lançamento, os componentes são enviados para um *repositório de componentes*, como Nuget, NPM, Docker Hub, onde um histórico de diferentes versões é mantido.

A cada compilação, o número da versão é incrementado no último dígito.

A atualização da versão principal/minor indica mudanças na API/interfaces/contratos:

* Versão Principal: Uma mudança que quebra a compatibilidade
* Versão Menor: Uma mudança menor compatível com versões anteriores
* Build/Revisão: Sem mudança na API, apenas uma compilação diferente.

## Versionamento Semântico

O Versionamento Semântico é um esquema de versionamento que especifica como interpretar os diferentes números de versão. O formato mais comum é `major.minor.patch`. O número da versão é incrementado com base nas seguintes regras:

* Versão Principal quando você faz mudanças na API que quebram a compatibilidade,
* Versão Menor quando você adiciona funcionalidades de maneira compatível com versões anteriores e
* Versão de Patch quando você faz correções de bugs compatíveis com versões anteriores.

Exemplos de números de versão semântica:

* **1.0.0-alpha.1**: +1 commit *após* o lançamento alpha de 1.0.0
* **2.1.0-beta**: 2.1.0 no branch beta
* **2.4.2**: Lançamento 2.4.2

Uma prática comum é determinar o número da versão durante o processo de compilação. Para isso, o repositório de controle de origem é utilizado para determinar automaticamente o número da versão com base no repositório de código-fonte.

A ferramenta `GitVersion` utiliza o histórico do Git para gerar um número de versão *repetível* e *único* com base em:

* número de commits desde o último lançamento importante ou menor
* mensagens de commit
* tags
* nomes de branches

As atualizações de versão ocorrem por meio de:

* Mensagens de commit ou tags para atualizações de Versão Principal/Minor/Revisão.
  > Ao usar mensagens de commit, é recomendável seguir uma convenção, como os "Conventional Commits" (consulte [Orientação do Git - Estrutura de Mensagens de Commit](git-guidance/README.md#commit-message-structure))
* Nomes de branches (por exemplo, develop, release/..) para Alpha/Beta/RC
* Caso contrário: Número de commits (+12, ...)

## Recursos

* [GitVersion](https://gitversion.net/)
* [Versionamento Semântico (em inglês)](https://semver.org/)
* [Versionamento em C# (em inglês)](https://learn.microsoft.com/pt-br/dotnet/csharp/versioning)
