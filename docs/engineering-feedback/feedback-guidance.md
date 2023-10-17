# Orientações para Feedback de Engenharia

As seguintes orientações fornecem um conjunto mínimo de detalhes que resultarão em feedback de engenharia acionável. Certifique-se de fornecer o máximo de detalhes possível para cada uma das seguintes seções, conforme relevante.

## Título

Forneça um título significativo e descritivo. Não é necessário incluir o serviço Azure no título, pois isso será incluído como parte da seção de **Categorização**.

Exemplos bons:

- Versões de X suportadas não documentadas
- Requer história Y completa

## Resumo

Resuma o feedback em um parágrafo curto.

## Categorização

### Serviço Azure

A que serviço Azure se refere este item de feedback? Se houver vários serviços Azure envolvidos, escolha o serviço principal e inclua os detalhes dos outros na seção **Notas**.

### Tipo

Selecione um dos seguintes para descrever que tipo de feedback está sendo fornecido:

- Impedimento de Negócios (por exemplo, Sem SLA em X, Serviço Y não está na GA, Serviço A não está na Região B)
- Impedimento Técnico (por exemplo, Rede Acelerada não disponível no Serviço X)
- Documentação (por exemplo, Instruções para configurar o cenário X ausentes)
- Solicitação de Recurso (por exemplo, Habilitar integração simples com X no Serviço Y)

### Estágio

Selecione um dos seguintes para descrever o estágio do ciclo de vida do engajamento que gerou este feedback:

- Produção
- Estágio
- Teste
- Desenvolvimento

### Impacto

Descreva o impacto para o cliente e o engajamento que esse feedback implica.

### Prazo

Forneça um prazo dentro do qual este item de feedback precisa ser resolvido (se relevante).

### Prioridade

Forneça a prioridade do ponto de vista do cliente para o feedback. O feedback é priorizado em um dos quatro níveis a seguir:

- **P0 - O impacto é crítico e grande**: Precisa ser abordado imediatamente; o impacto é crítico e de grande alcance (ou seja, grande interrupção do serviço; torna o serviço ou funções inutilizáveis/inacessíveis para uma grande parte do espaço abrangível; sem solução conhecida).
- **P1 - O impacto é alto e significativo**: Precisa ser abordado rapidamente; impacta uma grande porcentagem do espaço abrangível e dificulta o progresso. Uma solução alternativa parcial existe ou é muito problemática.
- **P2 - O impacto é moderado e varia em alcance**: Precisa ser abordado em um período de tempo razoável (ou seja, questões que estão prejudicando a adoção e o uso sem soluções alternativas razoáveis). Por exemplo, o feedback pode estar relacionado a problemas de nível de recurso para resolver a fricção.
- **P3 - O impacto é baixo**: O problema pode ser resolvido quando possível ou mais tarde (ou seja, relevante para o espaço abrangível central, mas o problema não impede o progresso ou possui solução razoável). Por exemplo, o feedback pode estar relacionado a ideias ou oportunidades de recursos.

## Passos de Reprodução

Os passos de reprodução são importantes, pois ajudam a confirmar e reproduzir o problema e são essenciais para demonstrar o sucesso quando houver uma solução.

### Pré-requisitos

Forneça um conjunto claro de todas as condições e pré-requisitos necessários antes de seguir os passos de reprodução. Isso pode incluir:

- Plataforma (por exemplo, cluster AKS 1.16.4 com Azure CNI, VM Ubuntu 19.04)
- Serviços (por exemplo, Azure Key Vault, Azure Monitor)
- Rede (por exemplo, VNET com sub-rede)

### Passos

Forneça um conjunto claro de passos repetíveis que permitirão a reprodução deste feedback. Isso pode assumir a forma de:

- Scripts (por exemplo, bash, PowerShell, terraform, modelo de braço)
- Instruções de linha de comando (por exemplo, az, helm, terraform)
- Capturas de tela (por exemplo, telas do portal Azure)

## Notas

Inclua itens como diagramas de arquitetura, capturas de tela, logs, traces, etc., que possam ajudar a entender suas anotações e o item de feedback. Inclua também detalhes sobre o cenário do cliente/parceiro, o máximo possível, no conteúdo principal.

### O que não funcionou

Descreva o que não funcionou ou que lacuna de recurso você identificou.

### Qual era a sua expectativa ou resultado desejado

Descreva o que você esperava que acontecesse. Qual era o resultado esperado? 

### Descreva os passos que você seguiu

Forneça uma descrição clara dos passos tomados e o resultado/descrição em cada ponto.
