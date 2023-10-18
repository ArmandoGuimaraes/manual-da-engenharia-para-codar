# Coisas a Observar ao Construir Sistemas Observáveis

## Observabilidade como Pensamento Tardio

Um dos objetivos de design ao construir um sistema deve ser possibilitar o monitoramento do sistema. Isso ajuda no planejamento e na consideração da disponibilidade da aplicação, do registro de eventos e das métricas no momento do design e desenvolvimento. A observabilidade também atua como uma ótima ferramenta de depuração, fornecendo aos desenvolvedores uma visão geral do sistema. Ao deixar a instrumentação e o registro de métricas para o final, as equipes de desenvolvimento perdem informações valiosas durante o desenvolvimento.

## Fadiga de Métricas

1. É recomendável coletar e medir *o que você precisa* **e** *não o que você pode*. Não tente monitorar tudo.
2. Se os dados não forem acionáveis, eles são inúteis e se tornam ruído. Por outro lado, às vezes é muito difícil prever todos os cenários possíveis que podem dar errado.
3. Deve haver um equilíbrio entre coletar o que é necessário e registrar todas as atividades no sistema. Uma regra geral é seguir esses princípios:

   - regras que identificam incidentes devem ser simples, relevantes e confiáveis
   - qualquer dado que seja coletado, mas não agregado ou não gerou alertas, deve ser revisado para verificar se ainda é necessário.

## Contexto

Todos os dados registrados devem conter um contexto rico, que seja útil para obter uma visão geral do sistema e fácil de rastrear erros/falhas durante a solução de problemas. Ao registrar dados, também é importante evitar a criação de silos de dados.

## Informações Pessoais Identificáveis

Como regra geral, não registre nenhuma informação sensível do cliente e informações pessoais identificáveis (PII). Certifique-se de seguir as regulamentações de privacidade pertinentes em relação à PII (Ex: GDPR etc.)
Leia mais [aqui](logs-privacy.md) sobre como manter dados sensíveis fora dos registros.
