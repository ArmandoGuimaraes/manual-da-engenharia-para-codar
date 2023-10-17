# Estudos de Viabilidade

O principal objetivo dos estudos de viabilidade é avaliar se é viável resolver o problema de forma satisfatória usando aprendizado de máquina (ML) com os dados disponíveis. Queremos evitar investir demais na solução antes de termos:

* Evidências suficientes de que uma solução seria a melhor solução técnica, dadas as considerações do caso de negócios.
* Evidências suficientes de que uma solução é compatível com o contexto do problema.
* Evidências suficientes de que uma solução é possível.
* Alguma direção validada sobre como uma solução deve ser.

Esse esforço garante soluções de qualidade respaldadas pela quantidade apropriada de consideração e evidência.

## Quando são úteis os estudos de viabilidade?

Cada engajamento pode se beneficiar de um estudo de viabilidade no início do projeto.

Discussões arquitetônicas ainda podem ocorrer em paralelo enquanto a equipe trabalha para obter uma compreensão sólida e definição do que será construído.

Os estudos de viabilidade podem durar entre 4 e 16 semanas, dependendo dos detalhes específicos do problema, volume de dados, estado dos dados, etc. Começar com um marco de 4 semanas pode ser útil, durante o qual pode ser determinado quanto mais tempo, se houver, é necessário para a conclusão.

## Quem colabora nos estudos de viabilidade?

A colaboração de pessoas com conjuntos de habilidades diversas é desejada nessa fase, incluindo cientistas de dados, engenheiros de dados, engenheiros de software, gerentes de projeto, pesquisadores de experiência humana e especialistas do domínio. Isso abraça o uso de fundamentos de engenharia, com alguma flexibilidade. Por exemplo, nem toda experimentação requer cobertura de teste completa e revisão de código. A experimentação geralmente não faz parte de um pipeline de CI/CD (Integração Contínua e Entrega Contínua). Os artefatos podem estar na branch `main` como uma pasta excluída do pipeline de CI/CD ou como uma branch experimental separada, dependendo das preferências do cliente/equipe.

## O que os estudos de viabilidade envolvem?

### Definição do problema e resultado desejado

* Garantir que o problema seja complexo o suficiente para que regras de codificação ou dimensionamento manual sejam irrealistas.
* Definição clara do problema do ponto de vista de negócios e técnico.

### Compreensão contextual profunda

Confirme que as seguintes perguntas podem ser respondidas com base no que foi aprendido durante a Fase de Descoberta do projeto. Para itens que não podem ser respondidos satisfatoriamente, empreenda investigações adicionais para respondê-los.

* Compreender as pessoas que estão usando e/ou sendo afetadas pela solução.
* Compreender as forças contextuais em jogo em torno do problema, incluindo metas, cultura e contexto histórico.
* Para realizar isso, um pesquisador irá:
  * Colaborar com clientes e colegas para explorar o cenário das pessoas que se relacionam e podem ser afetadas pelo espaço do problema sendo explorado (usuários, partes interessadas, especialistas no assunto, etc.).
  * Formular as perguntas de pesquisa a serem abordadas.
  * Selecionar e projetar a pesquisa que melhor servirá às perguntas de pesquisa.
  * Identificar e selecionar participantes representativos da pesquisa em todo o espaço do problema com os quais conduzir a pesquisa.
  * Construir um plano de pesquisa e documentos de preparação necessários para o método de pesquisa selecionado.
  * Conduzir a atividade de pesquisa com os participantes por meio dos métodos selecionados.
  * Sintetizar, analisar e interpretar as descobertas da pesquisa.
  * Quando relevante, construir estruturas, artefatos e processos que ajudem a explorar as descobertas e implicações da pesquisa em toda a equipe.
  * Compartilhar o que foi descoberto e entendido, bem como suas implicações, com toda a equipe de engajamento e partes interessadas relevantes.
* Se a pesquisa acima foi conduzida durante a fase de Descoberta, ela deve ser revisada, e quaisquer lacunas significativas de conhecimento devem ser identificadas e preenchidas seguindo o processo acima.

### Acesso aos dados

* Verificar se toda a equipe tem acesso aos dados.
* Configurar um ambiente dedicado e/ou restrito, se necessário.
* Realizar qualquer desidentificação ou omissão de informações sensíveis.
* Compreender os requisitos de acesso aos dados (retenção, acesso baseado em função, etc.).

### Descoberta de dados

