# Utilizando Azurite para Executar Testes de Armazenamento Blob em um Pipeline

Este documento determina a abordagem para escrever testes automatizados com um ciclo de feedback curto (ou seja, testes unitários) contra considerações de segurança (endpoints privados) para a funcionalidade do Azure Blob Storage.

Uma vez que os endpoints privados são ativados para as contas de armazenamento do Azure, os testes atuais falharão quando executados localmente ou como parte de um pipeline, pois essa conexão será bloqueada.

## Utilize um emulador de armazenamento Azure - Azurite

Para emular um Azure Blob Storage local, podemos usar o [Azure Storage Emulator](https://learn.microsoft.com/en-us/azure/storage/common/storage-use-emulator). O Storage Emulator atualmente roda apenas no Windows. Se você precisar de um Storage Emulator para Linux, uma opção é o emulador de armazenamento de código aberto mantido pela comunidade [Azurite](https://github.com/azure/azurite).

> *O Azure Storage Emulator não está mais sendo ativamente desenvolvido.* [**Azurite**](https://learn.microsoft.com/en-us/azure/storage/common/storage-use-azurite) *é a plataforma de emulador de armazenamento daqui para frente. Azurite substitui o Azure Storage Emulator. Azurite continuará a ser atualizado para suportar as versões mais recentes das APIs de armazenamento Azure. Para mais informações, veja* [**Use o emulador Azurite para desenvolvimento local de armazenamento Azure**](https://learn.microsoft.com/en-us/azure/storage/common/storage-use-azurite)*.*

Existem algumas diferenças de funcionalidade entre o Storage Emulator e os serviços de armazenamento Azure. Para mais informações sobre essas diferenças, consulte [Diferenças entre o Storage Emulator e o armazenamento Azure](https://learn.microsoft.com/en-us/azure/storage/common/storage-use-emulator#differences-between-the-storage-emulator-and-azure-storage).

Há várias maneiras de instalar e executar o Azurite em seu sistema local, conforme listado [aqui](https://learn.microsoft.com/en-us/azure/storage/common/storage-use-azurite#install-and-run-azurite-by-using-npm). Neste documento, abordaremos `Instalar e executar o Azurite usando NPM` e `Instalar e executar a imagem Docker do Azurite`.

## 1. Instalar e executar o Azurite

### a. Usando NPM

Para executar o Azurite V3, você precisa ter o Node.js >= 8.0 instalado em seu sistema. O Azurite funciona de forma multiplataforma no Windows, Linux e OS X.

Após a instalação do Node.js, você pode instalar o Azurite simplesmente com npm, que é a ferramenta de gerenciamento de pacotes Node.js incluída em cada instalação do Node.js.

```bash
# Instalar o Azurite
npm install -g azurite

# Criar o diretório azurite
mkdir c:/azurite

# Iniciar o Azurite para Windows
azurite --silent --location c:\azurite --debug c:\azurite\debug.log
```

A saída será:

```shell
O serviço Blob do Azurite está iniciando em http://127.0.0.1:10000
O serviço Blob do Azurite está ouvindo com sucesso em http://127.0.0.1:10000
O serviço de fila do Azurite está iniciando em http://127.0.0.1:10001
O serviço de fila do Azurite está ouvindo com sucesso em http://127.0.0.1:10001
```

### b. Usando uma imagem docker

Outra forma de executar o Azurite é usando o docker, usando o endpoint `HTTP` padrão.

```bash
docker run -p 10000:10000 mcr.microsoft.com/azure-storage/azurite azurite-blob --blobHost 0.0.0.0
```

O Docker Compose é outra opção e pode executar a mesma imagem docker usando o arquivo `docker-compose.yml` abaixo.

```yaml
version: '3.4'
services:
  azurite:
    image: mcr.microsoft.com/azure-storage/azurite
    hostname: azurite
    volumes:
      - ./cert/azurite:/data
    command: "azurite-blob --blobHost 0.0.0.0 -l /data --cert /data/127.0.0.1.pem --key /data/127.0.0.1-key.pem --oauth basic"
    ports:
      - "10000:10000"
      - "10001:10001"
```

## 2. Executar testes em sua máquina local

O Python 3.8.7 é usado para isso, mas deve funcionar bem em outras versões 3.x também.

1. Instale e execute o Azurite para testes locais:

   Opção 1: usando npm:

   ```bash
   # Instalar o Azurite
   npm install -g azurite
   # Criar o diretório azurite
   mkdir c:/azurite
   # Iniciar o Azurite para Windows
   azurite --silent --location c:\azurite --debug c:\azurite\debug.log
   ```

   Opção 2: usando docker

   ```bash
   docker run -p 10000:10000 mcr.microsoft.com/azure-storage/azurite azurite-blob --blobHost 0.0.0.0
   ```

1. No Azure Storage Explorer, selecione `Anexar a um emulador local`

   ![conectar blob](images/blob_storage_connection.png)

1. Forneça um nome para exibição e número da porta, então sua conexão estará pronta, e você poderá usar o Storage Explorer para gerenciar seu armazenamento blob local.

   ![anexar ao local](images/blob_storage_connection_attach.png)

   Para testar e ver como esses endpoints estão funcionando, você pode anexar seu armazenamento blob local ao [**Azure Storage Explorer**](https://azure.microsoft.com/en-us/features/storage-explorer/).

1. Crie um ambiente virtual python
   `python -m venv .venv`

1. Nome do contêiner e inicialize as variáveis de ambiente: Use conftest.py para integração de teste.

   ```python
   from azure.storage.blob import BlobServiceClient
   import os

   def pytest_generate_tests(metafunc):
      os.environ['STORAGE_CONNECTION_STRING'] = 'DefaultEndpointsProtocol=http;AccountName=devstoreaccount1;AccountKey=Eby8vdM02x

NOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==;BlobEndpoint=http://127.0.0.1:10000/devstoreaccount1;'
      os.environ['STORAGE_CONTAINER'] = 'test-container'

      # Criar contêiner para Azurite na primeira execução
      blob_service_client = BlobServiceClient.from_connection_string(os.environ.get("STORAGE_CONNECTION_STRING"))
      try:
         blob_service_client.create_container(os.environ.get("STORAGE_CONTAINER"))
      except Exception as e:
         print(e)
   ```

   **Nota: o valor para `STORAGE_CONNECTION_STRING` é o valor padrão para o Azurite, não é uma chave privada*

1. Instale as dependências

    `pip install -r requirements_tests.txt`

1. Execute os testes:

   ```bash
   python -m pytest ./tests
   ```

Após executar os testes, você pode ver os arquivos em seu armazenamento blob local

![https local blob](images/http_local_blob_storage.png)

## 3. Executar testes no Azure Pipelines

Após executar os testes localmente, precisamos garantir que esses testes também passem no Azure Pipelines. Temos 2 opções aqui, podemos usar a imagem docker como agente hospedado no Azure ou instalar um pacote npm nas etapas do Pipeline.

```bash
trigger:
- master

steps:
- task: UsePythonVersion@0
  displayName: 'Usar Python 3.7'
  inputs:
    versionSpec: 3.7

- bash: |
    pip install -r requirements_tests.txt
  displayName: 'Configurar requisitos para testes'
  
- bash: |
    sudo npm install -g azurite
    sudo mkdir azurite
    sudo azurite --silent --location azurite --debug azurite\debug.log &
  displayName: 'Instalar e Executar o Azurite'

- bash: |
    python -m pytest --junit-xml=unit_tests_report.xml --cov=tests --cov-report=html --cov-report=xml ./tests
  displayName: 'Executar Testes'

- task: PublishCodeCoverageResults@1
  inputs:
    codeCoverageTool: Cobertura
    summaryFileLocation: '$(System.DefaultWorkingDirectory)/**/coverage.xml'
    reportDirectory: '$(System.DefaultWorkingDirectory)/**/htmlcov'

- task: PublishTestResults@2
  inputs:
    testResultsFormat: 'JUnit'
    testResultsFiles: '**/*_tests_report.xml'
    failTaskOnFailedTests: true
```

Uma vez que configuramos nosso pipeline no Azure Pipelines, o resultado será como abaixo

![azure pipelines](images/azure_pipeline.png)

## Referências

- [Azure Storage Emulator](https://learn.microsoft.com/en-us/azure/storage/common/storage-use-emulator)
- [Azurite no GitHub](https://github.com/azure/azurite)
- [Use o emulador Azurite para desenvolvimento local de armazenamento Azure](https://learn.microsoft.com/en-us/azure/storage/common/storage-use-azurite)
- [Azure Storage Explorer](https://azure.microsoft.com/en-us/features/storage-explorer/)
