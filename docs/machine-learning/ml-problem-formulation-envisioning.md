# Visão e Formulação do Problema

Antes de iniciar uma investigação de ciência de dados, é necessário definir uma declaração de problema que a equipe de ciência de dados possa explorar; essa declaração de problema pode ter uma influência significativa na probabilidade de sucesso do projeto.

## Metas de Visão

As principais metas do processo de visão são:

* Estabelecer uma compreensão clara do domínio do problema e do objetivo de negócio subjacente
* Definir como uma solução potencial seria usada e como seu desempenho deve ser medido
* Determinar quais dados estão disponíveis para resolver o problema
* Compreender as capacidades e práticas de trabalho da equipe de ciência de dados
* Garantir que todas as partes tenham a mesma compreensão do escopo e dos próximos passos (por exemplo, integração, workshop de exploração de dados)

O processo de visão geralmente envolve uma série de sessões de 'visão' onde a equipe de ciência de dados trabalha junto com especialistas no assunto para formular o problema de tal maneira que haja uma compreensão compartilhada do domínio do problema, um objetivo claro e uma abordagem predefinida para avaliar uma solução potencial.

## Compreensão do Domínio do Problema

Geralmente, antes de definir o escopo de um projeto para uma investigação de ciência de dados, devemos primeiro compreender o domínio do problema:

* Qual é o problema?
* Por que o problema precisa ser resolvido?
* Este problema requer uma solução de aprendizado de máquina?
* Como uma solução potencial seria usada?

No entanto, estabelecer essa compreensão pode ser difícil, especialmente para aqueles que não estão familiarizados com o domínio do problema. Para facilitar esse processo, podemos abordar problemas de maneira estruturada, seguindo as etapas a seguir:

* Identifique um problema mensurável e defina-o em termos de negócios. O objetivo deve ser claro, e devemos ter uma boa compreensão dos fatores que podemos controlar - que podem ser usados como entradas - e como eles afetam o objetivo. Seja o mais específico possível.
* Decida como o desempenho de uma solução será medido e identifique se isso é possível dentro das restrições desse problema. Certifique-se de que esteja alinhado com o objetivo de negócios e que você identificou os dados necessários para avaliar a solução. Observe que os dados necessários para avaliar uma solução podem ser diferentes dos dados necessários para criar uma solução.
* Pensando na solução como uma caixa preta, detalhe a função que uma solução para este problema deve desempenhar para cumprir o objetivo e verifique se os dados relevantes estão disponíveis para resolver o problema.
   * Uma maneira de abordar isso é pensar em como um especialista no assunto poderia resolver o problema manualmente e nos dados necessários; se um especialista humano no assunto não conseguir resolver o problema com os dados disponíveis, isso indica que informações adicionais são necessárias e/ou mais dados precisam ser coletados.
* Com base nos dados disponíveis, defina declarações de hipóteses específicas - que podem ser comprovadas ou refutadas - para orientar a exploração da equipe de ciência de dados. Sempre que possível, cada declaração de hipótese deve ter critérios de sucesso claramente definidos (por exemplo, *com uma precisão superior a 60%*), no entanto, isso nem sempre é possível - especialmente para projetos em que atualmente não existe solução para o problema. Nesses casos, a medida de sucesso pode ser baseada na verificação de um especialista no assunto de que os resultados atendem às suas expectativas.
* Documente todas as informações acima para garantir alinhamento entre as partes interessadas e estabelecer uma compreensão clara do problema a ser resolvido. Tente garantir que o máximo de conhecimento de domínio relevante seja capturado e que as características presentes nos dados disponíveis - e a maneira como os dados foram coletados - sejam claramente explicadas, de modo que possam ser entendidas por um não especialista no assunto.

Depois que uma compreensão do domínio do problema for estabelecida, pode ser necessário decompor o problema geral em partes menores e significativas para manter o foco da equipe e garantir um escopo de projeto realista dentro do prazo estabelecido.

