# Captura de Requisitos Não Funcionais

## Objetivos

Em projetos de engenharia de software, características importantes do sistema, como testabilidade, confiabilidade, escalabilidade, observabilidade, segurança, gerenciabilidade, entre outras, devem ser consideradas como requisitos não funcionais de primeira classe no processo de levantamento de requisitos. Ao definir esses requisitos não funcionais em detalhes no início do projeto, eles podem ser adequadamente avaliados quando o custo de seu impacto nas decisões de design subsequentes é comparativamente baixo.

Para apoiar o processo de captura de _requisitos não funcionais completos_ de um projeto, este documento oferece uma taxonomia para requisitos não funcionais e fornece um framework para sua identificação, exploração, atribuição aos stakeholders do cliente e, finalmente, codificação em requisitos de engenharia formais como entrada para o subsequente design da solução.

---

## Áreas de Investigação

### [Segurança Empresarial](../../security/README.md)

- Privacidade
  - PII (Informações Pessoalmente Identificáveis)
  - HIPAA
- Criptografia
- Mobilidade de Dados
  - em repouso
  - em trânsito
  - em processo/memória
- Gerenciamento de Chaves
  - responsabilidade
    - plataforma
    - BYOK (Traga sua própria chave)
    - CMK (Chave de Gerenciamento de Cliente)
- Regulamentações/normas de INFOSEC (Segurança da Informação)
  - por exemplo, FIPS-140-2
    - Nível 2
    - Nível 3
  - Série ISO 27000
  - NIST (Instituto Nacional de Padrões e Tecnologia)
  - Outros
- Segurança de Rede
  - Limites/fluxo de tráfego físico/lógico/topologia
    - Azure <-- --> On-premises (Local)
    - Público <-- --> Azure
    - VNET
    - PIP (Endereço IP Público)
    - Firewalls
    - VPN (Rede Virtual Privada)
    - ExpressRoute
      - Topologia
      - Segurança
  - Certificados
    - Emissor
      - CA (Autoridade de Certificação)
      - Autoassinado
      - Rotação/expiração
- Resposta a Incidentes de INFOSEC
  - Processo
  - Pessoas
  - Responsabilidades
  - Sistemas
  - Jurídico/regulatório/conformidade

### Autenticação e Autorização Empresariais

- Usuários
- Serviços
- Autoridades/diretórios
- Mecanismos/apertos de mãos (handshakes)
  - Active Directory
  - SAML (Linguagem de Marcação de Segurança)
  - OAuth
  - Outros
- Controle de Acesso Baseado em Funções (RBAC - Role-Based Access Control)
  - Modelo de herança de permissões

### [Monitoramento/Operações Empresariais](../../observability/README.md)

- Registro (Logging)
  - Operações
  - Relatórios
  - Auditoria
- Monitoramento
  - Diagnóstico/Alertas
  - Operações
- Alta disponibilidade/Recuperação de Desastres
  - Redundância
  - Recuperação/Mitigação
- Práticas
  - Princípio do mínimo privilégio
  - Princípio da separação de responsabilidades

### Outras tecnologias/práticas empresariais padrão

- Ecossistema de Desenvolvedores
  - Plataforma/Sistema Operacional
    - Reforçado
    - Imagens base aprovadas
    - Repositório de imagens
  - Ferramentas, linguagens
    - Processo de aprovação
  - Repositórios de código
    - Padrões de gerenciamento de segredos
      - Variáveis de ambiente
      - Arquivo(s) de configuração
      - API de recuperação de segredos
  - Origens de gerenciadores de pacotes
    - Privadas
    - Públicas
    - Aprovadas/Confiáveis
  - CI/CD (Integração Contínua/Entrega Contínua)
  - Repositórios de artefatos

### Ecossistema de Produção

- Plataforma/Sistema Operacional
  - Reforçado
  - Imagens base aprovadas
  - Repositório de imagens
- Longevidade/volatilidade da implantação
  - Automação
  - Reprodutibilidade
    - IaC (Infraestrutura como Código)
    - Scripting
    - Outros

### Outras áreas/tópicos não abordados acima (requer entrada do cliente para enumerar de forma abrangente)

---

## Processo de Investigação

1. Identifique/brainstorm áreas/tópicos prováveis que requerem investigação/definição adicional.
1. Identifique os stakeholders do cliente responsáveis por cada área/tópico identificado.
1. Ag

ende sessões de definição de requisitos/debrief com cada stakeholder conforme necessário para alcançar uma compreensão suficiente do impacto provável de cada requisito no projeto, tanto no marco atual/inicial quanto no roadmap de longo prazo.
1. Documente os requisitos/dependências identificados e as restrições de design relacionadas.
1. Avalie os marcos atuais/near-term planejados através da lente dos requisitos/constrangimentos identificados.
   - Categorize cada requisito como afetando os marcos imediatos/near-termos ou como aplicável apenas ao roadmap de longo prazo/subsequentes marcos.
1. Adapte os planos para os marcos atuais/near-termos para acomodar os requisitos categorizados como imediatos/near-termos.

---

## Estrutura do Esboço/Atribuição do Stakeholder Responsável

No esboço a seguir, atribua o nome/email do 'stakeholder responsável' para cada elemento após o nível apropriado na hierarquia do esboço. Assuma o modelo de atribuição de responsabilidade por herança: o stakeholder em qualquer nível ancestral (pai) também é responsável pelos elementos descendentes (filhos), a menos que seja substituído no nível descendente.

Exemplo:

- Pai1 _[Susan/susan@dominio.com]_
  - filho1
  - filho2 _[John/john@dominio.com]_
    - neto1
  - filho3
- Pai2 _[Sam/sam@dominio.com]_
  - filho1
  - filho2

No exemplo anterior, 'Susan' é responsável por `Pai1` e todos os seus descendentes, _exceto_ por `Pai1/filho2` e `Pai1/filho2/neto1` (para os quais 'John' é o stakeholder). 'Sam' é responsável por `Pai2` e todos os seus descendentes.

Essa abordagem permite a retenção da hierarquia lógica dos elementos em si, ao mesmo tempo em que intercala de forma flexível as identificações de 'stakeholder' dentro da hierarquia de tópicos, se e quando elas precisarem divergir devido a nuances organizacionais do cliente.
