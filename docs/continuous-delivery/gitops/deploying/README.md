# Implantação com GitOps

## O que é GitOps?

GitOps é uma forma de gerenciar sua infraestrutura e aplicativos para que todo o sistema seja descrito de forma declarativa e controlada por versões (provavelmente em um repositório Git) e tenha um processo automatizado que garanta que o ambiente implantado corresponda ao estado especificado em um repositório." - [WeaveWorks](https://www.weave.works/technologies/gitops/#what-is-gitops)

## Por que devo usar o GitOps?

O GitOps simplesmente permite implantações mais rápidas, tendo repositórios Git no centro, oferecendo um rastreamento claro por meio de commits do Git e sem acesso direto ao ambiente. Saiba mais em [Por que devo usar o GitOps?](https://www.gitops.tech/#why-should-i-use-gitops)

O diagrama abaixo compara o fluxo de trabalho tradicional de CI/CD com o GitOps:
![implantações baseadas em push versus implantações baseadas em pull](images/GitopsWorflowVsTraditionalPush.jpg)

## Ferramentas para GitOps

Alguns frameworks populares de GitOps para Kubernetes apoiados pela comunidade [CNCF](https://landscape.cncf.io/card-mode?category=continuous-integration-delivery):

- [Flux V2](https://fluxcd.io/docs/get-started/)
- [Argo CD](https://argo-cd.readthedocs.io/en/stable/)
- [Rancher Fleet](https://fleet.rancher.io/)

## Implantação usando GitOps

O GitOps com o Flux v2 pode ser habilitado em clusters gerenciados do Azure Kubernetes Service (AKS) ou em clusters conectados do Kubernetes habilitados para o Azure Arc como uma extensão de cluster. Após a instalação da extensão de cluster microsoft.flux, você pode criar um ou mais recursos de fluxConfigurations que sincronizam as fontes do seu repositório Git com o cluster e reconciliam o cluster com o estado desejado. Com o GitOps, você pode usar seu repositório Git como a fonte da verdade para a configuração do cluster e a implantação de aplicativos.

- [Tutorial: Implante configurações usando GitOps em um cluster Kubernetes habilitado para Azure Arc](https://learn.microsoft.com/en-us/azure/azure-arc/kubernetes/tutorial-use-gitops-connected-cluster)
- [Tutorial: Implemente CI/CD com GitOps](https://learn.microsoft.com/en-us/azure/azure-arc/kubernetes/tutorial-gitops-flux2-ci-cd)
- [Ambiente com vários clusters e locatários com o Flux v2](https://github.com/microsoft/multicluster-gitops)
