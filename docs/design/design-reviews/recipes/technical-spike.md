# Investigação Técnica

De acordo com a [Wikipedia](https://en.wikipedia.org/wiki/Spike_(software_development))...

Uma investigação técnica em uma sprint pode ser utilizada de várias maneiras:

- Como uma forma de familiarizar a equipe com novo hardware ou software.
- Para analisar minuciosamente um problema e auxiliar na divisão adequada do trabalho entre membros separados da equipe.
- Testes de investigação podem também ser usados para mitigar riscos futuros e podem revelar problemas adicionais que passaram despercebidos.

É possível fazer uma distinção entre investigações técnicas e investigações funcionais. A investigação técnica é utilizada com maior frequência para avaliar o impacto que uma nova tecnologia tem na implementação atual. Uma investigação funcional é usada para determinar a interação com um novo recurso ou implementação.

Também podem ser realizadas [investigações de viabilidade técnica](./engineering-feasibility-spikes.md) para reduzir os riscos em um engajamento e aumentar a compreensão da equipe.

## Resultado

Geralmente, o resultado de uma Investigação Técnica deve ser um documento detalhando o que foi avaliado e o resultado dessa avaliação. Os detalhes contidos no documento podem variar, mas existem alguns princípios gerais que podem ser úteis.

- **Declaração do Problema/Objetivos:** Certifique-se de incluir uma seção que detalhe claramente por que uma avaliação está sendo feita e qual deve ser o resultado dessa avaliação. Isso é útil para garantir que a investigação técnica tenha sido produtiva e avançado o projeto como um todo de alguma forma.

- **Garanta que seja Repetível:** Detalhe os componentes usados, instruções de instalação, configuração, etc., necessários para criar o ambiente que foi usado para avaliação e teste. Se algum teste for realizado, certifique-se de incluir os scripts, links para as aplicações, opções de configuração, etc., para que os testes possam ser realizados novamente.

    Existem muitas razões pelas quais o ambiente de avaliação pode precisar ser reconstruído. Por exemplo:

  - Outro cenário precisa ser testado.
  - Uma nova versão da tecnologia foi lançada.
  - A tecnologia precisa ser testada em uma nova plataforma.

- **Coleta de Dados:** O objetivo de uma investigação deve ser a coleta de dados, não a tomada de decisões ou recomendações. Idealmente, a investigação técnica aprofunda várias questões técnicas e obtém respostas para que a _equipe de projeto mais ampla_ possa então se reunir e concordar com um curso apropriado para o futuro.

- **Evidências:** Geralmente, você usará seções para resumir os resultados dos testes que não incluem os resultados detalhados potencialmente numerosos; no entanto, você deve incluir todos os resultados detalhados dos testes em um apêndice ou um anexo. Ter resultados completos detalhados em algum lugar ajudará a equipe a confiar nos resultados. Além disso, os dados podem ser interpretados de várias maneiras diferentes, e pode ser necessário voltar aos dados originais para uma nova interpretação.

- **Organização:** A documentação técnica pode ser extensa. Geralmente, é uma boa ideia organizar as seções com cabeçalhos e incluir uma tabela de conteúdo. Geralmente, as seções no início do documento devem resumir os dados e usar um ou mais apêndices para mais detalhes.