## Ouvindo o Usuário Final

Esses problemas são complexos e requerem compreensão de uma variedade de perspectivas. Não é incomum que as partes interessadas não sejam os usuários finais do framework de solução. Nesses casos, ouvir os usuários finais reais é fundamental para o sucesso do projeto.

As seguintes perguntas podem ajudar a orientar a discussão na compreensão das perspectivas das partes interessadas:

* Quem é o usuário final?
* Qual é a prática atual relacionada ao problema de negócios?
* Qual é o desempenho da solução atual?
* Quais são seus pontos problemáticos?
* Qual é o problema mais difícil deles?
* Qual é o estado dos dados usados para construir a solução?
* Como o usuário final ou o SME (Especialista no Assunto) enxerga a solução?

## Orientações para a Visão

Durante as sessões de visão, o seguinte pode ser útil para orientar a discussão. Muitos desses pontos são retirados diretamente, ou adaptados de, [[1]](#references) e [[2]](#references).

### Formulação do Problema

1. Defina o objetivo em termos de negócios.
2. Como a solução será usada?
3. Quais são as soluções atuais/truques (se houver)? Que trabalho foi feito nessa área até agora? Essa solução precisa se encaixar em um sistema existente?
4. Como o desempenho deve ser medido?
5. A medida de desempenho está alinhada com o objetivo de negócios?
6. Qual seria o desempenho mínimo necessário para atingir o objetivo de negócios?
7. Existem restrições conhecidas em relação a requisitos não funcionais que precisariam ser considerados? (por exemplo, tempos de computação)
8. Estruture esse problema (supervisionado/não supervisionado, online/offline, etc.)
9. Existe experiência humana disponível?
10. Como você resolveria o problema manualmente?
11. Existem restrições quanto ao tipo de abordagens que podem ser usadas? (por exemplo, a solução precisa ser completamente explicável?)
12. Liste as suposições que você ou outros fizeram até agora. Verifique essas suposições, se possível.
13. Defina algumas declarações de hipóteses iniciais a serem exploradas.
14. Destaque e discuta quaisquer preocupações de AI responsável, se apropriado.

### Fluxo de Trabalho

1. Quais habilidades de ciência de dados existem na organização?
2. Quantos cientistas/engenheiros de dados estariam disponíveis para trabalhar neste projeto? Em que capacidade esses recursos estariam disponíveis (tempo integral, meio período, etc.)?
3. Como são as práticas atuais de fluxo de trabalho da equipe? Eles trabalham na nuvem/local? Em cadernos/IDE? O controle de versão é usado?
4. Como os dados, experimentos e modelos são rastreados atualmente?
5. A equipe utiliza uma metodologia Ágil? Como o trabalho é rastreado?
6. Existem atualmente soluções de ML em produção? Quem é responsável por manter essas soluções?
7. Quem seria responsável por manter uma solução produzida durante este projeto?
8. Existem restrições quanto às ferramentas que devem/não devem ser usadas?

## Exemplo - um problema de mecanismo de recomendação

Para ilustrar como o processo acima pode ser aplicado a um domínio de problema tangível, considere, como exemplo, que estamos olhando para implementar um mecanismo de recomendação para uma loja de roupas. Este exemplo foi, em parte, inspirado em [[3]](#references).

Frequentemente, o objetivo pode ser apresentado de forma simples, como "aumentar as vendas". No entanto, embora este seja o principal objetivo, podemos nos beneficiar sendo mais específicos aqui. Suponha que implantamos uma solução em novembro e observamos um aumento nas vendas em dezembro; como poderíamos distinguir quanto disso se deve ao novo mecanismo de recomendação, em oposição ao fato de dezembro ser uma temporada de compras de pico?

Um objetivo melhor, neste caso, seria "aumentar as vendas adicionais, apresentando ao cliente itens que eles *não teriam comprado sem a recomendação*". Aqui, os inputs que podemos controlar são a escolha dos itens apresentados a cada cliente e a ordem em que são exibidos, considerando fatores como com que frequência devem ser alterados, sazonalidade, etc.

Os dados necessários para avaliar uma solução potencial neste caso seriam quais recomendações resultaram em novas vendas e uma estimativa da probabilidade de um cliente comprar um item específico sem uma recomendação. Observe que, embora esses dados também possam ser usados para construir um mecanismo de recomendação, é improvável que esses dados estejam disponíveis antes que um sistema de recomendação tenha sido implementado. Portanto, é provável que tenhamos que usar uma fonte de dados alternativa para construir o modelo.

Podemos ter uma ideia inicial de como abordar uma solução para esse problema considerando como ele seria resolvido por um especialista no assunto. Pensando em como um estilista pessoal pode fazer uma recomendação, é provável que eles recomendem itens com base em um ou mais dos seguintes critérios:

* itens geralmente populares
* itens semelhantes aos que o cliente gosta/compra
* itens que foram gostados/comprados por clientes semelhantes
* itens que são complementares aos que o cliente possui

Embora esta lista não seja exaustiva, ela fornece uma boa indicação dos dados que provavelmente serão úteis para nós:

* dados de vendas de itens
* históricos de compra do cliente
* dados demográficos do cliente
* descrições e tags de itens
* conjuntos de itens anteriormente montados ou conjuntos que foram criados pelo estilista

Então, poderíamos usar esses dados para explorar:

* um método para medir a semelhança entre itens
* um método para medir a semelhança entre clientes
* um método para medir como os itens são complementares uns aos outros

que podem ser usados para criar e classificar recomendações. Dependendo do escopo do projeto e dos dados disponíveis, uma ou mais dessas áreas podem ser selecionadas para criar hipóteses a serem exploradas pela equipe de ciência de dados. Alguns exemplos de hipóteses podem ser:

* A partir das descrições de cada item, podemos determinar uma medida de semelhança entre diferentes itens com um grau de precisão especificado por um estilista.
* Com base no comportamento de clientes com históricos de compra semelhantes, somos capazes de prever quais itens um cliente provavelmente comprará; com uma certeza maior do que a escolha aleatória.
* Usando conjuntos de itens que foram vendidos juntos anteriormente, podemos formular regras em torno das características que determinam se os itens são complementares ou não, o que pode ser verificado por um estilista.

## Próximos Passos

Para garantir clareza e alinhamento, é útil resumir as descobertas da etapa de concepção, concentrando-se em cenários detalhados propostos, suposições e decisões acordadas, bem como próximos passos.

Sugerimos confirmar que você tem acesso a todos os recursos necessários (incluindo dados) como próximo passo antes de prosseguir com as oficinas de exploração de dados.

Aqui estão os links para o modelo de documento de saída e algumas perguntas que podem ser úteis para confirmar o acesso aos recursos.

* [Modelo de Documento de Saída de Escopo Resumido](./ml-envisioning-summary-template.md)
* [Lista de Perguntas de Acesso a Recursos](./ml-data-exploration.md)
* [Lista de Perguntas da Oficina de Exploração de Dados](./ml-data-exploration.md)

## Referências

Muitas das ideias apresentadas aqui - e muito mais - foram inspiradas por e podem ser encontradas nos seguintes recursos, todos altamente recomendados.

1. [Lista de Verificação de Projetos de Aprendizado de Máquina de Aurélien Géron](https://github.com/ageron/handson-ml/blob/master/ml-project-checklist.md)
2. [Lista de Verificação de Projetos de Dados da Fast.ai](https://www.fast.ai/2020/01/07/data-questionnaire)
3. [Designing great data products. Jeremy Howard, Margit Zwemer e Mike Loukides](https://www.oreilly.com/radar/drivetrain-approach-data-products/)
