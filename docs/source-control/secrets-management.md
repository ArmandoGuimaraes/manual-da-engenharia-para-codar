# Trabalhando com Segredosno Source Control

A melhor maneira de evitar a divulgação de segredos é armazená-los em arquivos locais/privados e excluí-los do rastreamento do git com um arquivo [.gitignore](https://git-scm.com/docs/gitignore).
Por exemplo, o seguinte padrão irá excluir todos os arquivos com a extensão `.private.config`:

```bash
# remover configurações privadas
*.private.config
```

Para obter mais detalhes sobre o gerenciamento adequado de credenciais e segredos em source controls, e como lidar com a inclusão acidental de segredos no source control, consulte o documento de [Gerenciamento de Segredos](../continuous-delivery/secrets-management/README.md), que contém informações adicionais, divididas por linguagem.

Como medida de segurança adicional, aplique a [verificação de credenciais](../continuous-integration/dev-sec-ops/secret-management/credential_scanning.md) em seu pipeline de CI/CD.
