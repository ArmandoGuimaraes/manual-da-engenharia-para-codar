# Práticas Recomendadas

1. **ID de Correlação**: Inclua um identificador único no início da interação para vincular dados agregados de vários componentes do sistema e fornecer uma visão holística. Leia mais orientações sobre o uso do [ID de correlação](correlation-id.md).
2. Certifique-se de que a **saúde dos serviços** seja **monitorada** e forneça insights sobre o desempenho e o comportamento do sistema.
3. Certifique-se de que os **serviços dependentes** sejam monitorados adequadamente. Erros e exceções em serviços dependentes, como o cache Redis, o Service bus, etc., devem ser registrados e alertados. Além disso, métricas relacionadas aos serviços dependentes devem ser capturadas e registradas.

   - Além disso, falhas nos **serviços dependentes** devem ser propagadas por cada nível da pilha por meio da verificação de integridade.

4. **Falhas, crashes e falhas** são registrados como eventos discretos. Isso ajuda os engenheiros a identificar áreas problemáticas durante as falhas.
5. Certifique-se de que a configuração de registro (por exemplo, definir o registro como "verbose") possa ser controlada sem alterações de código.
6. Garanta que **métricas** relacionadas à latência e duração sejam coletadas e possam ser agregadas.
7. Comece pequeno e adicione métricas onde houver impacto no cliente. Evitar a [fadiga de métricas](pitfalls.md#metric-fatigue) é fundamental para coletar dados acionáveis.
8. É importante que todos os dados coletados contenham contexto relevante e rico.
9. Informações Pessoalmente Identificáveis ou qualquer outra informação sensível do cliente nunca devem ser registradas. Deve-se prestar atenção especial a regulamentações locais de privacidade de dados e os dados coletados devem estar em conformidade com essas regulamentações (por exemplo, GDPR).
10. **Verificação de Saúde**: Verificações de saúde apropriadas devem ser adicionadas para determinar se o serviço está saudável e pronto para atender ao tráfego. Em uma plataforma Kubernetes, diferentes tipos de sondagens, como Liveness, Readiness, Startup, etc., podem ser usados para determinar a saúde e prontidão do serviço implantado.

Leia mais [aqui](pitfalls.md) para entender o que observar ao projetar e construir um sistema observável.
