# Verificação de Dependências e Contêineres

A verificação de dependências e contêineres é realizada para procurar vulnerabilidades nos sistemas operacionais, pacotes de linguagem e aplicativos.

## Por que a Verificação de Dependências e Contêineres

As imagens de contêiner são um formato padrão de entrega de aplicativos em ambientes nativos da nuvem. Tendo uma ampla seleção de imagens da comunidade, frequentemente escolhemos uma imagem base da comunidade e depois adicionamos pacotes de que precisamos, que também podem vir de fontes da comunidade. Essas dependências arbitrárias podem introduzir vulnerabilidades em nossa imagem e aplicação.

## Aplicação da Verificação de Dependências e Contêineres

As imagens que contêm software com vulnerabilidades de segurança se tornam exploráveis em tempo de execução. Ao criar uma imagem em seu pipeline de CI, a verificação de imagens deve ser um requisito para que a construção seja aprovada. Imagens que não passaram na verificação nunca devem ser enviadas para o registro de contêiner acessível em produção.

Melhores práticas de verificação de dependências e contêineres:

1. Imagem Base - se sua imagem for construída em cima de uma imagem base de terceiros, valide o seguinte:
   - A imagem é de uma empresa conhecida ou de um grupo de código aberto.
   - Ela está hospedada em um registro respeitável.
   - O Dockerfile está disponível e verifique as dependências instaladas nele.
   - A imagem é atualizada com frequência - imagens antigas podem não conter as últimas atualizações de segurança.
1. Remova Software Não Essencial - Comece com uma imagem base mínima e instale apenas as ferramentas, bibliotecas e arquivos de configuração necessários para seu aplicativo. Evite instalar as seguintes ferramentas ou remova-as se estiverem presentes:
   - Ferramentas e clientes de rede: por exemplo, wget, curl, netcat, ssh.
   - Shells: por exemplo, sh, bash. Observe que remover shells também impede o uso de scripts de shell em tempo de execução. Em vez disso, use um executável sempre que possível.
   - Compiladores e depuradores. Eles devem ser usados apenas em contêineres de build e desenvolvimento, mas nunca em contêineres de produção.
1. As imagens de contêiner devem ser imutáveis - faça o download e inclua todas as dependências necessárias durante a construção da imagem.
1. Verifique as vulnerabilidades nas dependências de software - hoje, provavelmente, não existe projeto de software sem alguma forma de bibliotecas externas, dependências ou código aberto. Embora permita que a equipe de desenvolvimento se concentre no código de seu aplicativo, a dependência traz um lado negativo esperado, em que a postura de segurança do aplicativo real agora depende dela. Para detectar vulnerabilidades contidas nas dependências de um projeto, use ferramentas de verificação de contêineres que, como parte de sua análise, examinem as dependências de software (consulte "Estruturas e Ferramentas de Verificação de Dependências e Contêineres").

## Estruturas e Ferramentas de Verificação de Dependências e Contêineres

1. [Trivy](https://github.com/aquasecurity/trivy) - uma ferramenta simples e abrangente de verificação de vulnerabilidades para contêineres (não suporta contêineres Windows).
1. [Aqua](https://www.aquasec.com/solutions/azure-container-security/) - verificação de dependências e contêineres para aplicativos em execução no AKS, ACI e contêineres Windows. Possui integração com os pipelines do Azure DevOps.
1. [Plugin Dependency-Check para SonarQube](https://github.com/dependency-check/dependency-check-sonar-plugin) - Verificação de dependência local
1. [Mend (anteriormente WhiteSource)](https://www.mend.io/) - Software de verificação de código aberto

## Conclusão

Tecnologias poderosas, como contêineres, devem ser usadas com cuidado. Instale apenas os requisitos mínimos necessários para seu aplicativo, esteja ciente das dependências de software que seu aplicativo está usando e certifique-se de mantê-las ao longo do tempo usando ferramentas de verificação de contêineres e dependências.
