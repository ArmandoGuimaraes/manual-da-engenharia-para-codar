# Verificação de Credenciais

A verificação de credenciais é a prática de inspecionar automaticamente um projeto para garantir que não haja segredos incluídos no código-fonte do projeto. Segredos incluem senhas de banco de dados, strings de conexão de armazenamento, logins de administrador, princípios de serviço, etc.

## Por que a Verificação de Credenciais

Incluir segredos no código-fonte de um projeto é um risco significativo, pois pode tornar esses segredos disponíveis para partes indesejadas. Mesmo que pareça que o código-fonte seja acessível às mesmas pessoas que têm acesso aos segredos, essa situação provavelmente mudará à medida que o projeto crescer. Espalhar segredos em diferentes lugares torna mais difícil gerenciá-los, controlar o acesso e revogá-los de forma eficiente. Segredos que são confirmados no controle de origem também são mais difíceis de descartar, pois persistirão no histórico do código-fonte.  
Outra consideração é que acoplar o código do projeto à sua infraestrutura e detalhes de implantação é limitante e considerado uma má prática. Do ponto de vista do design de software, o código deve ser independente da configuração de tempo de execução que será usada para executá-lo, e essa configuração de tempo de execução inclui segredos.
Portanto, deve haver um limite claro entre o código e os segredos: os segredos devem ser gerenciados fora do código-fonte (leia mais [aqui](../../../continuous-delivery/secrets-management/README.md)) e a verificação de credenciais deve ser empregada para garantir que esse limite nunca seja violado.

## Aplicando a Verificação de Credenciais

Idealmente, a verificação de credenciais deve ser executada como parte do fluxo de trabalho de um desenvolvedor (por exemplo, por meio de um [hook git pré-commit](https://pre-commit.com/)), no entanto, para proteger contra erros de desenvolvedor, a verificação de credenciais também deve ser aplicada como parte do processo de integração contínua para garantir que nunca haja comprometimento de credenciais em um ramo principal de um projeto.
Para implementar a verificação de credenciais em um projeto, considere o seguinte:

1. Armazene segredos em um repositório externo seguro destinado a armazenar informações sensíveis.
1. Use ferramentas de verificação de segredos para avaliar o estado atual de seus repositórios, escaneando todo o histórico em busca de segredos.
1. Incorpore uma ferramenta automatizada de verificação de segredos em seu pipeline de CI para detectar a confirmação não intencional de segredos.
1. Evite comandos `git add .` no git.
1. Adicione arquivos sensíveis ao arquivo .gitignore.

## Estruturas e Ferramentas de Verificação de Credenciais

Receitas e Cenários -

1. [detect-secrets](./recipes/detect-secrets.md) é um módulo apropriadamente nomeado para detectar segredos dentro de uma base de código.
1. Use [detect-secrets dentro do Azure DevOps Pipeline](./recipes/detect-secrets-ado.md).
1. [Extensão de Análise de Código de Segurança da Microsoft](https://learn.microsoft.com/en-us/azure/security/develop/security-code-analysis-overview).

Ferramentas Adicionais -

1. [CodeQL](https://securitylab.github.com/tools/codeql) - Segurança do GitHub. O CodeQL permite consultar o código como se fosse dados. Escreva uma consulta para encontrar todas as variantes de uma vulnerabilidade.
1. [Git-secrets](https://github.com/awslabs/git-secrets) - Impede que você confirme senhas e outras informações confidenciais em um repositório git.

## Conclusão

A gestão de segredos é essencial para todos os projetos. Armazenar segredos em um repositório externo de segredos e incorporar essa mentalidade em seu fluxo de trabalho melhorará sua postura de segurança e resultará em código mais limpo.
