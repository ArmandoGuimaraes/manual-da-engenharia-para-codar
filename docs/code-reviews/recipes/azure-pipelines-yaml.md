# Revisão de Código YAML (Azure Pipelines)

## Guia de Estilo

Os desenvolvedores devem seguir a [referência do esquema YAML](https://learn.microsoft.com/en-us/azure/devops/pipelines/yaml-schema?view=azure-devops&tabs=schema%2Cparameter-schema).

## Análise de Código / Linting

O linter YAML mais popular é a extensão [YAML](https://marketplace.visualstudio.com/items?itemName=redhat.vscode-yaml). Esta extensão fornece validação YAML, destaque de documentos, auto-completar, suporte de hover e recursos de formatação.

## Extensões do VS Code

Existe uma extensão [Azure Pipelines para o VS Code](https://marketplace.visualstudio.com/items?itemName=ms-azure-devops.azure-pipelines) para adicionar realce de sintaxe e autocompletar para o YAML das Pipelines do Azure no VS Code. Ele também ajuda a configurar compilação e implantação contínuas para aplicativos da Web do Azure sem sair do VS Code.

## Visão Geral do YAML nas Pipelines do Azure

Quando a pipeline é acionada, antes de executar a pipeline, existem algumas fases, como [Tempo de Fila, Tempo de Compilação e Tempo de Execução](https://adamtheautomator.com/azure-devops-variables/#Pipeline_Execution_Phases), onde as variáveis são interpretadas pelo [sintaxe de expressão em tempo de execução](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/variables?view=azure-devops&tabs=yaml%2Cbatch#runtime-expression-syntax).

Quando a pipeline é acionada, todos os arquivos YAML aninhados são expandidos para serem executados nas Pipelines do Azure. Esta lista de verificação contém algumas dicas para revisar todos os arquivos YAML aninhados.

Estes documentos podem ser úteis ao revisar os arquivos YAML:

- [Documentação YAML das Pipelines do Azure](https://learn.microsoft.com/en-us/azure/devops/pipelines/yaml-schema).
- [Sequência de execução da pipeline](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/runs?view=azure-devops)
- [Principais conceitos para as novas Pipelines do Azure](https://learn.microsoft.com/en-us/azure/devops/pipelines/get-started/key-pipelines-concepts?view=azure-devops)

**Visão geral dos principais conceitos das Pipelines do Azure**
![Conceitos-chave das Pipelines do Azure](images/key-concepts-overview.png)

- Um acionador diz a uma Pipeline para ser executada.
- Uma pipeline é composta por um ou mais estágios. Uma pipeline pode ser implantada em um ou mais ambientes.
- Um estágio é uma maneira de organizar trabalhos em uma pipeline e cada estágio pode ter um ou mais trabalhos.
- Cada trabalho é executado em um agente. Um trabalho também pode ser sem agente.
- Cada agente executa um trabalho que contém uma ou mais etapas.
- Uma etapa pode ser uma tarefa ou um script e é a menor unidade de construção de uma pipeline.
- Uma tarefa é um script pré-empacotado que realiza uma ação, como invocar uma API REST ou publicar um artefato de compilação.
- Um artefato é uma coleção de arquivos ou pacotes publicados por uma execução.

## Lista de Verificação de Revisão de Código

Além da [Lista de Verificação de Revisão de Código](../process-guidance/reviewer-guidance.md), você também deve procurar por itens específicos da revisão de código YAML das Pipelines do Azure.

### Estrutura da Pipeline

- [ ] Os passos são bem compreendidos e os componentes são facilmente identificáveis. Certifique-se de que há uma descrição adequada `displayName:` para cada etapa na pipeline.
- [ ] Etapas/estágios da pipeline são verificados nas Pipelines do Azure para ter uma compreensão maior dos componentes.
- [ ] No caso de ter arquivos YAML aninhados complexos, a pipeline nas Pipelines do Azure é editada para encontrar o arquivo raiz do acionador.
- [ ] Todas as referências de arquivos de modelo são visitadas para garantir que uma pequena alteração não cause alterações quebradas, alterar um arquivo pode afetar várias pipelines
- [ ] Scripts longos incorporados no arquivo YAML são movidos para arquivos de script

### Estrutura YAML

- [ ] Componentes reutilizáveis são divididos em modelos YAML separados.
- [ ] Variáveis são separadas por ambiente armazenadas em modelos ou grupos de variáveis.
- [ ] Mudanças de valor de variável em `Tempo de Fila`, `Tempo de Compilação` e `Tempo de Execução` são consideradas.
- [ ] Valores de sintaxe de variável usados com `Sintaxe de Macro`, `Sintaxe de Expressão de Modelo` e `Sintaxe de Expressão em Tempo de Execução` são considerados.
- [ ] Variáveis não utilizadas/parâmetros são removidos na pipeline.
- [ ] A pipeline atende aos critérios de `Condições` do estágio/trabalho?

### Verificação de Permissão e Segurança

- [ ] Valores secretos não devem ser impressos na pipeline, `issecret` é usado para imprimir segredos para depuração.
- [ ] Se a pipeline estiver usando grupos de variáveis na Biblioteca, verifique se a pipeline tem acesso aos grupos de variáveis criados.
- [ ] Se a pipeline tiver uma tarefa remota em outro repositório/organização, ela tem acesso?
- [ ] Se a pipeline estiver tentando acessar um arquivo seguro, ela tem permissão?
- [ ] Se a pipeline exigir aprovação para implantações em ambientes, quem é o aprovador?
- [ ] É necessário manter segredos e gerenciá-los, você considerou o uso do Azure KeyVault?

### Dicas de Solução de Problemas

- Considere a Sintaxe de Variável com [Expressões em Tempo de Execução](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/variables?view=azure-devops&tabs=yaml%2Cbatch#runtime-expression-syntax) na pipeline. Aqui está um bom exemplo para entender a [Expansão de variáveis](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/variables?view=azure-devops&tabs=yaml%2Cbatch#expansion-of-variables).

- Quando atribuímos uma variável como abaixo, ela não será definida durante o tempo de inicialização, será atribuída durante a execução, e então podemos recuperar alguns er

ros com base no momento em que o modelo é executado.

  ```yaml
  - task: AzureWebApp@1
    displayName: 'Implantar Azure Web App : $(webAppName)'
    inputs:
      azureSubscription: '$(azureServiceConnectionId)'
      appName: '$(webAppName)'
      package: $(Pipeline.Workspace)/drop/Application$(Build.BuildId).zip
      startUpCommand: 'gunicorn --bind=0.0.0.0 --workers=4 app:app'
  ```

  Erro:

  ![Problema de autorização devido ao tempo de inicialização](images/authorization_issue_due_to_initialize_time.png)

  Após passar essas variáveis como parâmetro, ela carrega os valores corretamente.

  ```yaml
    - template: steps-deployment.yaml
      parameters:
        azureServiceConnectionId: ${{ variables.azureServiceConnectionId  }}
        webAppName: ${{ variables.webAppName  }}
  ```

  ```yaml
  - task: AzureWebApp@1
    displayName: 'Implantar Azure Web App :${{ parameters.webAppName }}'
    inputs:
      azureSubscription: '${{ parameters.azureServiceConnectionId }}'
      appName: '${{ parameters.webAppName }}'
      package: $(Pipeline.Workspace)/drop/Application$(Build.BuildId).zip
      startUpCommand: 'gunicorn --bind=0.0.0.0 --workers=4 app:app'
  ```

- Use `issecret` para imprimir segredos para depuração

  ```bash
  echo "##vso[task.setvariable variable=token;issecret=true]${token}"
  ```
