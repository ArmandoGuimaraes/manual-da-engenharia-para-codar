# Engenharia de Interface do Usuário e Experiência do Usuário

Também conhecida como _UI/UX_, _Desenvolvimento Front-End_ ou _Desenvolvimento Web_, a engenharia de interface do usuário e experiência do usuário é um tópico amplo e abrange muitos aspectos diferentes do desenvolvimento moderno de aplicativos. Quando é necessária uma interface do usuário, a ISE (Engenharia de Software de Interface do Usuário) desenvolve principalmente uma **aplicação web**. Aplicativos web podem ser construídos de várias maneiras com diversas ferramentas diferentes.

## Objetivo

O objetivo da seção **Interface do Usuário** é fornecer orientações sobre o desenvolvimento de aplicações web. Todos devem começar lendo o [Guia Geral](#guia-geral) para uma introdução rápida aos quatro principais aspectos de cada projeto de aplicação web. A partir daí, os leitores são incentivados a aprofundar cada tópico ou começar a revisar as orientações técnicas que se aplicam ao seu projeto. Todos os projetos de UI/UX devem começar com um documento de design detalhado. Revise a seção [Processo de Design](#processo-de-design) para obter mais detalhes e um modelo para começar.

Lembre-se de que, como todo software, não existe uma "maneira certa" de construir uma aplicação de interface do usuário. Aproveite e confie na experiência e expertise de sua equipe ou do seu cliente para obter a melhor experiência de desenvolvimento.

## Guia Geral

O estado da engenharia de plataformas web está em constante evolução. Não existe uma solução única que sirva para todos. Para que qualquer equipe tenha sucesso na construção de uma UI, é necessário entender os aspectos de alto nível de todos os projetos de UI.

1. [**Acessibilidade**](../accessibility/README.md) - garantir que sua aplicação seja utilizável e apreciada por quantas pessoas forem possíveis é o cerne da acessibilidade e do design inclusivo.
1. [**Usabilidade**](./usability.md) - quão fácil deve ser para qualquer usuário utilizar a aplicação? Eles precisam de treinamento especial ou de um documento para entender como usá-la, ou será intuitivo?
1. [**Manutenibilidade**](./maintainability.md) - a aplicação é apenas uma prova de conceito para mostrar uma ideia para trabalhos futuros, ou será um MVP (Produto Mínimo Viável) e servirá como ponto de partida para uma aplicação maior e pronta para produção? Às vezes, você não precisa do React ou de qualquer outro framework. Às vezes, você precisa do React, mas não de todos os recursos do create-react-app. Compreender os requisitos de manutenção do projeto pode simplificar significativamente as necessidades de ferramentas de um projeto e permitir iterações sem dores de cabeça.
1. [**Estabilidade**](./stability.md) - qual é o custo de adicionar uma dependência? Ela está ativamente estável/atualizada/manutenida? Se não, você pode arcar com a dívida técnica (às vezes a resposta pode ser sim!)? Você poderia chegar a 90% do caminho sem adicionar outra dependência?

Mais informações estão disponíveis para cada seção de orientação geral nas páginas correspondentes.

## Processo de Design

Todas as aplicações de interface do usuário começam com o processo de design. A verdadeira definição de "o processo de design" está em constante mudança e é altamente baseada em opiniões. Esta seção tem como objetivo oferecer uma visão geral geral de um processo de design que _qualquer_ equipe de engenharia pode conduzir ao iniciar um projeto de aplicação de interface do usuário.

> Ao se comprometer com um projeto de UI/UX, certifique-se de não prometer demais em relação aos requisitos da aplicação web. Entregar uma aplicação pronta para produção envolve um grande número de complexidades de engenharia, resultando em um prazo muito longo. Comece sempre com um prova de conceito ou mínimo produto viável primeiro. Esses projetos podem ser facilmente concluídos em um prazo de alguns meses (e às vezes até menos).

O primeiro passo no processo de design é entender o problema em questão e delinear o que a solução deve alcançar. Comumente referido como _Resultados Desejados_, a saída desta primeira etapa deve ser uma lista generalizada de resultados que a solução alcançará. Considere o seguinte exemplo:

Uma biblioteca pública possui um conjunto de dados contendo informações sobre sua coleção. Os dados armazenam texto, imagens e o status de um livro (emprestado, disponível, reservado). O bibliotecário da biblioteca deseja compartilhar esses dados com seus usuários.

1. Como bibliotecário, quero notificar os usuários antes que eles recebam penalidades por atraso em livros
1. Como bibliotecário, quero notificar os usuários quando um livro que eles reservaram estiver disponível

Com os resultados desejados em mente, o próximo passo no processo de design é definir as personas do usuário. Independentemente da solução para um determinado problema, entender as necessidades do usuário leva a uma melhor compreensão do desenvolvimento de recursos e escolhas tecnológicas. As personas são escritas em forma de parágrafos que descrevem diferentes tipos de usuários. Considerando o exemplo anterior, as várias personas de usuário podem ser:

1. Uma pessoa sem deficiências, mas não familiarizada com interfaces de software
1. Uma pessoa sem deficiências e familiarizada com interfaces de software
1. Uma pessoa com deficiências e não familiarizada com interfaces de software (com ou sem o uso de ferramentas de acessibilidade)
1. Uma pessoa com deficiências, mas familiarizada com interfaces de software por meio do uso de ferramentas de acessibilidade

Após definir essas personas, fica claro que qualquer que seja a solução, ela requer muito trabalho de design de acessibilidade e experiência do usuário. Às vezes, as personas podem ser mais simples do que isso, mas **sempre inclua usuários com deficiências**. Mesmo quando um conjunto de usuários é predefinido como um grupo de pessoas sem deficiências, não há garantia de que esse conjunto de usuários permanecerá dessa forma.

Após definir os _resultados desejados_ e as _personas_, o próximo passo no processo de design é começar a conduzir [Estudos de Troca](./../design/design-reviews/trade-studies/README.md) para soluções

 potenciais. O primeiro estudo de troca deve ser de alto nível e orientado para a solução. Ele utilizará os resultados das etapas anteriores e proporá múltiplas soluções para alcançar os resultados desejados com as personas listadas em mente. Continuando com o exemplo da biblioteca, este primeiro estudo de troca pode comparar várias soluções de aplicativos, como emails ou mensagens de texto automatizadas, um feed RSS ou uma aplicação de interface de usuário. Existem prós e contras para cada solução, tanto do ponto de vista da experiência do usuário quanto da experiência do desenvolvedor, mas nesta fase é importante focar nos usuários. Após chegar à melhor solução, o próximo estudo de troca pode explorar diferentes métodos de implementação. É nesses estudos subsequentes que a experiência do desenvolvedor se torna mais importante.

A vantagem de construir aplicações de software é que existem infinitas maneiras de construir algo. Uma equipe pode usar as ferramentas mais modernas e brilhantes, ou pode utilizar aquelas que já foram testadas e comprovadas. É por isso que focar completamente no usuário até que uma solução seja definida é melhor do que se preocupar com as escolhas tecnológicas. Dentro da ISE, muitas vezes recorremos a ferramentas como o framework [React](https://reactjs.org/). O React é uma ótima ferramenta quando usado por uma equipe experiente. Caso contrário, pode criar mais obstáculos do que vale a pena. Lembre-se de que, mesmo que _você_ se sinta capaz com o React, o resto da sua equipe e a equipe de desenvolvimento do seu cliente também precisam se sentir assim. Algumas outras ótimas opções a serem consideradas ao construir uma prova de conceito ou mínimo produto viável são:

1. HTML/CSS/JavaScript
   - Volte ao básico! Comece com um único **index.html**, inclua um popular framework CSS como o [Bootstrap](https://getbootstrap.com/) usando o link CDN deles e comece a criar protótipos!
   - Raramente será necessário dar suporte a navegadores antigos; portanto, você pode contar com os recursos modernos da linguagem JavaScript! Não há necessidade de ferramentas de construção ou mesmo TypeScript (você sabia que pode [verificar o tipo do JavaScript](https://www.typescriptlang.org/docs/handbook/intro-to-js-ts.html)).
1. Frameworks de Componentes Web
   - Os Componentes Web agora são padronizados em todos os navegadores modernos
   - A Microsoft possui seu próprio framework estável e ativamente mantido, o [Fast](https://www.fast.design/)

Para obter mais informações sobre a escolha da ferramenta de implementação certa, leia o documento [Tecnologias Recomendadas](./recommended-technologies.md).

Continue lendo a seção de [Estudo de Troca](./../design/design-reviews/trade-studies/README.md) deste site para obter mais informações sobre como concluir esta etapa no processo de design.

Após iterar por vários documentos de estudos de troca, este processo de design pode ser considerado concluído! Com uma solução acordada e uma implementação em mente, agora é hora de iniciar o desenvolvimento. Uma continuação natural do processo de design é envolver os usuários (ou partes interessadas) o mais cedo possível. Procure constantemente feedback de design e usabilidade e utilize-o para melhorar a aplicação à medida que ela é desenvolvida.

### Exemplo

> Em breve!
