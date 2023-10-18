# Reutilização de contêineres de desenvolvimento em um pipeline

Dado um repositório com um contêiner de desenvolvimento local, também conhecido como [dev container](../devcontainers/README.md), que contém todas as ferramentas necessárias para o desenvolvimento, faria sentido reutilizar esse contêiner para executar as ferramentas nos pipelines de Integração Contínua?

## Opções para construir dev containers dentro do pipeline

Existem três maneiras de construir dev containers dentro do pipeline:

- Com [GitHub - devcontainers/ci](https://github.com/devcontainers/ci), que cria o contêiner com o arquivo `devcontainer.json`. Exemplo aqui: [devcontainers/ci · Getting Started](https://github.com/devcontainers/ci/blob/main/docs/github-action.md#getting-started).
- Com [GitHub - devcontainers/cli](https://github.com/devcontainers/cli), que é o mesmo que o acima, mas usa a CLI subjacente diretamente, sem tarefas.
- Construindo o `DockerFile` com `docker build`. Essa opção exclui todas as configurações/recursos especificados no `devcontainer.json`.

## Opções Consideradas

- Executar os pipelines de CI em ambiente nativo.
- Executar os pipelines de CI no contêiner de desenvolvimento construindo a imagem localmente.
- Executar os pipelines de CI no contêiner de desenvolvimento com um registro de contêiner.

Aqui estão os prós e contras de ambas as abordagens:

### Executar os pipelines de CI em ambiente nativo

| Prós                                                  | Contras                                                                                                                                        |
|-------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|
| Pode usar qualquer tarefa de pipeline disponível      | É necessário manter duas conjuntos de ferramentas e suas versões sincronizadas                                                                |
| Sem registro de contêiner                             | Pode levar algum tempo para iniciar, dependendo das ferramentas/dependências necessárias                                                     |
| O agente estará sempre atualizado com correções de segurança | O contêiner de desenvolvimento deve ser sempre construído a cada execução do pipeline de CI, para verificar se as alterações na branch não quebraram nada |

### Executar os pipelines de CI no contêiner de desenvolvimento sem cache de imagem

| Prós                                                                                             | Contras                                                                                                      |
|--------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------|
| Os scripts de utilidade funcionarão imediatamente                                              | É necessário reconstruir o contêiner a cada execução, dado que pode haver alterações na branch em construção |
| As regras usadas (para linting ou testes unitários) serão as mesmas no CI                       | Nem tudo no contêiner é necessário para o pipeline de CI&#185;                                               |
| Sem surpresas para os desenvolvedores, as saídas locais (por exemplo, de linting) serão iguais no CI | Algumas tarefas de pipeline não estarão disponíveis                                                           |
| Todas as ferramentas e suas versões definidas em um único lugar                                 | A construção da imagem para cada execução de pipeline é lenta&#178;                                           |
| Ferramentas/dependências já estão presentes                                                     |                                                                                                               |
| O contêiner de desenvolvimento está sendo testado para incluir todas as novas ferramentas, além de não estar quebrado |                                                                                                               |

> &#185;: o tamanho do contêiner pode ser reduzido exportando a camada que contém apenas as ferramentas necessárias para o pipeline de CI.
>
> &#178;: poderia ser mitigado adicionando o cache de imagem sem usar um registro de contêiner.

### Executar os pipelines de CI no contêiner de desenvolvimento com registro de imagem

| Prós                                                                                                                                                                                                                                                                                            | Contras                                                                                                      |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------|
| Os scripts de utilidade funcionarão imediatamente                                                                                                                                                                                                                                                      | É necessário reconstruir o contêiner a cada execução, dado que pode haver alterações na branch em construção |
| Sem surpresas para os desenvolvedores, as saídas locais (por exemplo, de linting) serão iguais no CI                                                                                                                                                                                              | Nem tudo no contêiner é necessário para o pipeline de CI&#185;                                               |
| As regras usadas (para linting ou testes unitários) serão as mesmas no CI                                                                                                                                                                                                                               | Algumas tarefas de pipeline não estarão disponíveis   &#178;                                                        |
| Todas as ferramentas e suas versões definidas em um único lugar                                                                                                                                                                                                                                        | Exige acesso a um registro de contêiner para hospedar o contêiner no pipeline&#179;                           |
| Ferramentas/dependências já estão presentes                                                                                                                                                                                                                                                          |                                                                                                               |
| O contêiner de desenvolvimento está sendo testado para incluir todas as novas ferramentas, além de não estar quebrado                                                                                                                                                                                                    |                                                                                                               |
| Publicar o contêiner construído a partir do `devcontainer.json` permite referenciá-lo no `cacheFrom` em `devcontainer.json` (consulte a [documentação](https://containers.dev/implementors/json_reference/#image-specific)). Dessa forma, o VS Code usará a imagem publicada como um cache de camada ao construir |                                                                                                               |

> &#185;: o tamanho do contêiner pode ser reduzido exportando a camada que contém apenas as ferramentas necessárias para o pipeline de CI. Isso exigiria construir a imagem sem tarefas.
>
> &#178;: usando jobs de contêiner no AzDO, você pode usar todas as tarefas (pelo que pude perceber). Referência: [Dockerizing DevOps V2 - AzDO container jobs - DEV Community](https://dev.to/eliises/dockerizing-devops-v2-azdo-container-jobs-3hbf)
>
> &#179;: nas ações do GitHub, o token padrão das Ações do GitHub pode ser usado para acessar o GHCR sem configurar um registro separado. Veja o exemplo abaixo.
> **NOTA:** Isso não compila o `Dockerfile` junto com o `devcontainer.json`

```yaml
    - uses: whoan/docker-build-with-cache-action@v5
        id: cache
        with:
          username: $GITHUB_ACTOR
          password: "${{ secrets.GITHUB_TOKEN }}"
          registry: docker.pkg.github.com
          image_name: devcontainer
          dockerfile: .devcontainer/Dockerfile
```
