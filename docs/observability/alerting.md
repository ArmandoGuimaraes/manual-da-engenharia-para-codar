# Orientações para Alertas

Um dos objetivos de construir sistemas altamente observáveis é fornecer uma visão valiosa do comportamento da aplicação. Sistemas observáveis permitem a identificação e a apresentação de problemas por meio de alertas antes que os usuários finais sejam afetados.

## Melhores Práticas

- A primeira coisa a fazer antes de criar alertas é implementar a observabilidade. Sem sistemas de monitoramento em vigor, torna-se praticamente impossível saber quais atividades precisam ser monitoradas e quando alertar as equipes.
- Identifique qual deve ser a qualidade mínima de serviço da aplicação. Não se trata do que você pretende oferecer, mas do que é aceitável para o cliente. Esses [Objetivos de Nível de Serviço](https://landing.google.com/sre/sre-book/chapters/service-level-objectives/)(SLOs) são uma métrica para medir o desempenho da aplicação.
- Os SLOs são definidos em relação aos usuários finais. Os alertas devem monitorar o impacto visível no usuário. Por exemplo, alertas sobre taxa de solicitações, latência e erros.
- Use ferramentas automatizadas e scriptáveis para imitar caminhos de código importantes de ponta a ponta relacionados às atividades na aplicação. Crie políticas de alerta para eventos que afetam os usuários ou mudanças na taxa de métricas.
- A fadiga de alerta é real. É recomendável que os engenheiros prestem atenção ao sistema de monitoramento para que alertas e limiares precisos possam ser definidos.
- Estabeleça um canal primário para alertas que precisam de atenção imediata e identifique a equipe/pessoa certa com base na natureza do incidente. Nem todos os alertas precisam ser enviados para o canal principal de plantão.
- Estabeleça um canal secundário para itens que precisam ser investigados, mas ainda não afetam os usuários. Por exemplo, armazenamento que está se aproximando do limite de capacidade. Esses itens serão o que os serviços de engenharia monitorarão regularmente para verificar a saúde do sistema.
- Certifique-se de configurar alertas adequados para falhas em serviços dependentes, como o cache Redis, o Service Bus, etc. Por exemplo, se o cache Redis estiver lançando 10 exceções nos últimos 60 segundos, é recomendável criar alertas apropriados para que essas falhas sejam identificadas e medidas sejam tomadas.
- É importante aprender com cada incidente e melhorar continuamente o processo. Após cada incidente ter sido triado, realize uma [pós-análise do cenário](https://landing.google.com/sre/workbook/chapters/postmortem-culture/). Cenários e situações que não foram considerados inicialmente ocorrerão, e o fluxo de pós-análise é uma ótima maneira de destacá-los para melhorar o monitoramento/alerta do sistema. Configurar um alerta para detectar aquele cenário de incidente é uma boa ideia para ver se o evento ocorre novamente.
