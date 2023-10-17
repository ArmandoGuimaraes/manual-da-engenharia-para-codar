# Estudos de Análise

Os estudos de análise são uma ferramenta para selecionar a melhor opção entre várias opções possíveis para um determinado problema (por exemplo: computação, armazenamento). Eles avaliam escolhas potenciais em relação a um conjunto de critérios/requisitos objetivos para claramente apresentar os benefícios e limitações de cada solução.

[Estudos de análise](https://en.wikipedia.org/wiki/Trade_study) são um conceito da engenharia de sistemas que adaptamos para projetos de software. Os estudos de análise provaram ser uma ferramenta crítica para direcionar o alinhamento com as partes interessadas, ganhar credibilidade ao fazê-lo e garantir que nossas decisões fossem respaldadas por dados e não por viés.

## Quando Usar Esta Ferramenta

Os estudos de análise andam de mãos dadas com o projeto de arquitetura de alto nível. Isso geralmente ocorre à medida que os requisitos do projeto estão se solidificando, antes do início da codificação. Os estudos de análise continuam a ser úteis ao longo do projeto sempre que houver várias opções a serem selecionadas. Novos pontos de decisão podem surgir devido a mudanças nos requisitos, obtenção dos resultados de uma investigação técnica ou identificação de desafios que não foram originalmente previstos.

Os estudos de análise devem ser evitados se houver uma escolha clara de solução. Porque eles exigem que cada solução seja totalmente pensada, eles têm o potencial de levar muito tempo para serem concluídos. Quando houver um design claro, o estudo de análise deve ser omitido, e uma entrada deve ser feita no [Registro de Decisões](../decision-log/README.md) documentando a decisão.

## Por Que Usar Estudos de Análise

Os estudos de análise são uma maneira de formalizar o processo de design e deixar um registro documentado do motivo pelo qual a decisão foi tomada. Isso oferece algumas vantagens:

1. O modelo de estudo de análise orienta o usuário por meio do processo de design. Isso fornece estrutura à fase de design.

2. Ter um processo de design uniforme ajuda a dividir o trabalho entre os membros da equipe. Tivemos sucesso com engenheiros se reunindo para definir requisitos, critérios de avaliação e brainstorming de soluções possíveis. Em seguida, eles podem se separar para revisar soluções em paralelo, antes de se reunirem para tomar a decisão final.

3. O estudo de análise concluído ajuda a alinhar a equipe e os tomadores de decisão. Para apresentar os resultados do estudo, o próprio documento pode ser usado para destacar os principais pontos. Alternativamente, extraímos requisitos, diagramas para cada solução e a tabela de resultados para um slide deck que fornece uma visão geral de alto nível dos resultados.

4. O estudo de análise concluído é verificado no repositório de código, fornecendo documentação do processo de decisão. Isso deixa um histórico dos requisitos no momento que levou a cada decisão. Além disso, a tabela de resultados oferece uma referência rápida para como a decisão seria afetada se os requisitos mudarem à medida que o projeto avança.

## Fluxo de um Estudo de Análise

Os estudos de análise podem variar amplamente em escopo; no entanto, eles seguem o padrão comum descrito abaixo:

1. Solidifique os requisitos - Trabalhe com as partes interessadas para concordar com os requisitos para a funcionalidade que você está tentando construir.

2. Crie critérios de avaliação - Este é um conjunto de pontos de avaliação qualitativa e quantitativa que representam os requisitos. Juntos, eles se tornam um substituto fácil de medir para os requisitos potencialmente abstratos.

3. Brainstorm de soluções - Reúna uma lista de possíveis soluções para o problema. Em seguida, use seu melhor julgamento para escolher as 2-4 soluções que parecem mais promissoras. Para ajudar a reduzir as soluções, lembre-se de entrar em contato com especialistas em assuntos e outras equipes que possam ter passado por uma decisão semelhante.

4. Avalie soluções selecionadas - Aprofunde-se em cada solução e meça-a em relação aos critérios de avaliação. Nesta etapa, limite seu tempo de pesquisa para evitar investir demais em uma determinada área.

5. Compare resultados e escolha a solução - Alinhe a decisão com a equipe. Se você não conseguir decidir, uma lista clara de ações e responsáveis para conduzir a decisão final deve ser produzida.

## Modelo

Consulte [template.md](./template.md) para um exemplo de como estruturar as informações acima. Este modelo foi criado para orientar o usuário na condução de um estudo de análise. Após a tomada de decisão, recomendamos adicionar uma entrada no [Registro de Decisões](../decision-log/README.md) que faça referência ao texto completo do estudo de análise.
