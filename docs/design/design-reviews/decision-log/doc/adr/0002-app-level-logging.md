# 2. Registro de Log no Nível de Aplicativo com Serilog e Application Insights

Data: 8 de abril de 2020

## Status

Aceito

## Contexto

Devido ao design de microservices da plataforma, precisamos garantir a consistência do registro em cada serviço para que o rastreamento de uso, desempenho, erros, etc., possa ser realizado de ponta a ponta. Deve ser utilizado um único framework de registro/monitoramento sempre que possível para alcançar isso, permitindo ao mesmo tempo a flexibilidade para integração/exportação em outras ferramentas em um estágio posterior. Os desenvolvedores devem ter uma interface simples para registrar mensagens e métricas.

## Decisão

Concordamos em utilizar o Serilog como o framework de registro preferido do Dotnet no nível da aplicação, com integração ao Log Analytics e Application Insights para análise.

## Consequências

Será necessário configurar a amostragem no Application Insights para que ele não se torne excessivamente caro ao receber milhões de mensagens, mas também não impeça a captura de informações essenciais.
A equipe precisará registrar apenas o que foi acordado como essencial para monitoramento como parte das revisões de design, a fim de reduzir o ruído e os níveis desnecessários de amostragem.
