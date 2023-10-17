# Princípios Sustentáveis

As visões gerais dos princípios a seguir fornecem as bases que apoiam ações específicas no [Checklist de Engenharia Sustentável](./readme.md#sustainable-engineering-checklist). Mais detalhes sobre cada princípio podem ser encontrados seguindo os links nos cabeçalhos ou visitando o [site Princípios de Engenharia de Software Verde](https://principles.green/).

## [Consumo de Eletricidade](https://principles.green/principles/electricity/)

A maior parte da eletricidade ainda é produzida pela queima de combustíveis fósseis e é responsável por 49% do carbono emitido na atmosfera.

O software consome eletricidade em sua execução. A operação de hardware consome eletricidade mesmo com utilização de zero por cento. Algumas das melhores maneiras de reduzir o consumo de eletricidade e as emissões subsequentes de poluição por carbono são tornar nossas aplicações mais eficientes em termos de energia quando estão em execução e limitar o hardware ocioso.

## [Proporcionalidade Energética](https://principles.green/principles/energy-proportionality/)

![Gráfico ilustrativo mostrando que um computador com 0% de utilização consome 100 watts, com 50% de utilização consome 180 watts e com 100% de utilização consome 200 watts. A relação entre consumo de energia e utilização não é linear e não cruza a origem.](https://principles.green/assets/images/principles/energy-proportionality-1.png?v=e5febc24f5d4d4930ad43de3686aa856)

A relação entre energia e utilização não é proporcional.

Quanto mais você utiliza um computador, mais eficiente ele se torna na conversão de eletricidade em operações de computação úteis. Executar seu trabalho em o máximo possível de servidores com a taxa de utilização mais alta maximiza sua eficiência energética.

Um computador ocioso, mesmo funcionando com zero por cento de utilização, ainda consome eletricidade.

## [Carbono Embutido](https://principles.green/principles/embodied-carbon/)

O carbono embutido (também referido como "Carbono Incorporado") é a quantidade de poluição por carbono emitida durante a criação e disposição de um dispositivo. Ao calcular a poluição total por carbono dos computadores que executam seu software, leve em consideração tanto a poluição por carbono para operar o computador quanto o carbono embutido do computador. Portanto, uma ótima maneira de reduzir o carbono embutido é evitar a necessidade de fabricar novos dispositivos, estendendo a utilidade dos existentes.

## [Moldagem da Demanda](https://principles.green/principles/demand-shaping/)

A moldagem da demanda é uma estratégia de moldar nossa demanda por recursos para que ela corresponda à oferta existente.

Se a oferta for alta, aumente a demanda fazendo mais em suas aplicações. Se a oferta for baixa, diminua a demanda. Isso significa fazer menos em suas aplicações ou adiar o trabalho até que a oferta seja maior.

## [Rede](https://principles.green/principles/networking/)

Uma rede é uma série de switches, roteadores e servidores. Todos os computadores e equipamentos de rede em uma rede consomem eletricidade e têm [carbono embutido](#embodied-carbon). A internet é uma rede global de dispositivos normalmente alimentados pela mistura padrão de energia da rede local.

Quando você envia dados pela internet, está enviando esses dados por muitos dispositivos na rede, cada um desses dispositivos consome eletricidade. Como resultado, qualquer dado que você envia ou recebe pela internet emite carbono.

A quantidade de carbono emitida ao enviar dados depende de muitos fatores, incluindo:

- Distância que os dados percorrem
- Número de saltos entre dispositivos de rede
- Eficiência energética dos dispositivos de rede
- Intensidade de carbono da energia usada por cada dispositivo no momento em que os dados são transmitidos.
- Protocolo de rede usado para coordenar a transmissão de dados - por exemplo, multiplexação, compressão de cabeçalho, TLS/Quic

[Estudos recentes de redes - Cloud Carbon Footprint](https://www.cloudcarbonfootprint.org/docs/methodology/#appendix-iv-recent-networking-studies)
