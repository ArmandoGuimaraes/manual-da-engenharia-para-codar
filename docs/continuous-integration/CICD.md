# Integração Contínua e Entrega Contínua

A Integração Contínua é a prática de engenharia de frequentemente enviar código para um repositório compartilhado, idealmente várias vezes ao dia, e realizar uma compilação automatizada desse código. Essas mudanças são compiladas junto com outras alterações simultâneas no sistema, o que permite a detecção precoce de problemas de integração entre vários desenvolvedores que trabalham em um projeto. Quebras na compilação devido a falhas de integração são tratadas como a questão de maior prioridade para todos os desenvolvedores de uma equipe, e geralmente o trabalho para até que sejam corrigidas.

Associada a uma abordagem de teste automatizado, a integração contínua também nos permite testar a compilação integrada de forma a verificar não apenas se a base de código ainda é compilada corretamente, mas também se ainda é funcionalmente correta. Isso também é uma prática recomendada para a construção de sistemas de software robustos e flexíveis.

A Entrega Contínua leva o conceito de Integração Contínua ainda mais longe, testando também implantações da base de código integrada em uma réplica do ambiente em que será finalmente implantada. Isso nos permite aprender antecipadamente sobre quaisquer problemas operacionais não previstos que surgem de nossas alterações o mais rápido possível e também aprender sobre lacunas em nossa cobertura de teste.

O objetivo de tudo isso é garantir que o branch principal (main) esteja sempre pronto para ser entregue, ou seja, significa que poderíamos, se necessário, pegar uma compilação do branch principal de nossa base de código e implantá-la na produção.

Se esses conceitos são desconhecidos para você, reserve alguns minutos para ler [Integração Contínua](https://www.martinfowler.com/articles/continuousIntegration.html) e [Entrega Contínua](https://martinfowler.com/bliki/ContinuousDelivery.html).

Nossa expectativa é que a IC/CD deve ser usada em todos os projetos de engenharia que fazemos com nossos clientes e que estamos construindo, testando e implantando cada alteração que fazemos em qualquer sistema de software que estamos desenvolvendo.

Para uma compreensão mais profunda de todos esses conceitos, os livros [Integração Contínua](https://www.amazon.com/Continuous-Integration-Improving-Software-Reducing/dp/0321336380) e [Entrega Contínua](https://www.amazon.com/gp/product/0321601912) fornecem um conhecimento abrangente.

## Ferramentas

### Azure Pipelines

Nossa ferramenta na Microsoft tornou fácil configurar sistemas de integração e entrega como este. Se você não está familiarizado com ela, reserve alguns momentos agora para ler sobre [Azure Pipelines](https://azure.microsoft.com/en-us/services/devops/pipelines/) (anteriormente VSTS). Para um exemplo prático de como isso funciona na prática, você pode ler [IC/CD no Kubernetes com VSTS](https://medium.com/@timfpark/application-ci-cd-on-kubernetes-with-visual-studio-team-services-ccacecdea8a5).

### Jenkins

O Jenkins é uma das ferramentas mais comumente usadas na comunidade de código aberto. É bem conhecido e possui centenas de plugins para atender a todos os requisitos de compilação.
O Jenkins é gratuito, mas requer um servidor dedicado.
Você pode criar facilmente uma máquina virtual Jenkins usando este [modelo](https://ms.portal.azure.com/#create/azure-oss.jenkinsjenkins).

### Travis CI

O Travis CI pode ser usado gratuitamente em projetos de código aberto, mas os desenvolvedores devem adquirir um plano empresarial para projetos privados.
Este serviço é ideal para validação de PRs no GitHub, pois é leve e fácil de configurar, sem a necessidade de configuração de servidor dedicado.
Ele também suporta uma característica de matriz de compilação que permite acelerar o processo de compilação e teste dividindo-o em partes.

### CircleCI

O CircleCI é um serviço gratuito para projetos de código aberto, sem a necessidade de servidor dedicado. Também é ideal para validação de PRs no GitHub.
O CircleCI também permite fluxos de trabalho, paralelismo e a divisão de seus testes em qualquer número de contêineres com uma ampla variedade de pacotes pré-instalados nos contêineres de compilação.

### AppVeyor

O AppVeyor é outro serviço de IC gratuito para projetos de código aberto que também suporta compilações baseadas no Windows.
