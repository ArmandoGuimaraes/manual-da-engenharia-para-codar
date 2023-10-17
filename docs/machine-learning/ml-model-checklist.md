# Lista de Verificação para Produção de Modelo de ML

O objetivo desta lista de verificação é garantir que:

- A equipe avaliou se o modelo está pronto para a produção antes de avançar para o processo de pontuação.
- A equipe preparou um plano de produção para o modelo.

A lista de verificação fornece diretrizes para a criação deste plano de produção. Deve ser utilizada por equipes/organizações que já construíram/treinaram um modelo de ML e agora estão considerando colocá-lo em produção.

## Lista de Verificação

Antes de colocar um modelo de ML individual em produção, os seguintes aspectos devem ser considerados:

- [ ] [Existe uma linha de base bem definida? O modelo está se saindo melhor do que a linha de base?](#existe-uma-linha-de-base-bem-definida-o-modelo-está-se-saindo-melhor-do-que-a-linha-de-base)
- [ ] [As métricas de desempenho de aprendizado de máquina estão definidas tanto para o treinamento quanto para a pontuação?](#as-métricas-de-desempenho-de-aprendizado-de-máquina-estão-definidas-tanto-para-o-treinamento-quanto-para-a-pontuação)
- [ ] [O modelo foi comparado com outros modelos de referência?](#o-modelo-foi-comparado-com-outros-modelos-de-referência)
- [ ] [É possível obter ou inferir a verdade fundamental na produção?](#é-possível-obter-ou-inferir-a-verdade-fundamental-na-produção)
- [ ] [A distribuição dos dados nos conjuntos de treinamento, teste e validação foi analisada?](#a-distribuição-dos-dados-nos-conjuntos-de-treinamento-teste-e-validação-foi-analisada)
- [ ] [Foram estabelecidos objetivos e limites rígidos para o desempenho, velocidade de previsão e custos, de modo que possam ser considerados se forem necessárias compensações?](#foram-estabelecidos-objetivos-e-limites-rígidos-para-o-desempenho-velocidade-de-previsão-e-custos-de-modo-que-possam-ser-considerados-se-forem-necessárias-compensações)
- [ ] [Como o modelo será integrado a outros sistemas e qual será seu impacto?](#como-o-modelo-será-integrado-a-outros-sistemas-e-qual-será-seu-impacto)
- [ ] [Como a qualidade dos dados de entrada será monitorada?](#como-a-qualidade-dos-dados-de-entrada-será-monitorada)
- [ ] [Como será monitorada a variação nas características dos dados?](#como-será-monitorada-a-variação-nas-características-dos-dados)
- [ ] [Como o desempenho será monitorado?](#como-o-desempenho-será-monitorado)
- [ ] [Foram consideradas preocupações éticas?](#foram-consideradas-preocupações-éticas)

Por favor, observe que pode haver cenários em que não seja possível verificar todos os itens desta lista de verificação. No entanto, é aconselhável passar por todos os itens e tomar decisões informadas com base em seu caso de uso específico.

## A performance do seu modelo será diferente na produção do que na fase de treinamento

Uma vez implantado na produção, o modelo pode apresentar um desempenho muito pior do que o esperado. Esse desempenho ruim pode ser resultado de:

- Os dados a serem pontuados na produção são significativamente diferentes dos conjuntos de treinamento e teste.
- As etapas de engenharia de recursos são diferentes ou inconsistentes na produção em comparação com o processo de treinamento.
- A medida de desempenho não é consistente (por exemplo, seu conjunto de testes abrange vários meses de dados, enquanto a métrica de desempenho para a produção foi calculada para um mês de dados).

### Existe uma linha de base bem definida? O modelo está se saindo melhor do que a linha de base?

Uma boa maneira de pensar em uma linha de base para um modelo é o modelo mais simples que se pode imaginar: seja um limite simples, uma suposição aleatória ou um modelo linear muito básico. Esta linha de base é o ponto de referência que seu modelo precisa superar. Uma linha de base bem definida é diferente para cada tipo de problema e não há uma abordagem única para todos os casos.

Como exemplo, consideremos alguns tipos comuns de problemas de aprendizado de máquina:

- **Classificação**: Prever entre uma classe positiva e uma classe negativa. A classe com mais observações ou um modelo de regressão logística simples pode ser a linha de base.
- **Regressão**: Prever os preços das casas em uma cidade. O preço médio das casas no último ano ou no último mês, um modelo de regressão linear simples ou o preço médio anterior das casas em um bairro podem ser a linha de base.
- **Classificação de imagens**: Construir um classificador de imagens para distinguir entre gatos e não gatos em uma imagem. Se suas classes forem desequilibradas, por exemplo, 70% de gatos e 30% de não gatos, e se você sempre prever gatos, seu classificador ingênuo terá uma precisão de 70%, e isso pode ser sua linha de base. Se suas classes forem equilibradas, por exemplo, 52% de gatos e 48% de não gatos, então uma arquitetura convolucional simples pode ser a linha de base (1 camada convolucional + 1 max pooling + 1 densa). Além disso, a precisão humana na rotulação também pode ser a linha de base em um cenário de classificação de imagens.

Algumas perguntas a fazer ao comparar com uma linha de base:

- Como o seu modelo se compara a uma suposição aleatória?
- Como o desempenho do seu modelo se compara à aplicação de um limite simples?
- Como o seu modelo se compara com a previsão constante do valor mais comum?

**Nota**: Em alguns casos, a paridade humana pode ser muito ambiciosa como linha de base, mas isso deve ser decidido caso a caso. A precisão humana é uma das opções disponíveis, mas não a única.

Recursos:

- [Artigo "Como obter Resultados de Linha de Base e por que eles Importam"](https://machinelearningmastery.com/how-to-get-baseline-results-and-why-they-matter/)
- [Artigo "Sempre comece com um modelo tolo, sem exceções"](https://blog.insightdatascience.com/always-start-with-a-stupid-model-no-exceptions-3a22314b9aaa)

### As métricas de desempenho de aprendizado de máquina estão definidas tanto para o treinamento quanto para a pontuação?

A metodologia de traduzir as métricas de treinamento para as métricas de pontuação deve ser bem definida e compreendida. Dependendo do tipo de dados e do modelo, o cálculo das métricas do modelo pode ser diferente na produção e no treinamento. Por exemplo, o procedimento de treinamento calculou métricas por um longo período de tempo (um ano, uma década) com diferentes características sazonais, enquanto o procedimento de pontuação calculará as métricas em um intervalo de tempo restrito (por exemplo, uma semana, um mês, um trimestre). Métricas de desempenho de ML bem definidas são essenciais na produção para que uma diminuição ou aumento no desempenho do modelo possa ser detectada com precisão.

Coisas a serem consideradas:

- No caso de previsão, se você alterar o período de avaliação do desempenho, de um mês para um ano, por exemplo, você pode obter um resultado diferente. Por exemplo, se seu modelo está prevendo as vendas de um produto por dia e o RMSE (Erro Quadrático Médio) é muito baixo no primeiro mês em que o modelo está em produção. Conforme o modelo fica ativo por mais tempo, o RMSE aumenta, tornando-se 10 vezes maior do que o RMSE do primeiro mês em comparação com o primeiro ano.

- Em um cenário de classificação, a precisão geral pode ser boa, mas o modelo pode estar se saindo mal para alguns subgrupos. Por exemplo, um classificador tem uma precisão de 80% no geral, mas apenas 55% para o grupo etário de 20-30 anos. Se este for um grupo etário significativo para os dados de produção, sua precisão pode sofrer consideravelmente quando estiver em produção.

- Em um cenário de classificação de cena, o modelo tenta identificar uma cena específica em um vídeo, e o modelo foi treinado e testado (divisão de 80-20) em 50000 segmentos, onde metade dos segmentos contém a cena e metade não contém. A precisão no conjunto de treinamento é de 85% e 84% no conjunto de teste. No entanto, quando um vídeo inteiro é pontuado, as pontuações são obtidas em todos os segmentos, e esperamos que poucos segmentos contenham a cena. A precisão para um vídeo inteiro não é comparável ao procedimento de treinamento/teste neste caso, portanto, métricas diferentes devem ser consideradas.

- Se técnicas de amostragem (sobreamostragem, subamostragem) forem usadas para treinar o modelo quando as classes estiverem desequilibradas, certifique-se de que as métricas usadas durante o treinamento sejam comparáveis com as usadas na pontuação.

- Se o número de amostras usadas para treinamento e teste for pequeno, as métricas de desempenho podem mudar significativamente à medida que novos dados são pontuados.

### O modelo foi benchmarked?

O modelo treinado a ser colocado em produção está bem benchmarked se as métricas de desempenho de aprendizado de máquina (como precisão, recall, RMSE ou o que for apropriado) forem medidas no conjunto de treinamento e teste. Além disso, a divisão entre conjunto de treinamento e teste deve estar bem documentada e ser reproduzível.

### É possível obter ou inferir a verdade fundamental na produção?

Sem uma verdade fundamental confiável, as métricas de aprendizado de máquina não podem ser calculadas. É importante identificar se a verdade fundamental pode ser obtida à medida que o modelo pontua novos dados por meio de meios manuais ou automáticos. Se a verdade fundamental não puder ser obtida de forma sistemática, outras proxies e metodologias devem ser investigadas para obter alguma medida do desempenho do modelo.

Uma opção é usar seres humanos para rotular amostras manualmente. Um aspecto importante da rotulagem humana é levar em consideração a precisão humana. Se dois indivíduos diferentes rotularem uma imagem, é provável que os rótulos sejam diferentes para algumas amostras. É importante entender como os rótulos foram obtidos para avaliar a confiabilidade da verdade fundamental (daí falarmos sobre a precisão humana).

Para maior clareza, consideremos os seguintes exemplos (de forma alguma uma lista exaustiva):

- **Previsão**: Cenários de previsão são exemplos de problemas de aprendizado de máquina em que a verdade fundamental pode ser obtida na maioria dos casos, mesmo que haja um atraso. Por exemplo, para um modelo que prevê as vendas de sorvete em uma loja local, a verdade fundamental será obtida à medida que as vendas acontecerem, mas pode aparecer no sistema em um momento posterior em relação à previsão do modelo.

- **Sistemas de recomendação**: Para sistemas de recomendação, obter a verdade fundamental é um problema complexo na maioria dos casos, pois não há como identificar a recomendação ideal. Para um site de varejo, por exemplo, cliques/não cliques, compra/não compra ou outras interações do usuário com a recomendação podem ser usados como proxies da verdade fundamental.

- **Detecção de objetos em imagens**: Para um modelo de detecção de objetos, à medida que novas imagens são pontuadas, não há novos rótulos sendo gerados automaticamente. Uma opção para obter a verdade fundamental para as novas imagens é usar pessoas para rotular as imagens manualmente. A rotulagem humana é cara, demorada e não é 100% precisa, então na maioria dos casos, apenas um subconjunto das imagens pode ser rotulado. Essas amostras podem ser escolhidas aleatoriamente ou usando técnicas de aprendizado ativo para selecionar as amostras não rotuladas mais informativas.

### A distribuição de dados dos conjuntos de treinamento, teste e validação (se aplicável) foi analisada?

A distribuição de dados dos seus conjuntos de treinamento, teste e validação (se aplicável) deve ser analisada para garantir que todos eles provenham da mesma distribuição. Se isso não for o caso, algumas opções a serem consideradas são: reembaralhar, reamostrar, modificar os dados, coletar mais amostras ou remover recursos do conjunto de dados.

Diferenças significativas nas distribuições de dados dos diferentes conjuntos de dados podem afetar grandemente o desempenho do modelo. Algumas perguntas potenciais a fazer:

- Quanto os dados de treinamento e teste representam o resultado final?
- A distribuição de cada característica individual é consistente em todos os seus conjuntos de dados? (ou seja, a mesma representação de grupos etários, gênero, raça, etc.)
- Existem informações sobre a origem dos dados? De onde vieram os dados? Como os dados foram coletados? A coleta e a rotulagem podem ser automatizadas?

Recursos:

- [Tutorial "Dividindo em conjuntos de treinamento, desenvolvimento e teste"](http://cs230.stanford.edu/blog/split/)

### Foram estabelecidos objetivos e limites rígidos para o desempenho, velocidade de previsão e custos, de modo que possam ser considerados se forem necessárias compensações?

Alguns modelos de aprendizado de máquina alcançam alto desempenho de ML, mas são caros e demorados para serem executados. Nesses casos, um modelo menos performático e mais barato pode ser preferido. Portanto, é importante calcular as métricas de desempenho do modelo (precisão, precisão, recall, RMSE etc.), mas também coletar dados sobre o custo de execução do modelo e quanto tempo levará para executar. Uma vez que esses dados sejam reunidos, uma decisão informada deve ser tomada sobre qual modelo será colocado em produção.

Métricas do sistema a serem consideradas:

- Uso de CPU/GPU/memória
- Custo por previsão
- Tempo necessário para fazer uma previsão

### Como o modelo será integrado a outros sistemas e qual será o impacto?

Modelos de Aprendizado de Máquina não existem isoladamente, mas fazem parte de um sistema muito maior. Esses sistemas podem ser sistemas antigos e proprietários ou novos sistemas sendo desenvolvidos como resultado da criação de um novo modelo de aprendizado de máquina. Em ambos os casos, é importante entender onde o modelo real se encaixará, qual saída é esperada do modelo e como essa saída será usada pelo sistema maior. Além disso, é essencial decidir se o modelo será usado para inferência em lote e/ou em tempo real, pois os caminhos de produção podem ser diferentes.

Possíveis perguntas para avaliar o impacto do modelo:

- Há um humano no loop?
- Como o feedback é coletado por meio do sistema? (por exemplo, como sabemos se uma previsão está errada)
- Existe um mecanismo de fallback quando as coisas dão errado?
- O sistema é transparente de que há um modelo fazendo uma previsão e quais dados são usados para fazer essa previsão?
- Qual é o custo de uma previsão errada?

### Como a qualidade dos dados de entrada será monitorada?

À medida que os sistemas de dados se tornam cada vez mais complexos no cenário atual, é especialmente vital empregar protocolos de monitoramento, alerta e correção de qualidade de dados. Seguir as melhores práticas de validação de dados pode prevenir problemas insidiosos que se infiltram nos modelos de aprendizado de máquina, reduzindo a utilidade do modelo na melhor das hipóteses e introduzindo riscos na pior das hipóteses. A validação de dados reduz o risco de interrupções nos dados (aumentando a folga) e dívidas técnicas, além de apoiar o sucesso a longo prazo de modelos de aprendizado de máquina e outras aplicações que dependem dos dados.

As melhores práticas de validação de dados incluem:

- Utilizar processos automatizados de teste de qualidade de dados em cada estágio do pipeline de dados.
- Roteamento de dados que falham nos testes de qualidade para um armazenamento de dados separado para diagnóstico e resolução.
- Utilização de observabilidade de dados de ponta a ponta quanto à atualidade, distribuição, volume, esquema e linhagem dos dados.

Observe que a validação de dados é distinta da detecção de deriva de dados. A validação de dados detecta erros nos dados (por exemplo, um dado está fora da faixa esperada), enquanto a detecção de deriva de dados descobre mudanças legítimas nos dados que são verdadeiramente representativas do fenômeno em questão (por exemplo, as preferências do usuário mudam). Problemas de validação de dados devem acionar o roteamento e a correção, enquanto a deriva de dados deve acionar a adaptação ou o retrabalho de um modelo.

Recursos:

- ["Fundamentos da Qualidade de Dados" por Moses et al.](https://www.oreilly.com/library/view/data-quality-fundamentals/9781098112035/)

### Como será monitorada a deriva nas características dos dados?

A detecção de deriva de dados descobre mudanças legítimas nos dados de entrada que são verdadeiramente representativas do fenômeno em questão e não são errôneas (por exemplo, mudança nas preferências do usuário). É imperativo entender se os novos dados em produção serão significativamente diferentes dos dados na fase de treinamento. Também é importante verificar se as informações de distribuição de dados podem ser obtidas para qualquer um dos novos dados que estão chegando. O monitoramento de deriva pode informar quando as mudanças estão ocorrendo e quais são suas características (por exemplo, abruptas vs. graduais) e guiar estratégias eficazes de adaptação ou retrabalho para manter o desempenho.

Possíveis perguntas a fazer:

- Quais são alguns exemplos de deriva, ou desvio da norma, que foram experimentados no passado ou que podem ser esperados?
- Existe uma estratégia de detecção de deriva em vigor? Ela está alinhada com os tipos esperados de mudanças?
- Existem avisos quando anomalias nos dados de entrada estão ocorrendo?
- Existe uma estratégia de adaptação em vigor? Ela está alinhada com os tipos esperados de mudanças?

Recursos:

- ["Aprendizado sob Deriva de Conceito: Uma Revisão" por Lu et al.](https://arxiv.org/pdf/2004.05785.pdf)
- [Compreendendo a mudança de conjunto de dados](https://towardsdatascience.com/understanding-dataset-shift-f2a5a262a766)

### Como o desempenho será monitorado?

É importante definir como o modelo será monitorado quando estiver em produção e como esses dados serão usados para tomar decisões. Por exemplo, quando um modelo precisa ser re-treinado devido à degradação do desempenho e como identificar as causas subjacentes dessa degradação podem fazer parte dessa metodologia de monitoramento.

Idealmente, o monitoramento do modelo deve ser feito automaticamente. No entanto, se isso não for possível, deve haver uma verificação manual periódica do desempenho do modelo.

O monitoramento do modelo deve levar a:

- Capacidade de identificar mudanças no desempenho do modelo.
- Avisos quando anomalias na saída do modelo estão ocorrendo.
- Decisões de re-treinamento e estratégia de adaptação.

### Foram consideradas quaisquer preocupações éticas?

Todos os projetos de Aprendizado de Máquina passam pelo processo de [AI Responsável](responsible-ai.md) para garantir que eles sigam os [6 princípios de AI Responsável da Microsoft](https://www.microsoft.com/en-us/ai/responsible-ai).
