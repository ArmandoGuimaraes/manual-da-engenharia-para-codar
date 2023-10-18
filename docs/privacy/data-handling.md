# Privacidade e Dados

## Objetivo

O objetivo desta seção é descrever brevemente as melhores práticas nos fundamentos de privacidade para projetos de dados ou partes de um projeto que possam conter dados.

**O que não é**: Este documento não é uma lista de verificação para como os clientes ou leitores devem lidar com dados em seu ambiente e não substitui as políticas da Microsoft ou dos clientes para o tratamento de dados, proteção de dados e segurança da informação.

## Introdução

A Microsoft baseia-se na confiança. Nossos clientes confiam na ISE para aderir aos mais altos padrões ao lidar com seus dados. Proteger os dados de nossos clientes é uma responsabilidade conjunta entre a Microsoft e os clientes; ambos têm a responsabilidade de ajudar os projetos a seguir as diretrizes descritas nesta página.

Os desenvolvedores que trabalham em projetos da ISE devem implementar as melhores práticas e orientações sobre o manuseio de dados ao longo das fases do projeto. Esta página não tem a intenção de sugerir como os clientes devem lidar com dados em seu ambiente. **Ela não substitui**:

- [Política de Segurança da Informação da Microsoft](https://aka.ms/CTRMSsecppext)
- [Adendo de Proteção de Dados Limitados](https://aka.ms/mpsldpa)
- [Adendo de Proteção de Dados de Serviços Profissionais](https://www.microsoft.com/licensing/docs/view/Microsoft-Products-and-Services-Data-Protection-Addendum-DPA)

## Os 5 W's do tratamento de dados

Ao trabalhar em um projeto, é importante abordar os seguintes 5 **W**:

- **Quem** - tem acesso aos dados e com quem compartilharemos os dados e/ou modelos desenvolvidos com esses dados?

- **O que** - dados são compartilhados conosco e com quais expectativas e entendimento.
Os clientes precisam ser explícitos sobre como os dados que compartilham se aplicam ao esforço geral.
O entendimento não deve ser vago, e não devemos ter acesso a um amplo conjunto de dados se isso não for necessário.

- **Onde** - os dados serão armazenados e sob que jurisdição legal esses dados estarão sujeitos.
Isso é especialmente importante em países como a Alemanha, onde diferentes leis de privacidade se aplicam, mas também é importante quando se trata de responder a intimações legais para os dados.

- **Quando** - o acesso aos dados será fornecido e por quanto tempo?
É importante não deixar o acesso aos dados indefinidamente após o término do projeto e definir antecipadamente as políticas de retenção de dados.

- **Por que** - você deu acesso aos dados?
Isso é particularmente importante para esclarecer o propósito e quaisquer restrições de uso além do propósito pretendido.

Por favor, use as diretrizes acima para garantir que os dados sejam usados apenas para os fins pretendidos e, assim, conquistar a confiança. É importante estar ciente das melhores práticas de tratamento de dados e garantir a clareza necessária para aderir aos 5 W's acima.

## Tratamento de dados em engajamentos da ISE

Os dados nunca devem sair dos ambientes controlados pelo cliente, e contratados ou outros membros no engajamento nunca devem ter acesso a conjuntos completos de dados do cliente, mas devem usar conjuntos de dados limitados do cliente usando as abordagens priorizadas a seguir:

1. Contratados ou parceiros de engajamento não trabalham diretamente com dados de produção, os dados serão copiados antes do processamento de acordo com as diretrizes abaixo.
1. Sempre aplique os princípios de [minimização de dados](https://www.forbes.com/sites/bernardmarr/2016/03/16/why-data-minimization-is-an-important-concept-in-the-age-of-big-data/#3fb711e91da4) para minimizar o alcance de erros, trabalhe apenas com o conjunto de dados mínimo necessário para alcançar os objetivos.
1. Gere dados sintéticos para dar suporte ao trabalho do engajamento. Se dados sintéticos não forem possíveis para alcançar os objetivos do projeto, solicite dados anonimizados nos quais a probabilidade de identificar indivíduos únicos seja mínima.
1. Selecione um conjunto de dados limitado e adequado, mais uma vez, siga os Princípios de Minimização de Dados e tente trabalhar com o menor número possível de linhas para alcançar os objetivos.

Antes de começar a trabalhar com dados, certifique-se de que os patches do sistema operacional estejam atualizados e as permissões estejam corretamente definidas sem acesso aberto à internet.

Os desenvolvedores que trabalham em projetos da ISE trabalharão com nossos clientes para definir os dados necessários para cada engajamento.

Se houver necessidade de acessar dados de produção,
a ISE precisa revisar a necessidade com seu líder e trabalhar com o cliente para colocar auditorias em vigor para verificar quais dados foram acessados.

Os dados de produção só devem ser compartilhados com membros aprovados da equipe de engajamento e não devem ser processados/transmitidos fora do ambiente controlado pelo cliente.

Os clientes devem fornecer à ISE uma cópia dos dados solicitados em um local gerenciado pelo cliente.
O cliente deve considerar ativar qualquer capacidade de registro para que possam identificar claramente quem tem acesso e o que é feito com esse acesso.
A ISE deve notificar o cliente quando terminar com os dados e sugerir que o cliente destrua as cópias dos dados se não forem mais necessárias.

### Nossos princípios orientadores ao lidar com dados em um engajamento

- Nunca acesse diretamente dados de produção.
- Declare explicitamente o propósito pretendido dos dados que podem ser usados no engajamento.
- Compartilhe apenas cópias dos dados de produção com os membros aprovados da equipe de engajamento.
- Toda a equipe deve trabalhar em conjunto para garantir que não haja cópias inativas de dados. Quando os dados não forem mais necessários,
a equipe deve trabalhar prontamente para limpar as cópias de dados do engajamento.
- Não envie cópias dos dados de produção fora do ambiente controlado pelo cliente.
- Use apenas o subconjunto mínimo de dados necessário para o propósito do engajamento.

### Perguntas a considerar ao trabalhar com dados

- Que dados precisamos?
- Qual é a base legal para o processamento desses dados?
- Se somos o processador com base na obrigação contratual, qual é nossa responsabilidade listada no contrato?
- O contrato precisa ser alterado?
- Como podemos conter a proliferação de dados?
- Quais controles de segurança estão em vigor para proteger esses dados?
- Qual é o protocolo de violação de dados?
- Como esses dados beneficiam o titular dos dados?
- Qual é a vida útil desses dados?
- Precisamos manter esses dados vinculados a um titular de dados?
- Podemos transformar esses dados em dados Não em Posição de Identificar (NPI) para uso posterior?
- Como o sistema está arquitetado para que os direitos do titular dos dados possam ser cumpridos? (por exemplo, manualmente, automaticamente)
- Se dados pessoais estiverem envolvidos, envolveu as equipes de privacidade e jurídica para este projeto?

## Resumo

É importante apenas trazer dados que são necessários para o problema em questão, quando isso é colocado em prática, descobrimos que só mantemos dados adequados, relevantes e limitados ao que é necessário em relação aos fins para os quais são processados. Isso é particularmente importante para dados pessoais. Uma vez que você tenha dados pessoais, muitas regras e regulamentos se aplicam, alguns exemplos desses podem ser HIPAA, GDPR, CCPA. O cliente deve estar ciente e destacar quais regulamentos se aplicam aos seus dados. Além disso, os [sete princípios do design de privacidade](https://privacy.ucsc.edu/resources/privacy-by-design---foundational-principles.pdf) devem ser revisados e considerados ao lidar com qualquer tipo de dado sensível.

## Recursos

- [Centro de Confiança da Microsoft](https://www.microsoft.com/en-us/trust-center/privacy)
- [Ferramentas para IA responsável - Proteção](https://www.microsoft.com/en-us/ai/responsible-ai-resources?activetab=pivot1:primaryr5)
- [Recursos de Proteção de Dados](https://servicetrust.microsoft.com/ViewPage/TrustDocuments?command=Download&docTab=6d000410-c9e9-11e7-9a91-892aae8839ad_AuditedControls)
- [FAQ e White Papers](https://servicetrust.microsoft.com/ViewPage/TrustDocuments?command=Download&docTab=6d000410-c9e9-11e7-9a91-892aae8839ad_AuditedControls)
- [Ofertas de Conformidade da Microsoft](https://learn.microsoft.com/en-us/compliance/regulatory/offering-home?view=o365-worldwide)
- [Listas de Verificação de Prontidão para Responsabilidade](https://learn.microsoft.com/en-us/compliance/regulatory/gdpr-arc?view=o365-worldwide#gdpr-compliance-controls)
- [Privacidade por Design: Os 7 Princípios Fundamentais](https://privacy.ucsc.edu/resources/privacy-by-design---foundational-principles.pdf)
