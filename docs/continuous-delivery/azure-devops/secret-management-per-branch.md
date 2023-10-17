# Azure DevOps: Gerenciando Configurações de Forma Branch Específica

Ao utilizar o [Azure DevOps Pipelines](https://azure.microsoft.com/en-us/services/devops/pipelines/) para CI/CD, é conveniente aproveitar as [variáveis de pipeline](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/variables) integradas para [gerenciamento de segredos](../secrets-management/README.md), mas o uso de variáveis de pipeline para gerenciamento de segredos tem suas desvantagens:

- *Variáveis de pipeline são gerenciadas fora do código que faz referência a elas.* Isso torna fácil introduzir divergências entre o código-fonte e os segredos, por exemplo, ao adicionar uma referência a um novo segredo no código, mas esquecer de adicioná-lo às variáveis de pipeline (o que leva a quebras de compilação confusas) ou excluir uma referência a um segredo no código e esquecer de removê-lo das variáveis de pipeline (o que leva a variáveis de pipeline confusas).

- *Variáveis de pipeline são um estado global compartilhado.* Isso pode levar a situações confusas e problemas difíceis de depurar quando os desenvolvedores fazem alterações simultâneas nas variáveis de pipeline que podem se sobrepor umas às outras. Ter um único conjunto global de variáveis de pipeline também torna impossível que os segredos variem por ambiente (por exemplo, ao usar um modelo de implantação baseado em branches, onde 'master' faz implantações usando os segredos de produção, 'development' faz implantações usando os segredos de staging, e assim por diante).

Uma solução para essas limitações é gerenciar os segredos no repositório Git em conjunto com o código-fonte do projeto. Conforme descrito em [gerenciamento de segredos](../secrets-management/README.md), não inclua segredos no repositório em texto simples. Em vez disso, podemos adicionar uma versão criptografada de nossos segredos ao repositório e permitir que nossos agentes CI/CD e desenvolvedores descriptografem os segredos para uso local com alguma chave pré-compartilhada. Isso nos oferece o melhor dos dois mundos: um armazenamento seguro para segredos, bem como o gerenciamento lado a lado de segredos e código.

```sh
# Primeiro, certifique-se de nunca cometer nossos segredos em texto simples e gere uma chave de criptografia forte
echo ".env" >> .gitignore
ENCRYPTION_KEY="$(LC_ALL=C < /dev/urandom tr -dc '_A-Z-a-z-0-9' | head -c128)"

# Agora, vamos adicionar algum segredo ao nosso arquivo .env
echo "MEU_SEGREDO=..." >> .env

# Atualize também nosso arquivo de documentação de segredos
cat >> .env.template <<< "
# Insira a descrição do seu segredo aqui
MEU_SEGREDO=
"

# Em seguida, criptografe os segredos em texto simples; o arquivo resultante .env.enc pode ser comitado com segurança para o repositório
echo "${ENCRYPTION_KEY}" | openssl enc -aes-256-cbc -md sha512 -pass stdin -in .env -out .env.enc
git add .env.enc .env.template
git commit -m "Atualizar segredos"
```

Ao executar o CI/CD, o servidor de compilação agora pode acessar os segredos descriptografando-os. Por exemplo, para o Azure DevOps, configure `ENCRYPTION_KEY` como uma [variável de pipeline secreta](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/variables#secret-variables) e, em seguida, adicione a seguinte etapa ao arquivo `azure-pipelines.yml`:

```yaml
steps:
  - script: echo "$(ENCRYPTION_KEY)" | openssl enc -aes-256-cbc -md sha512 -pass stdin -in .env.enc -out .env -d
    displayName: Descriptografar segredos
```

Você também pode usar [grupos de variáveis vinculados diretamente ao Azure Key Vault](https://learn.microsoft.com/en-us/azure/devops/pipelines/library/variable-groups?view=azure-devops&tabs=yaml#link-secrets-from-an-azure-key-vault) para seus pipelines para gerenciar todos os segredos em um local.
