# Observabilidade de Pipelines de CI/CD

Com o aumento da complexidade das pipelines de entrega, é muito importante considerar a observabilidade no contexto da compilação e implantação de aplicativos.

## Benefícios

- Ter uma instrumentação adequada durante o tempo de compilação ajuda a obter insights sobre as várias etapas do processo de compilação e implantação.

- Ajuda os desenvolvedores a entender onde estão os gargalos de desempenho da pipeline, com base nos dados coletados. Isso ajuda a ter conversas baseadas em dados sobre a identificação de latência entre tarefas, problemas de desempenho, tempos de upload/download de artefatos, fornecendo informações valiosas sobre a disponibilidade e capacidade dos agentes.

- Ajuda a identificar tendências em falhas, permitindo que os desenvolvedores façam rapidamente uma análise da causa raiz.

- Ajuda a fornecer uma visão da saúde da pipeline em toda a organização, para identificar tendências facilmente.

## Pontos a Considerar

- É importante identificar os Indicadores-chave de Desempenho (KPIs) para avaliar uma pipeline de CI/CD bem-sucedida. Quando necessário, é possível adicionar rastreamento adicional para registrar melhor as métricas de KPI. Por exemplo, adicionar tags de compilação da pipeline para identificar um 'Candidato a Lançamento' vs. 'Não Candidato a Lançamento' ajuda a avaliar o cronograma do processo de lançamento de ponta a ponta.

- Dependendo das ferramentas usadas (Azure DevOps, Jenkins etc.), relatórios básicos sobre as pipelines estão disponíveis prontos para uso. É importante avaliar esses relatórios em relação aos KPIs para entender se é necessário uma solução de relatórios personalizados para suas pipelines. Se necessário, é possível criar painéis personalizados usando ferramentas de terceiros como Grafana ou Power BI Dashboards.
