# Análise de Opções: GitOps

Conduzido por: Tess e Jeff

Item de Trabalho no Backlog: #21672

Tomadores de Decisão: Wallace, equipe inteira

## Visão Geral

Para o Memory, estaremos criando uma aplicação nativa em nuvem com infraestrutura como código.
Usaremos o GitOps para Implantação Contínua, por meio de solicitações pull para refletir as alterações na infraestrutura.

Em geral, entre nossas duas opções, uma delas é mais simples e direcionada de uma maneira que acreditamos que atenderia aos requisitos deste projeto.
A outra faz o mesmo, com recursos adicionais que podem ou não valer a configuração e a instalação extras.

### Critérios de Avaliação

1. Estilo do repositório: mono versus multi
2. Aplicação de Políticas
3. Métodos de Implantação
4. Monitoramento de Implantação
5. Controle de Admissão
6. Disponibilidade de Documentação Azure
7. Manutenibilidade
8. Maturidade
9. Interface do Usuário

## Soluções

### Flux

[Flux](https://toolkit.fluxcd.io/) é uma ferramenta criada pela Waveworks e é construída em cima do sistema de extensão da API do Kubernetes, suporta multilocação e integra-se perfeitamente a ferramentas populares como Prometheus.

#### Avaliação dos Critérios de Aceitação do Flux

1. Estilo do repositório: mono versus multi
   - O Flux suporta ambos a partir da versão 2
2. Aplicação de Políticas
   - [A Política Azure está em prévia](https://learn.microsoft.com/en-us/azure/azure-arc/kubernetes/use-azure-policy)
3. Métodos de Implantação
   - Defina um [release](https://toolkit.fluxcd.io/guides/helmreleases/) Helm usando Controladores Helm
   - [Kustomization](https://toolkit.fluxcd.io/get-started/#deploy-podinfo-application) descreve implantações
4. Monitoramento de Implantação
   - O Flux funciona com o Prometheus para monitoramento de implantação, bem como painéis do Grafana.
5. Controle de Admissão
   - O Flux usa o RBAC do Kubernetes para restringir permissões de sincronização.
   - [Usa a conta de serviço para acessar segredos de imagem](https://docs.fluxcd.io/en/1.21.1/faq/#how-do-i-give-flux-access-to-an-image-registry)
6. Disponibilidade de Documentação Azure
   - Boa, melhor quando usado com Operadores Helm
7. Manutenibilidade
   - Gerenciado por meio de arquivos YAML no repositório Git
8. Maturidade
   - [A versão 2 é publicada sob a licença Apache no GitHub](https://github.com/fluxcd/flux2), funciona com Helm v3 e tem commits de PR tão recentes quanto hoje
   - 945 estrelas, 94 forks
9. Interface do Usuário
   - CLI, a opção mais simples e leve

Outros recursos a serem observados (veja mais no site)

- O Flux suporta apenas implantações baseadas em pull, o que significa que deve ser combinado com um operador.
- O Flux pode enviar notificações e receber webhooks para sincronização.
- Avaliações de saúde.
- Gerenciamento de dependências.
- Implantação automática.
- Coleta de lixo.
- Implantação ao confirmar.

#### Variações

##### Controladores

Ambas as opções de controlador são opcionais.

O Controlador Helm busca adicionalmente artefatos do Helm para publicação, veja o diagrama abaixo.

O Controlador Kustomize gerencia o estado e a implantação contínua.

Não decidiremos aqui qual controlador usar, pois isso é um estudo de escolha separado, no entanto, observaremos que o Helm é mais amplamente documentado na documentação do Flux.

##### Flux v1

[Flux v1](https://github.com/fluxcd/flux) está apenas em modo de manutenção e não deve ser mais usado.
Portanto, esta seção não considera a opção v1 como válida.

##### GitOps Toolkit

O Flux v2 é construído em cima do [GitOps Toolkit](https://toolkit.fluxcd.io/components/), no entanto, não avaliamos o uso do [GitOps Toolkit](https://toolkit.fluxcd.io/components/) sozinho, pois isso é para quando você deseja criar seu próprio sistema de CD, o que não é o que queremos.

### ArgoCD com Helm Charts

O ArgoCD é uma ferramenta de Entrega Contínua (CD) declarativa baseada em GitOps para Kubernetes.

#### Avaliação dos Critérios de Aceitação do ArgoCD com Helm

1. Estilo do repositório: mono versus multi
   - O ArgoCD suporta ambos
2. Aplicação de Políticas
   - [A Política Azure está em prévia](https://learn.microsoft.com/en-us/azure/azure-arc/kubernetes/use-azure-policy)
3. Métodos de Implantação
   - Implantação com [Helm](https://argo-cd.readthedocs.io/en/stable/user-guide/helm/) Chart
   - Use o [Kustomize](https://argo-cd.readthedocs.io/en/stable/user-guide/kustomize/) para aplicar algumas pós-renderizações aos modelos de lançamento Helm
4. Monitoramento de Implantação
   - O Argo CD expõe dois conjuntos de [métricas do Prometheus](https://argo-cd.readthedocs.io/en/stable/operator-manual/metrics/) (métricas de aplicativo e métricas do servidor de API) para monitoramento de implantação.
5. Controle de Admissão
   - O ArgoCD usa o recurso RBAC.
     [RBAC](https://argo-cd.readthedocs.io/en/stable/operator-manual/rbac/) requer configuração SSO ou configuração de um ou mais usuários locais.
     Uma vez configurado o SSO ou os usuários locais, é possível definir funções RBAC adicionais
   - O Argo CD não possui seu próprio sistema de gerenciamento de usuários e possui apenas um usuário integrado, o administrador.
     O usuário administrador é um superusuário e tem acesso irrestrito ao sistema
   - [A autorização é tratada por meio de tokens JWT](https://argo-cd.readthedocs.io/en/stable/operator-manual/security/#authentication) e verificação de grupos neles
6

. Disponibilidade de Documentação Azure
   - O Argo tem [documentação](https://argo-cd.readthedocs.io/en/stable/operator-manual/user-management/microsoft/) sobre o Azure AD
7. Maturidade
   - Tem commits de PR tão recentes quanto hoje
   - 5.000 estrelas, 1.100 forks
8. Manutenibilidade
   - Pode ser gerenciado por meio do GitOps
9. Interface do Usuário
   - O ArgoCD tem uma GUI e pode ser usado em vários clusters

Outros recursos a serem observados (veja mais no site)

- O ArgoCD suporta tanto o modelo pull quanto o modelo push para entrega contínua.
- O Argo pode enviar notificações, mas você precisa de uma ferramenta separada para isso.
- O Argo pode receber webhooks.
- Avaliações de saúde.
- Ferramentas de multilocação potencialmente muito mais úteis.
  Gerencia vários projetos, mapeia-os para equipes, etc.
- Integração SSO.
- Coleta de lixo.

![Arquitetura](https://argo-cd.readthedocs.io/en/stable/assets/argocd_architecture.png)

## Resultados

Esta seção deve conter uma tabela que classifica cada solução em relação a cada um dos critérios de avaliação:

| Solução | Estilo de Repositório | Aplicação de Políticas    | Métodos de Implantação            | Monitoramento de Implantação | Controle de Admissão | Documentação Azure              | Manutenibilidade        | Maturidade                                  | Interface do Usuário            |
|----------|-----------------------------|-----------------------------------|----------------------------------|-----------------------------|------------------|----------------------------------|----------------------------|-------------------------------------------|----------------------------------------|
| Flux     | mono, multi               | Política Azure, prévia     | Helm, Kustomize               | Prometheus, Grafana   | RBAC              | Sim, na Azure               | YAML no repositório Git    | 945 estrelas, 94 forks, atualmente mantido | CLI                                           |
| ArgoCD   | mono, multi               | Política Azure, prévia     | Helm, Kustomize, KSonnet, ... | Prometheus, Grafana   | RBAC              | Apenas em seus próprios docs | Manifestos no repositório Git | 5.000 estrelas, 1.100 forks                  | GUI, vários clusters na mesma GUI |

## Decisão

O ArgoCD é mais rico em recursos, suportará mais cenários e será uma ferramenta melhor para colocarmos em nosso arsenal.
Portanto, decidimos neste momento seguir com o ArgoCD.

## Referências

1. [GitOps](https://www.gitops.tech/#:~:text=What%20is%20GitOps?%20GitOps%20is%20a%20way%20of,familiar%20with,%20including%20Git%20and%20Continuous%20Deployment%20tools.)
2. [Política](https://learn.microsoft.com/en-us/azure/azure-arc/kubernetes/use-azure-policy)
3. [Monitoramento](https://learn.microsoft.com/en-us/azure/azure-monitor/insights/container-insights-onboard)
4. [Políticas](https://learn.microsoft.com/en-us/azure/governance/policy/concepts/policy-for-kubernetes)
5. [Implantação](https://learn.microsoft.com/en-us/azure/azure-arc/kubernetes/use-gitops-with-helm)
6. [Push com ArgoCD no Azure DevOps](https://www.linkedin.com/pulse/azure-devops-gitops-aks-argocd-ajay-vikram-singh?articleId=6677601917633355776)
