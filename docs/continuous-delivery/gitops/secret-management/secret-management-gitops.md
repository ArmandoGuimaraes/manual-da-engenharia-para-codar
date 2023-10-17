# Gerenciamento de Senhas com GitOps

Projetos GitOps têm repositórios Git como o ponto central, considerados a fonte da verdade para o gerenciamento tanto da infraestrutura quanto das aplicações. Essa infraestrutura e aplicação exigirão acesso seguro a outros recursos do sistema por meio de senhas.
Fazer commit de senhas em texto claro em repositórios Git é inaceitável, mesmo que os repositórios sejam privados para sua equipe e organização. As equipes precisam de uma maneira segura de lidar com senhas ao usar GitOps.

Existem várias maneiras de gerenciar senhas com GitOps e, em alto nível, podem ser categorizadas em:

1. Senhas criptografadas em repositórios Git
2. Referência a senhas armazenadas em um cofre de chaves externo

> **TLDR**: Fazer referência a senhas em um cofre de chaves externo é a abordagem recomendada. É mais fácil orquestrar a rotação de senhas e mais escalável com vários clusters e/ou equipes.

## Senhas Criptografadas em Repositórios Git

Nessa abordagem, os desenvolvedores criptografam manualmente senhas usando uma chave pública, e a chave só pode ser descriptografada pelo controlador Kubernetes personalizado em execução no cluster de destino. Algumas ferramentas populares para essa abordagem são [Bitnami Sealed Secrets](https://github.com/bitnami-labs/sealed-secrets) e [Mozilla SOPS](https://github.com/mozilla/sops).

Todas as ferramentas de criptografia de senhas compartilham as seguintes características:

- Alterações de senhas são gerenciadas fazendo alterações dentro do repositório GitOps, o que proporciona grande rastreabilidade.
- Todas as senhas podem ser rotacionadas fazendo alterações no GitOps, sem acessar o cluster.
- Elas suportam cenários de GitOps completamente desconectados.
- As senhas são armazenadas criptografadas no repositório GitOps. Se a chave de criptografia privada for vazada e o invasor tiver acesso ao repositório, todas as senhas podem ser descriptografadas.

### [Bitnami Sealed Secrets](https://github.com/bitnami-labs/sealed-secrets)

O Sealed Secrets usa criptografia assimétrica para criptografar senhas. Um controlador Kubernetes gera um par de chaves (privada-pública) e armazena a chave privada no banco de dados `etcd` do cluster como um segredo Kubernetes. Os desenvolvedores usam o [Kubeseal CLI](https://github.com/bitnami-labs/sealed-secrets#public-key--certificate) para selar senhas antes de fazer commit no repositório Git.

Alguns pontos-chave do uso do Sealed Secrets incluem:

- [Suporta rotação automática de chaves](https://github.com/bitnami-labs/sealed-secrets#sealing-key-renewal) para a chave privada e pode ser usada para forçar a recriptografia de senhas.
  - Devido à [renovação automática da chave de selagem](https://github.com/bitnami-labs/sealed-secrets#sealing-key-renewal), a chave deve ser pré-carregada do cluster ou configurada para armazenar a chave de selagem em um local secundário durante a renovação.
- Suporte à multi-tenancy no nível de namespace pode ser imposto pelo controlador.
- Ao selar senhas, os desenvolvedores precisam de uma conexão com o plano de controle do cluster para buscar a chave pública ou a chave pública deve ser explicitamente compartilhada com o desenvolvedor.
- Se a chave privada no cluster for perdida por algum motivo, todas as senhas precisam ser recriptografadas após a geração de um novo par de chaves.
- Não dimensiona bem com múltiplos clusters, pois cada cluster exigirá um controlador com seu próprio par de chaves.
- Só pode criptografar o tipo de recurso `secret`.
- A documentação do Flux tem inconsistências nos exemplos do Azure Key Vault.

### [Mozilla SOPS](https://github.com/mozilla/sops)

O SOPS: Secrets OPerationS é uma ferramenta de criptografia que suporta formatos YAML, JSON, ENV, INI e BINARY e criptografa com AWS KMS, GCP KMS, Azure Key Vault, age e PGP, não se limitando apenas ao Kubernetes. Ele suporta a integração com alguns sistemas comuns de gerenciamento de chaves, incluindo o Azure Key Vault, onde um ou mais sistemas de gerenciamento de chaves são usados para armazenar a chave de criptografia para criptografar senhas e não as próprias senhas.

Alguns pontos-chave do uso do SOPS incluem:

- [Flux](https://toolkit.fluxcd.io/guides/mozilla-sops/#azure) tem suporte nativo ao SOPS com descriptografia no lado do cluster.
- Fornece uma camada adicional de segurança, pois a chave privada usada para descriptografar é protegida em um cofre de chaves externo.
- Para usar o CLI Helm para criptografia, é necessário o plug-in ([Helm Secrets](https://github.com/jkroepke/helm-secrets)).
- Necessita do plug-in ([KSOPS](https://github.com/viaduct-ai/kustomize-sops))([kustomize-sopssecretgenerator](

https://github.com/goabout/kustomize-sopssecretgenerator)) para funcionar com Kustomization.
- Não dimensiona bem com equipes maiores, pois cada desenvolvedor precisa criptografar as senhas.
- A chave pública é suficiente para criar novos arquivos. A chave secreta é necessária para descriptografar e editar arquivos existentes porque o SOPS calcula um código de autenticação de mensagem (MAC) em todos os valores. Ao usar apenas a chave pública para adicionar ou remover um campo, o arquivo inteiro deve ser excluído e recriado.
- Suporta vários tipos de chaves que podem ser usadas em estados conectados e desconectados. Uma senha pode ter uma lista de chaves e tentará descriptografar com todas elas.

## Referência a Senhas Armazenadas em um Cofre de Chaves Externo (Recomendado)

Essa abordagem depende de um sistema de gerenciamento de chaves, como o [Azure Key Vault](https://learn.microsoft.com/pt-br/azure/key-vault/general/overview), para armazenar as senhas, e o manifesto Git nas repositórios faz referência às senhas do cofre de chaves. Os desenvolvedores não realizam nenhuma operação criptográfica com arquivos nos repositórios. Operadores Kubernetes em execução no cluster de destino são responsáveis por recuperar as senhas do cofre de chaves e disponibilizá-las como segredos Kubernetes ou volumes de segredos montados no pod.

Todas as ferramentas abaixo compartilham as seguintes características:

- As senhas não são armazenadas no repositório.
- Suportam métricas Prometheus para observabilidade.
- Suportam sincronização com Segredos Kubernetes.
- Suportam contêineres Linux e Windows.
- Fornecem gerenciamento de segredos externos de nível empresarial.
- Facilmente escaláveis com múltiplos clusters e equipes maiores.
- Ambas as soluções suportam Azure Active Directory (Azure AD) [principal de serviço](https://learn.microsoft.com/pt-br/azure/active-directory/develop/app-objects-and-service-principals) ou [identidade gerenciada](https://learn.microsoft.com/pt-br/azure/active-directory/managed-identities-azure-resources/overview) para [autenticação com o Key Vault](https://learn.microsoft.com/pt-br/azure/key-vault/general/authentication).

Para ideias sobre rotação de senhas, consulte [Rotação de Senhas em Variáveis de Ambiente e Segredos Montados](rotacao-de-senhas-em-pods.md)

Para saber como autenticar registros de contêineres privados com um principal de serviço, consulte: [Registro de Contêiner Privado Autenticado](#authenticated-private-container-registry)

### [Provedor de Azure Key Vault para Secrets Store CSI Driver](https://github.com/Azure/secrets-store-csi-driver-provider-azure)

O Provedor de [Azure Key Vault](https://docs.microsoft.com/pt-br/azure/key-vault/general/overview) (AKVP) para [Driver de Armazenamento de Segredos CSI para Kubernetes](https://github.com/kubernetes-sigs/secrets-store-csi-driver) permite que você obtenha o conteúdo de segredos armazenados em uma instância do Azure Key Vault e use a interface Secrets Store CSI driver para montá-los nos pods do Kubernetes. Monta segredos/chaves/certificados no pod usando um volume Inline CSI.

Guia de instalação do Provedor de Azure Key Vault para Secrets Store CSI Driver [aqui](https://secrets-store-csi-driver.sigs.k8s.io/getting-started/installation.html).

O driver CSI precisará de acesso ao Azure Key Vault, seja por meio de um principal de serviço ou identidade gerenciada (recomendado). Para tornar esse acesso seguro, você pode aproveitar a [Identidade de Carga de Trabalho do Azure AD](https://github.com/Azure/azure-workload-identity)(recomendado) ou [Identidade de Pod do AAD](https://github.com/Azure/aad-pod-identity). Observe que a identidade de pod do AAD em breve será substituída pela identidade de carga de trabalho.

Links do Grupo de Produtos fornecidos para AKVP com SSCSID:

  1. Diferenças entre ESO / SSCSID ([GitHub Issue](https://github.com/external-secrets/external-secrets/issues/478))
  2. Palestra sobre Gerenciamento de Segredos no K8S [aqui](https://www.youtube.com/watch?v=EW25WpErCmA) (Segredos Nativos, Vault.io e ESO vs. SSCSID)

Vantagens:

- Suporta a portabilidade de pods com o SecretProviderClass CRD
- Suporta a rotação automática de segredos com intervalos de sincronização personalizáveis por **cluster**.
- Parece ser a escolha da Microsoft (o driver Secrets Store CSI é fortemente contribuído pela [MSFT](https://github.com/kubernetes-sigs/secrets-store-csi-driver) e pelo Kubernetes-SIG)

Desvantagens:

- [Suporte ausente para cenários desconectados](https://github.com/kubernetes-sigs/secrets-store-csi-driver/issues/446): Quando o nó está offline, o SSCSID falha ao buscar o segredo e, portanto, a montagem do volume falha, tornando a escalabilidade e a reinicialização de pods impossíveis enquanto estiver offline
- O AKVP só pode acessar o Key Vault de um ambiente não-Azure usando um [principal de serviço](https://azure.github.io/secrets-store-csi-driver-provider-azure/docs/configurations/identity-access-modes/service-principal-mode/#configure-service-principal-to-access-keyvault)
- O [Segredo Kubernetes contendo as credenciais do principal de serviço](https://azure.github.io/secrets-store-csi-driver-provider-azure/docs/configurations/identity-access-modes/service-principal-mode/) precisa ser criado como um segredo no mesmo namespace do pod de aplicação. Se os pods em vários namespaces precisarem usar o mesmo SP para acessar o Key Vault, este Segredo Kubernetes precisa ser criado em cada namespace.
- O repositório GitOps deve conter o nome do Key Vault dentro do SecretProviderClass
- Deve montar segredos como volumes para permitir a sincronização em Segredos Kubernetes
- Usa mais recursos (4 pods; driver de armazenamento CSI e provedor) e é um daemonset - não testado quanto ao uso de recursos/RPS.

### [Operador de Segredos Externos com Azure Key Vault](https://external-secrets.io/)

O Operador de Segredos Externos (ESO) é um operador Kubernetes de código aberto que pode ler segredos de armazenamentos de segred

os externos (por exemplo, Azure Key Vault) e sincronizá-los com Segredos Kubernetes. Em contraste com o Driver CSI, o controlador ESO cria os segredos no cluster como Segredos K8s, em vez de montá-los como volumes em pods.

Documentação sobre o uso do provedor Azure Key Vault do ESO [aqui](https://external-secrets.io/v0.8.1/provider/azure-key-vault/).

O ESO precisará de acesso ao Azure Key Vault, seja por meio de um principal de serviço ou identidade gerenciada (por meio da [Identidade de Carga de Trabalho do Azure AD](https://github.com/Azure/azure-workload-identity)(recomendado) ou [Identidade de Pod do AAD](https://github.com/Azure/aad-pod-identity)).

Vantagens:

- Suporta a rotação automática de segredos com intervalos de sincronização personalizáveis por **segredo**.
- Os componentes são divididos em diferentes CRDs para namespace (ExternalSecret, SecretStore) e cluster-wide (ClusterSecretStore, ClusterExternalSecret), tornando a sincronização mais gerenciável em relação a diferentes implantações/pods, etc.
- O segredo do principal de serviço para os (Cluster)SecretStores pode ser colocado em um namespace que apenas o ESO pode acessar (veja [Shared ClusterSecretStore](https://external-secrets.io/v0.7.1/guides/multi-tenancy/)).
- Eficiente em recursos (um único pod) - não testado quanto ao uso de recursos/RPS.
- Código aberto e alto número de contribuições, ([GitHub](https://github.com/external-secrets/external-secrets))
- A montagem de segredos como volumes é suportada por meio das APIs do K8S (veja [aqui](https://kubernetes.io/docs/concepts/configuration/secret/#using-secrets-as-files-from-a-pod))
- Suporta cenários parcialmente desconectados: como o ESO usa segredos nativos do K8s, o cluster pode estar offline e isso não afeta a reinicialização e a escalabilidade de pods enquanto estiver offline

Desvantagens:

- O repositório GitOps deve conter o nome do Key Vault dentro do SecretStore / ClusterSecretStore ou um ConfigMap que faça referência a ele.
- Deve criar segredos como segredos K8s.

## Links Importantes

- [Sealed Secrets com Flux v2](https://toolkit.fluxcd.io/guides/sealed-secrets/)
- [Mozilla SOPS com Flux v2](https://toolkit.fluxcd.io/guides/mozilla-sops/)
- [Gerenciamento de Segredos com Argo CD](https://argo-cd.readthedocs.io/en/stable/operator-manual/secret-management/)
- [Fluxo de Trabalho de Gerenciamento de Segredos](https://www.youtube.com/watch?v=-k6HEXaE75k)

## Apêndice
### Registro de Contêiner Privado Autenticado

Uma opção de como autenticar registros de contêiner privados (por exemplo, ACR):

1. Use um Segredo Kubernetes `dockerconfigjson` no nível do pod com `ImagePullSecret` (Isso também pode ser definido no [nível do namespace](https://kubernetes.io/docs/concepts/containers/images/#referring-to-an-imagepullsecrets-on-a-pod))

Espero que esta tradução seja útil para você. Se tiver alguma dúvida adicional ou precisar de mais informações, sinta-se à vontade para perguntar.
