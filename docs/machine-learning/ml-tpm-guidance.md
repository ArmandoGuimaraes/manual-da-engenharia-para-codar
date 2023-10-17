# Considerações sobre TPM para projetos de Aprendizado de Máquina

Neste documento, exploramos algumas das considerações de Gerenciamento de Programas para projetos de Aprendizado de Máquina (ML) e sugerimos recomendações para Gerentes de Programas Técnicos (TPM) trabalharem de forma eficaz com equipes de engenharia de Dados e Aprendizado de Máquina Aplicado.

## Determinar a necessidade de Aprendizado de Máquina no projeto

Em projetos de Inteligência Artificial (IA), o componente de ML geralmente faz parte de um problema de negócios geral e **NÃO** é o problema em si. Determine primeiro o problema de negócios geral e, em seguida, avalie se o ML pode ajudar a abordar uma parte do espaço do problema.
Algumas considerações para identificar o ajuste certo para o projeto:

- Envolva especialistas em experiência humana e utilize técnicas como [Design Thinking](https://www.microsoft.com/en-us/haxtoolkit/ai-guidelines/) e [Formulação de Problemas](ml-problem-formulation-envisioning.md) para **compreender as necessidades do cliente** e o comportamento humano primeiro. Identifique as partes interessadas corretas tanto da liderança de negócios quanto da liderança técnica e convide-os para essas oficinas. O resultado deve ser cenários de usuários finais e [personas](https://en.wikipedia.org/wiki/Persona_(user_experience)) para determinar as necessidades reais dos usuários.

- Concentre-se nos princípios de [Design de Sistema](https://learn.microsoft.com/en-us/azure/architecture/data-guide/big-data/ai-overview) para identificar os componentes arquiteturais, entidades, interfaces e restrições. Faça as perguntas certas desde cedo e explore alternativas de design com a equipe de engenharia.

- Pense cuidadosamente sobre os **custos do ML** e se estamos resolvendo um problema repetitivo em escala. Muitas vezes, os problemas dos clientes podem ser resolvidos com análise de dados, painéis de controle ou algoritmos baseados em regras como a primeira fase do projeto.

### Definir expectativas para alta ambiguidade nos componentes de ML

Projetos de ML podem ser afligidos por um fenômeno que podemos chamar de "**Morte pelo Desconhecido**". Ao contrário de projetos de engenharia de software, projetos focados em ML podem resultar em sucesso rápido no início (ou seja, diminuição repentina na taxa de erro), mas isso pode se estabilizar eventualmente. Algumas coisas a considerar:

- **Defina expectativas claras**: Identifique as métricas de desempenho e discuta uma taxa de previsão "suficientemente boa" que trará valor ao negócio. Uma taxa de "suficientemente boa" de 80% pode economizar custos de negócios e aumentar a produtividade, mas se passar de 80% para 95% exigir custos e esforços inimagináveis. Vale a pena? Pode ser um roadmap progressivo?

- Crie uma equipe menor e **realize uma análise de viabilidade** por meio de técnicas como [EDA](https://en.wikipedia.org/wiki/Exploratory_data_analysis) (Análise Exploratória de Dados). Um [estudo de viabilidade](ml-feasibility-study.md) é muito mais barato para avaliar a qualidade dos dados, as restrições do cliente e a viabilidade do modelo. Isso permite que um TPM compreenda melhor os casos de uso do cliente e o ambiente atual e pode atuar como um mecanismo de falha rápida. Observe que a viabilidade deve ser mais curta (em semanas) senão perderá o objetivo de economizar custos.

- Como em qualquer projeto, haverá novas necessidades (fontes de dados adicionais, restrições técnicas, contratação de rotuladores de dados, tempo dos usuários de negócios etc.). Incorpore técnicas [Ágeis](ml-project-management.md) para falhar rapidamente e minimizar custos e surpresas no cronograma.

Peço desculpas por não ter respeitado os marcadores Markdown em minha resposta anterior. Vou corrigir isso e fornecer a tradução novamente com a formatação correta:

### Notebooks != Produção de ML

Notebooks são uma ótima maneira de iniciar os esforços de Análise de Dados e Aprendizado de Máquina Aplicado, no entanto, para lançamentos em produção, devem ser consideradas restrições adicionais:

- Compreenda o [fluxo de gerenciamento de dados de ponta a ponta](https://learn.microsoft.com/en-us/azure/architecture/data-guide/big-data/ai-overview), como os dados serão disponibilizados (fluxos de ingestão), qual é a frequência, armazenamento e retenção de dados. Planeje histórias de usuário e spikes de design em torno desses fluxos para garantir o desenvolvimento de um pipeline de ML robusto.

- A equipe de engenharia deve seguir o mesmo rigor na construção de projetos de ML como em qualquer projeto de engenharia de software. Nós, da ISE (Indústria Solutions Engineering), desenvolvemos um bom conjunto de recursos a partir de nossas experiências em nosso [ISE Engineering Playbook](../index.md).

- Pense em como o modelo será implantado, por exemplo, existem restrições técnicas devido a um dispositivo de borda, ou restrições de rede que impedirão a atualização do modelo. Compreender o ambiente é fundamental, consulte a [Lista de Verificação de Produção de Modelos](ml-model-checklist.md) como referência para determinar escolhas de implantação do modelo.

- Projetos focados em ML não são uma solução de "um tiro" - eles precisam ser nutridos, evoluídos e aprimorados ao longo do tempo. Planeje um ciclo de melhoria contínua, as fases iniciais podem ser viabilidade e validação do modelo para obter a taxa de previsão "suficientemente boa", as fases posteriores podem ser escalonamento e melhoria dos modelos por meio de loops de feedback e conjuntos de dados atualizados.

### Dados Ruins na Entrada -> Modelo Ruim na Saída

A qualidade dos dados é um fator importante que afeta o desempenho do modelo e a implementação de produção, considere o seguinte:

- Realize uma oficina de [exploração de dados](ml-data-exploration.md) e **elabore um relatório sobre a qualidade dos dados** que inclua valores ausentes, duplicatas, dados não rotulados, dados expirados ou não válidos, dados incompletos (por exemplo, ter apenas representação masculina em um conjunto de dados de pessoas).

- **Identifique a confiabilidade da fonte de dados** para garantir que os dados provenham de uma fonte de produção. (por exemplo, as imagens são de uma câmera de produção ou industrial ou foram tiradas de um iPhone/Android.)

- **Identifique as restrições de aquisição de dados**: Determine como os dados estão sendo obtidos e as restrições relacionadas a isso. Alguns exemplos podem incluir restrições legais, contratuais, de privacidade, regulamentares e éticas. Essas restrições podem atrasar significativamente a implementação de produção se não forem capturadas nas fases iniciais do projeto.

- **Determine os volumes de dados**: Identifique se temos dados suficientes para amostrar o caso de uso de negócios necessário e como os dados serão aprimorados ao longo do tempo. A regra geral aqui é que os **dados devem ser suficientes para generalização** para evitar o overfitting.

### Planeje para Funções Únicas em Projetos de IA

Um projeto de ML tem várias etapas, e cada etapa pode exigir funções adicionais. Por exemplo, Pesquisa de Design e Designers para Experiência Humana, Engenheiro de Dados para Coleta de Dados, Engenharia de Recursos, um Rotulador de Dados para rotular dados estruturados, engenheiros para MLOps e implantação de modelo, e a lista pode continuar. Como TPM, leve em consideração ter esses recursos disponíveis no momento certo para evitar quaisquer riscos de cronograma.

### Engenharia de Recursos e Ajuste de Hiperparâmetros

A Engenharia de Recursos permite a transformação de dados para que eles se tornem utilizáveis por um algoritmo. Criar os recursos certos é uma arte e pode exigir experimentação, bem como experiência no domínio. Aloque tempo para que especialistas no domínio auxiliem na melhoria e identificação dos melhores recursos. Por exemplo, para um mecanismo de processamento de linguagem natural para extração de texto de documentos financeiros, podemos envolver pesquisadores financeiros e realizar um exercício de [avaliação de relevância](https://nlp.stanford.edu/IR-book/html/htmledition/information-retrieval-system-evaluation-1.html) e fornecer um ciclo de feedback para avaliar o desempenho do modelo.

### Considerações de IA Responsável

O viés no aprendizado de máquina pode ser o principal problema para um modelo não atingir suas necessidades pretendidas. Planeje incorporar [princípios de IA Responsável](responsible-ai.md) desde o primeiro dia para garantir a equidade, segurança, privacidade e transparência dos modelos. Por exemplo, para um algoritmo de reconhecimento de pessoas, se a fonte de dados estiver fornecendo apenas um tipo específico de pele, os cenários de produção podem não fornecer bons resultados.

### Fundamentos de Gerenciamento de Projetos

Essenciais para o papel de um TPM estão os fundamentos que incluem trazer clareza para a equipe, pensamento de design, direcionar a equipe para as decisões técnicas corretas, gerenciar riscos, gerenciar partes interessadas, gerenciamento de backlog e gerenciamento de projetos. **Esses são os superpoderes de um TPM**. Um TPM pode complementar a equipe de aprendizado de máquina garantindo que o problema e as necessidades do cliente sejam compreendidos, que um design de sistema holístico seja avaliado, que as expectativas das partes interessadas sejam gerenciadas e que os objetivos do cliente sejam alcançados.

Aqui estão algumas referências que podem ajudar:

- [O "T" em um TPM](https://www.linkedin.com/pulse/should-technical-program-manager-tpm-nikhil-sachdeva/)
- [O framework "TPM Don't M\*ck up"](https://www.linkedin.com/pulse/tpm-dont-mck-up-framework-nikhil-sachdeva/)
- [A mente de um TPM](https://www.linkedin.com/pulse/mind-technical-program-manager-nikhil-sachdeva/)
- [Jornada de Aprendizado de Máquina para um TPM](https://medium.com/data-science-at-microsoft/the-role-of-a-technical-program-manager-in-ai-projects-8f1ff41905b0)
