# Segurança

Os desenvolvedores que trabalham em projetos devem aderir às práticas padrão recomendadas pela indústria para o design seguro e implementação de código. Para os nossos clientes, isso significa que nossos engenheiros devem entender os [10 Principais Riscos de Segurança de Aplicações Web da OWASP](https://owasp.org/www-project-top-ten/), bem como como mitigar o máximo possível deles, usando os recursos abaixo.

**Se você está procurando uma maneira rápida de começar** a avaliar sua aplicação ou design, confira o documento "Referência Rápida de Práticas de Codificação Segura" abaixo, que contém uma lista de verificação detalhada de conceitos de alto nível que você pode verificar se estão sendo feitos corretamente. Esta lista cobre muitos erros comuns associados à lista dos 10 Principais Riscos da OWASP mencionada acima e deve representar o esforço mínimo em termos de segurança.

## Solicitação de Revisões de Segurança

Ao solicitar uma revisão de segurança para sua aplicação, certifique-se de ter se familiarizado com as [Regras de Engajamento](rules-of-engagement.md). Isso o ajudará a preparar a aplicação para o teste, bem como entender os limites de escopo do teste.

## Referências Rápidas

- [Referência Rápida de Práticas de Codificação Segura](https://owasp.org/www-pdf-archive/OWASP_SCP_Quick_Reference_Guide_v2.pdf)
- [Referência Rápida de Segurança de Aplicações Web](https://owasp.org/www-pdf-archive//OWASP_Web_Application_Security_Quick_Reference_Guide_0.3.pdf)
- [Mentalidade de Segurança/Criando um Programa de Segurança Inicial Rápido](https://github.com/OWASP/Quick-Start-Guide/blob/master/OWASP%20Quick%20Start%20Guide.pdf?raw=true)
- [Varredura de Credenciais / Detecção de Segredos](../continuous-integration/dev-sec-ops/secret-management/credential_scanning.md)
- [Modelagem de Ameaças](./threat-modelling.md)

## Segurança no Azure DevOps

- [Práticas de Engenharia de Segurança DevSecOps](https://www.microsoft.com/en-us/securityengineering/devsecops)
- [Visão Geral da Proteção de Dados no Azure DevOps](https://learn.microsoft.com/en-us/azure/devops/organizations/security/data-protection?view=azure-devops)
- [Segurança e Identidade no Azure DevOps](https://learn.microsoft.com/en-us/azure/devops/organizations/security/about-security-identity?view=azure-devops)
- [Análise de Código de Segurança](https://secdevtools.azurewebsites.net/)

## DevSecOps

Introduza a segurança ao seu projeto desde as fases iniciais. A seção [DevSecOps](../continuous-integration/dev-sec-ops/README.md) aborda práticas de segurança, automação, ferramentas e estruturas como parte da integração contínua da aplicação.

## Folhas de Resumo da OWASP

> Observação: a OWASP é considerada o padrão-ouro em informações de segurança de computadores. A OWASP mantém uma extensa série de folhas de resumo que cobrem todos os 10 Principais Riscos da OWASP e mais. Abaixo, muitas das folhas de resumo mais relevantes foram resumidas. Para ver todas as folhas de resumo, confira o [Índice de Folhas de Resumo](https://github.com/OWASP/CheatSheetSeries/blob/master/Index.md).

- [Noções Básicas de Controle de Acesso](https://github.com/OWASP/CheatSheetSeries/blob/master/cheatsheets/Access_Control_Cheat_Sheet.md)
- [Análise de Superfície de Ataque](https://github.com/OWASP/CheatSheetSeries/blob/master/cheatsheets/Attack_Surface_Analysis_Cheat_Sheet.md)
- [Política de Segurança de Conteúdo (CSP)](https://github.com/OWASP/CheatSheetSeries/blob/master/cheatsheets/Content_Security_Policy_Cheat_Sheet.md)
- [Prevenção de Falsificação de Solicitação entre Sites (CSRF)](https://github.com/OWASP/CheatSheetSeries/blob/master/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.md)
- [Prevenção de Script entre Sites (XSS)](https://github.com/OWASP/CheatSheetSeries/blob/master/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.md)
- [Armazenamento Criptográfico](https://github.com/OWASP/CheatSheetSeries/blob/master/cheatsheets/Cryptographic_Storage_Cheat_Sheet.md)
- [Serialização](https://github.com/OWASP/CheatSheetSeries/blob/master/cheatsheets/Deserialization_Cheat_Sheet.md)
- [Segurança do Docker/Kubernetes (k8s)](https://github.com/OWASP/CheatSheetSeries/blob/master/cheatsheets/Docker_Security_Cheat_Sheet.md)
- [Validação de Entrada](https://github.com/OWASP/CheatSheetSeries/blob/master/cheatsheets/Input_Validation_Cheat_Sheet.md)
- [Gerenciamento de Chaves](https://github.com/OWASP/CheatSheetSeries/blob/master/cheatsheets/Key_Management_Cheat_Sheet.md)
- [Defesa contra Injeção de Comando no Sistema Operacional](https://github.com/OWASP/CheatSheetSeries/blob/master/cheatsheets/OS_Command_Injection_Defense_Cheat_Sheet.md)
- [Exemplos de Parametrização de Consulta](https://github.com/OWASP/CheatSheetSeries/blob/master/cheatsheets/Query_Parameterization_Cheat_Sheet.md)
- [Prevenção de Solicitação de Servidor do Lado do Cliente](https://github.com/OWASP/CheatSheetSeries/blob/master/cheatsheets/Server_Side_Request_Forgery_Prevention_Cheat_Sheet.md)
- [Prevenção de Injeção SQL](https://github.com/OWASP/CheatSheetSeries/blob/master/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.md)
- [Redirecionamentos e Encaminhamentos não Validados](https://github.com/OWASP/CheatSheetSeries/blob/master/cheatsheets/Unvalidated_Redirects_and_Forwards_Cheat_Sheet.md)
- [Segurança de Serviços Web](https://github.com/OWASP/CheatSheetSeries/blob/master/cheatsheets/Web_Service_Security_Cheat_Sheet.md)
- [Segurança XML](https://github.com/OWASP/CheatSheetSeries/blob/master/cheatsheets/XML_Security_Cheat_Sheet.md)

## Ferramentas Recomendadas

Confira a lista de ferramentas para ajudar a habilitar a segurança em seus projetos.

> **Observação:** Embora algumas ferramentas sejam agnósticas, a lista abaixo se concentra na segurança nativa em nuvem, com foco em Kubernetes.

- Verificação de Vulnerabilidades

  - [SonarCloud](https://sonarcloud.io/)
    - Integra-se ao Azure Devops com o clique de um botão.
  - [Snyk](https://github.com/snyk/snyk)
  - [Trivy](https://github.com/aquasecurity/trivy)
  - [Cloudsploit](https://github.com/aquasecurity/cloudsploit)
  - [Anchore](https://github.com/anchore/anchore-engine)
  - [Outras ferramentas da OWASP](https://owasp.org/www-community/Vulnerability_Scanning_Tools)
  - [Veja por que você deve verificar vulnerabilidades em todas as camadas da pilha](https://sysdig.com/blog/image-scanning-best-practices/), bem como algumas dicas úteis adicionais para reduzir a superfície de ataque.

- Segurança em Tempo de Execução

  - [Falco](https://github.com/falcosecurity/falco)
  - [Tracee](https://github.com/aquasecurity/tracee)
  - [Kubelinter](https://github.com/stackrox/kube-linter)
    - Pode não se qualificar totalmente como segurança em tempo de execução, mas ajuda a garantir que você esteja habilitando as melhores práticas.

- Autorização Binária

  A autorização binária pode ocorrer tanto na camada do registro Docker quanto em tempo de execução (ou seja, por meio de um controlador de admissão K8s).
  A verificação de autorização garante que a imagem seja assinada por uma autoridade confiável. Isso pode ocorrer tanto para imagens de terceiros (pré-aprovadas)
  quanto para imagens internas. Levando isso adiante, a assinatura deve ocorrer _apenas_ em imagens em que todo o código foi revisado e aprovado.
  A autorização binária pode reduzir o impacto de danos causados por um ambiente de hospedagem comprometido e o dano causado por insiders maliciosos.

  - [Harbor](https://github.com/goharbor/harbor/)
    - [Operador disponível](https://github.com/goharbor/harbor-operator)
  - [Portieris](https://github.com/IBM/portieris)
  - [Notary](https://github.com/theupdateframework/notary)
    - Observação: o Harbor utiliza internamente o Notary.
  - [TUF](https://github.com/theupdateframework/tuf)

- Outra Segurança do K8s

  - [OPA](https://github.com/open-policy-agent/opa), [Gatekeeper](https://github.com/open-policy-agent/gatekeeper) e [Biblioteca Gatekeeper](https://github.com/open-policy-agent/gatekeeper-library/tree/master/library)
  - [cert-manager](https://github.com/jetstack/cert-manager) para fornecimento automático e rotação de certificados.
  - [Habilite rapidamente o mTLS entre seus microserviços com o Linkerd](https://linkerd.io/2/features/automatic-mtls/).

## Links Úteis

- [Orientações sobre Requisitos Não Funcionais](../design/design-patterns/non-functional-requirements-capture-guide.md)
