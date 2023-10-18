# Executando detect-secrets no Azure DevOps Pipelines

## Visão geral

Neste artigo, você encontrará informações sobre como integrar o detect-secrets do YELP em seu Azure DevOps Pipeline. O código proposto pode fazer parte do processo clássico de CI ou (maneira preferida) validação de build para PRs antes de mesclá-los com o branch `main`.

## Azure DevOps Pipeline

O Azure DevOps Pipeline proposto contém várias etapas descritas abaixo:

1. Definir o Python 3 como padrão.
1. Instalar o detect-secrets usando o pip.
1. Executar a ferramenta detect-secrets.
1. Publicar os resultados no Artifact do Pipeline.
   > OBS: É uma etapa opcional, mas para investigações futuras, o arquivo .json com os resultados pode ser útil.
1. Analisar os resultados do detect-secrets.
   > OBS: Esta etapa faz uma análise simples do arquivo .json. Se algum segredo for detectado, quebre o build com o código de saída 1.

> OBS: O exemplo abaixo tem 2 jobs: para agentes Linux e Windows. Você não precisa usar ambos os jobs - ajuste o pipeline às suas necessidades.
>
> OBS: O exemplo do Windows não usa a versão mais recente do detect-secrets. Isso está relacionado ao bug na ferramenta detect-secrets (veja mais em [Issue#452](https://github.com/Yelp/detect-secrets/issues/452)). É altamente recomendável monitorar a correção do problema e usar a versão mais recente, se possível, removendo a tag de versão `==1.0.3` no comando de instalação do pip.

```yaml
trigger:
  - none

jobs:
  - job: ubuntu
    displayName: "detect-secrets no agente Ubuntu Linux"
    pool:
      vmImage: ubuntu-latest
    steps:
      - task: UsePythonVersion@0
        displayName: "Definir o Python 3 como padrão"
        inputs:
          versionSpec: "3"
          addToPath: true
          architecture: "x64"

      - bash: pip install detect-secrets
        displayName: "Instalar detect-secrets usando pip"

      - bash: |
          detect-secrets --version
          detect-secrets scan --all-files --force-use-all-plugins --exclude-files FETCH_HEAD > $(Pipeline.Workspace)/detect-secrets.json
        displayName: "Executar a ferramenta detect-secrets"

      - task: PublishPipelineArtifact@1
        displayName: "Publicar resultados no Artifact do Pipeline"
        inputs:
          targetPath: "$(Pipeline.Workspace)/detect-secrets.json"
          artifact: "detect-secrets-ubuntu"
          publishLocation: "pipeline"

      - bash: |
          dsjson=$(cat $(Pipeline.Workspace)/detect-secrets.json)
          echo "${dsjson}"

          count=$(echo "${dsjson}" | jq -c -r '.results | length')

          if [ $count -gt 0 ]; then
            msg="Segredos foram detectados no código. ${count} arquivo(s) afetado(s)."
            echo "##vso[task.logissue type=error]${msg}"
            echo "##vso[task.complete result=Failed;]${msg}."
          else
            echo "##vso[task.complete result=Succeeded;]Nenhum segredo detectado."
          fi
        displayName: "Analisar resultados do detect-secrets"

  - job: windows
    displayName: "detect-secrets no agente Windows"
    pool:
      vmImage: windows-latest
    steps:
      - task: UsePythonVersion@0
        displayName: "Definir o Python 3 como padrão"
        inputs:
          versionSpec: "3"
          addToPath: true
          architecture: "x64"

      - script: pip install detect-secrets==1.0.3
        displayName: "Instalar detect-secrets usando pip"

      - script: |
          detect-secrets --version
          detect-secrets scan --all-files --force-use-all-plugins > $(Pipeline.Workspace)/detect-secrets.json
        displayName: "Executar a ferramenta detect-secrets"

      - task: PublishPipelineArtifact@1
        displayName: "Publicar resultados no Artifact do Pipeline"
        inputs:
          targetPath: "$(Pipeline.Workspace)/detect-secrets.json"
          artifact: "detect-secrets-windows"
          publishLocation: "pipeline"

      - pwsh: |
          $dsjson = Get-Content $(Pipeline.Workspace)/detect-secrets.json
          Write-Output $dsjson

          $dsObj = $dsjson | ConvertFrom-Json
          $count = ($dsObj.results | Get-Member -MemberType NoteProperty).Count

          if ($count -gt 0) {
            $msg = "Segredos foram detectados no código. $count arquivo(s) afetado(s). "
            Write-Host "##vso[task.logissue type=error]$msg"
            Write-Host "##vso[task.complete result=Failed;]$msg"


          }
          else {
            Write-Host "##vso[task.complete result=Succeeded;]Nenhum segredo detectado."
          }
        displayName: "Analisar resultados do detect-secrets"
```

