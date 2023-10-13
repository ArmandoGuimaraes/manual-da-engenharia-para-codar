# Construindo Containers com Azure DevOps usando o Padrão DevTest

Neste documento, destacamos os aprendizados obtidos ao aplicar o padrão DevTest ao desenvolvimento de containers no Azure DevOps por meio de pipelines.

O padrão nos permitiu construir containers para desenvolvimento, teste e liberação do container para reutilização posterior (pronto para produção).

Vamos explorar as ferramentas necessárias para construir, testar e enviar um container, nosso ambiente e passar por cada etapa separadamente.

Siga este link para aprofundar ou revisitar o [padrão DevTest](https://learn.microsoft.com/en-us/azure/architecture/solution-ideas/articles/dev-test-paas).

## Índice

[Construir o Container](#construir-o-container)
[Testar o Container](#testar-o-container)
[Enviar Container](#enviar-container)
[Referências](#referências)

## Construir o Container

O primeiro passo no desenvolvimento de containers, após criar os Dockerfiles e o código-fonte necessários, é construir o container. Até mesmo o próprio Dockerfile pode incluir alguns testes básicos. Os testes de código são realizados ao enviar o código para o repositório de origem, onde ele é então usado para construir o container.

A primeira etapa em nosso pipeline é executar o comando `docker build` com uma tag temporária e os argumentos de construção necessários:

```yaml
# ... código omitido para brevidade
```

Essa tarefa inclui os parâmetros `buildDirectory`, `imageName` e `dockerfileName`, que devem ser definidos previamente.
Essa tarefa pode, por exemplo, ser usada em um modelo para vários containers para melhorar a reutilização de código.

Também é possível passar variáveis de ambiente diretamente para o Dockerfile por meio da seção `env` da tarefa.

Se essa tarefa for bem-sucedida, o Dockerfile foi construído sem erros e podemos continuar testando o próprio container.

## Testar o Container

Para testar o container, estamos usando o ambiente tox.
Para mais detalhes sobre o tox, visite a seção tox deste repositório ou visite [a página oficial de documentação do tox](https://tox.wiki/en/latest/user_guide.html).

Antes de testarmos o container, estamos verificando se há credenciais expostas no histórico da imagem docker.
Se senhas conhecidas, usadas para acessar nossos recursos internos, estiverem expostas aqui, a etapa de construção falhará:

```yml
# ... código omitido para brevidade
```

Após o teste de credencial, o container é testado por meio da extensão pytest [testinfra](https://testinfra.readthedocs.io/en/latest/).
Testinfra é uma ferramenta baseada em Python que pode ser usada para iniciar um container, reunir pré-requisitos, testar o container e desligá-lo novamente, sem nenhum esforço além de escrever os testes. Esses testes podem, por exemplo, incluir:

- se arquivos existem
- se variáveis de ambiente estão configuradas corretamente
- se certos processos estão em execução
- se o ambiente host correto está sendo usado

Para uma coleção completa de capacidades e requisitos, visite [o projeto testinfra no GitHub](https://github.com/pytest-dev/pytest-testinfra).

Alguns métodos de um teste de container baseado em Linux podem parecer assim:

```python
# ... código omitido para brevidade
```

Para iniciar o teste, um comando [pytest](http://pytest.org) é executado por meio do tox.

Uma tarefa contendo o comando tox pode parecer assim:

```yaml
# ... código omitido para brevidade
```

O que poderia acionar o seguinte código pytest, que está contido no arquivo tox.ini:

```bash
# ... código omitido para brevidade
```

Como última tarefa deste pipeline para construir e testar o container, definimos uma variável chamada `testsPassed`, que é apenas `true`, se as tarefas anteriores tiverem sido bem-sucedidas:

```yml
# ... código omitido para brevidade
```

## Enviar Container

Após construir e testar, se nosso container funcionar conforme o esperado, queremos liberá-lo para nosso Azure Container Registry (ACR) para ser usado por nossa aplicação maior. Antes disso, queremos automatizar o comportamento de envio e definir uma tag significativa.

Como desenvolvedor, muitas vezes é útil ter containers enviados para o ACR, mesmo que estejam falhando.
Isso pode ser feito verificando a variável `testsPassed` que introduzimos no final de nossos testes.

Se o teste falhou, queremos adicionar um sufixo de falha no final da tag:

```yml
# ... código omitido para brevidade
```

A condição verifica se o valor de `testsPassed` é `false` e também se não estamos na *main branch*, pois não queremos enviar containers falhos da main.
Isso nos ajuda a manter nosso ambiente de produção limpo.

O valor para imageRepository foi definido em outro modelo, junto com o `failedSuffix` e `testsPassed`:

```yml
# ... código omitido para brevidade
```

A imageTag está aberta para discussão, pois depende muito de como sua equipe deseja usar o container. Optamos por `Build.SourceVersion`, que é o ID do commit da branch em que o container foi desenvolvido.
Isso permite que você rastreie facilmente a origem do container e auxilie na depuração.

Um link para as variáveis predefinidas do Azure DevOps pode ser encontrado na [Documentação do Azure sobre Azure DevOps](https://learn.microsoft.com/en-us/azure/devops/pipelines/build/variables?view=azure-devops&tabs=yaml#build-variables-devops-services).

Após adicionar uma tag ao container, a imagem deve ser enviada.
Isso pode ser feito com a seguinte tarefa:

```yml
# ... código omitido para brevidade
``

`

Da mesma forma, estas são as etapas para publicar o container no ACR, se os testes forem bem-sucedidos:

```yml
# ... código omitido para brevidade
```

Se você não quiser incluir a tag `latest`, também pode remover as etapas envolvendo latest (SetLatestSuffixTag & pushSuccessfulDockerImageLatest).

## Referências

- [Padrão DevTest](https://learn.microsoft.com/en-us/azure/architecture/solution-ideas/articles/dev-test-paas)
- [Documentação do Azure sobre Azure DevOps](https://learn.microsoft.com/en-us/azure/devops/pipelines/build/variables?view=azure-devops&tabs=yaml#build-variables-devops-services)
- [Página oficial de documentação do tox](https://tox.wiki/en/latest/user_guide.html)
- [Testinfra](https://testinfra.readthedocs.io/en/latest/)
- [Projeto Testinfra no GitHub](https://github.com/pytest-dev/pytest-testinfra)
- [pytest](http://pytest.org)
