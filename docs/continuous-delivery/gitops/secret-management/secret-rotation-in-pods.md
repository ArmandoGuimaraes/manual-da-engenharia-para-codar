# Rotação de Segredos de Variáveis de Ambiente e Segredos Montados em Pods

Este documento aborda algumas maneiras de realizar a rotação de segredos com variáveis de ambiente e segredos montados em pods do Kubernetes.

## Mapeamento de Segredos via secretKeyRef com Variáveis de Ambiente

Se mapearmos um segredo nativo do Kubernetes via `secretKeyRef` em uma variável de ambiente e rotacionarmos as chaves, a variável de ambiente não é atualizada, mesmo que o segredo nativo do Kubernetes tenha sido atualizado. Precisamos reiniciar o Pod para que as alterações sejam populadas. O [Reloader](https://github.com/stakater/Reloader) resolve esse problema com um controlador do Kubernetes.

```yaml
...
    env:
        - name: EVENTHUB_CONNECTION_STRING
          valueFrom:
            secretKeyRef:
              name: poc-creds
              key: EventhubConnectionString
...
```

## Mapeamento de Segredos via volumeMounts (Abordagem do ESO)

Se mapearmos um segredo nativo do Kubernetes via um volume mount e rotacionarmos as chaves, o arquivo é atualizado. A aplicação precisa ser capaz de captar as alterações sem a necessidade de reiniciar (provavelmente exigindo lógica personalizada na aplicação para oferecer suporte a isso). Nesse caso, não é necessária a reinicialização da aplicação.

```yaml
...
    volumeMounts:
    - name: mounted-secret
      mountPath: /mnt/secrets-store
      readOnly: true
  volumes:
  - name: mounted-secret
    secret:
      secretName: poc-creds
...
```

## Mapeamento de Segredos via volumeMounts (Abordagem do AKVP SSCSID)

O SSCSID concentra-se em montar segredos externos no CSI. Portanto, se rotacionarmos as chaves, o arquivo é atualizado. A aplicação precisa ser capaz de captar as alterações sem a necessidade de reiniciar (provavelmente exigindo lógica personalizada na aplicação para oferecer suporte a isso). Nesse caso, não é necessária a reinicialização da aplicação.

```yaml
...
    volumeMounts:
    - name: app-secrets-store-inline
      mountPath: "/mnt/app-secrets-store"
      readOnly: true
  volumes:
  - name: app-secrets-store-inline
    csi:
      driver: secrets-store.csi.k8s.io
      readOnly: true
      volumeAttributes:
        secretProviderClass: akvp-app
      nodePublishSecretRef:
        name: secrets-store-sp-creds
...
```
