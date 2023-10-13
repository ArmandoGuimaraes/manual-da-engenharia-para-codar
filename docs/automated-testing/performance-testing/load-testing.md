# Teste de Carga

"*O teste de carga é realizado para determinar o comportamento de um sistema sob condições normais e de pico de carga antecipadas.*" - [Teste de carga - Wikipedia](https://en.wikipedia.org/wiki/Load_testing)

Um teste de carga é projetado para determinar como um sistema se comporta sob cargas de trabalho normais e de pico esperadas. Especificamente, seu principal objetivo é confirmar se um sistema pode lidar com o nível de carga esperado. Dependendo do sistema-alvo, isso pode ser usuários simultâneos, solicitações por segundo ou tamanho de dados.

## Por que Teste de Carga

O principal objetivo é provar que o sistema pode se comportar normalmente sob a carga normal esperada antes de liberá-lo para produção. Os critérios que definem "comportar-se normalmente" dependerão do seu alvo; isso pode ser tão simples quanto "o sistema permanece disponível", mas também pode incluir o cumprimento de um SLA de tempo de resposta ou taxa de erro.

Além disso, os resultados de um teste de carga também podem ser usados como dados para ajudar no planejamento de capacidade e no cálculo da escalabilidade.

## Blocos de Design do Teste de Carga

Existem vários componentes básicos necessários para realizar um teste de carga.

1. Para ter resultados significativos, o sistema precisa ser testado em um ambiente semelhante à produção, com uma rede e hardware que se assemelhem ao ambiente de implantação esperado.

2. O teste de carga consistirá em um módulo que simula a atividade do usuário. Claro, a composição dessa "atividade do usuário" variará com base no tipo de aplicação que está sendo testada. Por exemplo, um site de comércio eletrônico pode simular a navegação e compra de itens do usuário, mas um pipeline de ingestão de dados de IoT simularia um fluxo de leituras de dispositivos. Certifique-se de que a simulação seja o mais próxima possível da atividade real e considere não apenas o volume, mas também os padrões e a variabilidade. Por exemplo, se os dados do simulador forem muito uniformes ou previsíveis, as proporções de acerto/erro do cache podem afetar seus resultados.

3. O teste de carga será iniciado a partir de um componente externo ao sistema-alvo, que pode controlar a quantidade de carga aplicada. Isso pode ser um único agente, mas pode precisar ser dimensionado para vários agentes para alcançar níveis mais altos de atividade.

4. Embora não seja necessário para executar um teste de carga, é aconselhável ter monitoramento e/ou registro em vigor para poder medir o impacto do teste e descobrir gargalos potenciais.

## Aplicando o Teste de Carga

### Planejamento

1. **Identificar cenários-chave para medir** - Reúna esses cenários do Product Owner; eles devem fornecer uma amostra representativa do tráfego do mundo real.
2. **Determinar carga normal e de pico esperadas para os cenários** - Determine um nível de carga, como usuários simultâneos ou solicitações por segundo, para encontrar o tamanho do teste de carga que você executará.
3. **Identificar métricas de critérios de sucesso** - Essas podem estar no lado do teste, como tempo de resposta e taxa de erro, ou podem estar no lado do sistema, como uso de CPU e memória.
4. **Selecionar a ferramenta certa** - Existem muitos frameworks para teste de carga, então considere se os recursos e limitações são adequados para suas necessidades. (Algumas ferramentas populares estão listadas abaixo).
5. **Observabilidade** - Determine quais métricas precisam ser coletadas para obter informações sobre taxa de transferência, latência, utilização de recursos, etc.
6. **Escalabilidade** - Determine a quantidade de escala necessária pelo gerador de carga, aplicação de carga de trabalho, CPU, Memória e componentes de rede necessários para alcançar os objetivos de teste. O uso de Kubernetes na nuvem pode ser usado para tornar o teste infinitamente escalável.

### Execução

É recomendado o uso de um framework de teste existente (veja abaixo). Essas ferramentas fornecerão um método para especificar os cenários de atividade do usuário e como executá-los sob carga. É comum aumentar lentamente a carga desejada para replicar melhor o comportamento do mundo real. Depois de atingir sua carga de trabalho definida, mantenha esse nível o tempo suficiente para ver se seu sistema se estabiliza. Para finalizar o teste, você também deve aumentar a carga para ver como o sistema desacelera.

Você também deve considerar a origem do tráfego do seu teste de carga. Dependendo do escopo do sistema-alvo, você pode querer iniciar de um local diferente para replicar melhor o tráfego do mundo real, como de uma região diferente.

**Nota:** Antes de começar, esteja ciente de quaisquer restrições em sua rede, como proteção DDOS, onde você pode precisar notificar um administrador de rede ou solicitar uma isenção.

### Testes Adicionais

Após concluir seu teste de carga, você deve estar preparado para continuar com testes adicionais relacionados, como:

- **Teste de Soak** - Também conhecido como **Teste de Resistência**. Realizar um teste de carga por um período prolongado para garantir a estabilidade a longo prazo.
- **Teste de Estresse** - Aumentar gradualmente a carga para encontrar os limites do sistema e identificar a capacidade máxima.
- **Teste de Pico** - Introduzir um aumento acentuado de curto prazo nos cenários de carga.
- **Teste de Escalabilidade** - Re-teste de um sistema à medida que você expande horizontal ou verticalmente para medir como ele escala.
- **Teste Distribuído** - O teste distribuído permite que você aproveite o poder de várias máquinas para realizar testes maiores ou mais aprofundados mais rapidamente. É necessário quando um nó totalmente otimizado não pode produzir a carga exigida pelo seu teste extremamente grande.

## Frameworks e Ferramentas de Geração de Carga de Teste

Aqui estão alguns frameworks de teste de carga populares que você pode considerar, e as linguagens usadas para definir seus cenários.

- **Azure Load Testing** ([<https://learn.microsoft.com/en-us/azure/load-testing/>](https://learn.microsoft.com/en-us/azure/load-testing/)) - Plataforma gerenciada para executar testes

 de carga no Azure. Permite executar e monitorar testes automaticamente, obter segredos do KeyVault, gerar tráfego em escala e testar endpoints privados do Azure. No caso simples, executa testes de carga com tráfego HTTP GET para um determinado endpoint. Para os casos mais complexos, você pode fazer upload de seus próprios **cenários JMeter**.
- **JMeter** ([<https://github.com/apache/jmeter>](https://github.com/apache/jmeter)) - Possui padrões integrados para testar sem codificação, mas pode ser estendido com Java.
- **Artillery** ([<https://artillery.io/>](https://artillery.io/)) - Escreva seus cenários em Javascript, executa uma aplicação node.
- **Gatling** ([<https://gatling.io/>](https://gatling.io/)) - Escreva seus cenários em Scala com seu DSL.
- **Locust** ([<https://locust.io/>](https://locust.io/)) - Escreva seus cenários em Python usando o conceito de atividade de usuário simultâneo.
- **K6** ([<https://k6.io/>](https://k6.io/)) - Escreva seus cenários de teste em Javascript, disponível como operador Kubernetes de código aberto, imagem Docker de código aberto ou como SaaS. Particularmente útil para teste de carga distribuído. Integra-se facilmente com o Prometheus.
- **NBomber** ([<https://nbomber.com/>](https://nbomber.com/)) - Escreva seus cenários de teste em C# ou F#, integração disponível com test runners (NUnit/xUnit).
- **WebValidate** ([<https://github.com/microsoft/webvalidate>](https://github.com/microsoft/webvalidate)) - Ferramenta de validação de solicitação web usada para executar testes de ponta a ponta e testes de desempenho e disponibilidade de longa duração.

## Aplicações de Carga de Trabalho de Amostra

No caso em que uma aplicação de carga de trabalho específica não está sendo fornecida e o foco está, em vez disso, no sistema, aqui estão algumas aplicações de carga de trabalho de amostra populares que você pode considerar.

- **HttpBin** ([Python](https://github.com/postmanlabs/httpbin), [GoLang](https://github.com/mccutchen/go-httpbin)) - Suporta vários tipos de endpoints e implementações de linguagem. Pode ecoar dados usados na solicitação.
- **NGSA** ([Java](https://github.com/retaildevcrews/ngsa-java), [C#](https://github.com/retaildevcrews/ngsa-java)) - Destinado ao teste de plataforma e monitoramento do Kubernetes. Construído em cima da loja de dados IMDB com muitos endpoints CRUD disponíveis. Não precisa ter uma conexão de banco de dados ao vivo.
- **MockBin** ([<https://github.com/Kong/mockbin>](https://github.com/Kong/mockbin)) - Permite que você gere endpoints personalizados para testar, simular e rastrear solicitações e respostas HTTP entre bibliotecas, sockets e APIs.

## Conclusão

Um teste de carga é uma etapa crítica para entender se um sistema-alvo será confiável sob o tráfego real esperado.

Claro, ele só é tão bom quanto sua capacidade de prever a carga esperada, então é importante seguir com outros testes adicionais para realmente entender como seu sistema se comporta em diferentes situações.

## Recursos

Lista de leituras adicionais sobre este tipo de teste para aqueles que gostariam de se aprofundar.

- [Microsoft Azure Well-Architected Framework > Teste de Carga](https://learn.microsoft.com/en-us/azure/architecture/framework/scalability/load-testing)