* Realizar uma oficina de [exploração de dados](ml-data-exploration.md) e aprofundar com especialistas do domínio.
* Compreender a disponibilidade dos dados e confirmar o acesso da equipe.
* Compreender o dicionário de dados, se disponível.
* Compreender a qualidade dos dados. Já existe uma estratégia de validação de dados em vigor?
* Garantir que os dados necessários estejam presentes em volumes razoáveis.
* Para problemas supervisionados (mais comuns), avaliar a disponibilidade de rótulos ou dados que possam ser usados para aproximar efetivamente rótulos.
* Se aplicável, garantir que todos os dados possam ser unidos conforme necessário e entender como isso é feito.
  * Idealmente, obter ou criar um diagrama de relacionamento de entidades (ERD).
* Potencialmente descobrir novas fontes úteis de dados.

### Descoberta de arquitetura

* Imagem clara da arquitetura existente.
* Pontos de arquitetura.

### Ideação e iteração de conceitos

* Desenvolver propostas de valor para usuários e partes interessadas com base na compreensão contextual desenvolvida por meio do processo de descoberta (por exemplo, elementos-chave de valor, benefícios).
* Conforme relevante, faça uso de
  * Co-criação com a equipe.
  * Co-criação com usuários e partes interessadas.
* Conforme relevante, crie vinhetas, narrativas ou outros materiais para comunicar o conceito.
* Identificar o próximo conjunto de hipóteses ou incógnitas a serem testadas (veja teste de conceito).
* Revisitar e iterar o conceito ao longo da descoberta à medida que a compreensão do espaço do problema evolui.

### Análise explorató

ria de dados (AED)

* Aprofundamento nos dados.
* Compreender as distribuições de valores de recursos e rótulos.
* Compreender as correlações entre recursos e entre recursos e rótulos.
* Compreender restrições específicas do problema de dados, como valores ausentes, cardinalidade categórica, potencial para vazamento de dados, etc.
* Identificar quaisquer lacunas nos dados que não puderam ser identificadas na fase de descoberta de dados.
* Abrir o caminho para uma compreensão mais aprofundada sobre quais técnicas são aplicáveis.
* Estabelecer uma compreensão mútua do que está dentro ou fora do escopo para viabilidade, garantindo que os dados dentro do escopo sejam significativos para o negócio.

### Pré-processamento de dados

* Acontece durante a AED e o teste de hipóteses.
* Engenharia de recursos.
* Amostragem.
* Dimensionamento e/ou discretização.
* Tratamento de ruído.

### Teste de hipóteses

* Projete várias soluções potenciais usando algoritmos e técnicas teoricamente aplicáveis, começando com a linha de base mais simples razoável.
* Treine modelos.
* Avalie o desempenho e determine se é satisfatório.
* Ajuste os designs de solução experimental com base nos resultados.
* Itere.
* Documente minuciosamente cada etapa e resultado, além de quaisquer hipóteses resultantes para facilitar o acompanhamento do processo de tomada de decisão.

### Teste de conceito

* Conforme relevante, para testar a proposta de valor, conceitos ou aspectos da experiência.
* Planeje pesquisa com usuários, partes interessadas e especialistas.
* Desenvolva e projete materiais de pesquisa necessários.
* Sintetize e avalie feedback para incorporar no desenvolvimento do conceito.
* Continue a iterar e testar diferentes elementos do conceito conforme necessário, incluindo testes para melhor atender aos objetivos e diretrizes de AI responsável.
* Garanta que a solução proposta e o enquadramento sejam compatíveis e aceitáveis para as pessoas afetadas.
* Garanta que a solução proposta e o enquadramento sejam compatíveis com os objetivos e contexto de negócios existentes.

### Avaliação de risco

* Identificação e avaliação de riscos e restrições.

### AI Responsável

* Consideração dos princípios de AI responsável.
* Compreensão dos contextos, necessidades e preocupações dos usuários e partes interessadas para informar o desenvolvimento da AI responsável.
* Teste de conceito de AI e elementos de experiência com usuários e partes interessadas.
* Discussão e feedback de diversas perspectivas em torno de preocupações de AI responsável, quando relevante.

## Resultado de um estudo de viabilidade

### Possíveis resultados

O principal resultado é um relatório de estudo de viabilidade, com uma recomendação sobre os próximos passos:
- Se não houver evidências suficientes para apoiar a hipótese de que este problema pode ser resolvido usando ML, de acordo com as medidas de desempenho predefinidas e o impacto nos negócios:

  * Detalhamos as lacunas e desafios que nos impediram de alcançar um resultado positivo.
  * Podemos restringir o escopo do projeto, se aplicável.
  * Podemos reavaliar o problema levando em consideração as descobertas do estudo de viabilidade.
  * Avaliamos a possibilidade de coletar mais dados ou melhorar a qualidade dos dados.

- Se houver evidências suficientes para apoiar a hipótese de que este problema pode ser resolvido usando ML
  * Fornecer recomendações e ativos técnicos para avançar para a fase de operacionalização.
