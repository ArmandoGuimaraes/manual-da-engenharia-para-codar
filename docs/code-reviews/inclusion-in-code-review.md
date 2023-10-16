# Inclusão na Revisão de Código

A seguir estão alguns pontos que enfatizam por que a inclusividade nas revisões de código é importante:

* Revisões de código são uma parte importante do nosso trabalho como profissionais de software.
* Na ISE, trabalhamos com equipes interculturais de todo o mundo.
* Como nos comunicamos afeta o moral da equipe.
* Revisões de código inclusivas acolhem novos desenvolvedores e os tornam confortáveis com a equipe.
* Ataques rudes ou pessoais durante revisões de código alienam - as pessoas podem, sem saber, fazer comentários rudes ao revisar pull requests (PRs).

## Tipos e Exemplos de Comportamento Não Inclusivo na Revisão de Código

* Atribuições de revisão injustas.
  * Exemplo: Atribuir a maioria das revisões a poucas pessoas e desconsiderar alguns membros da equipe.
* Interações interpessoais negativas.
  * Exemplo: Longas discussões sobre tópicos subjetivos, como estilo de código.
* Tomada de decisão tendenciosa.
  * Exemplo: Comentários sobre o desenvolvedor e não sobre o código. Supor que o código do desenvolvedor X sempre será bom e, portanto, não revisá-lo adequadamente e vice-versa.

## Exemplos de Revisões de Código Inclusivas

* Qualquer pessoa da equipe deve ser designada para revisar PRs.
* O revisor deve ser claro sobre o que é uma opinião, preferência pessoal, melhor prática ou um fato. Discussões sobre preferências pessoais e opiniões são na maioria das vezes evitáveis.
* Usar linguagem e tom inclusivos nos comentários da revisão de código. Por exemplo, ser sugestivo em vez de prescritivo nos comentários da revisão é uma boa maneira de passar o ponto de vista.
* É uma boa prática para o autor de um PR agradecer ao revisor pela revisão, quando eles contribuíram para melhorar o código ou você aprendeu algo novo.
* Usar o método sanduíche para recomendar uma mudança de código para um novo desenvolvedor ou um novo cliente: coloque a sugestão entre dois elogios. Por exemplo: "Ótimo trabalho até agora, mas eu recomendaria algumas mudanças aqui. A propósito, adorei o uso de XYZ aqui, bom trabalho!"

## Diretrizes para o Autor

* O objetivo é escrever um código que seja fácil de ler, revisar e manter.
* É importante garantir que quem estiver olhando para o código, seja o revisor ou um futuro engenheiro, possa entender as motivações e como seu código atinge seus objetivos.
* Pedir proativamente ajuda ou feedback direcionado.
* Responder claramente às perguntas feitas pelos revisores.
* Evitar commits grandes, enviando mudanças incrementais. Commits grandes e que contêm mudanças em vários arquivos levarão a uma revisão injusta do código. O comportamento tendencioso dos revisores pode surgir ao revisar tais PRs. Por exemplo, um commit grande de um desenvolvedor sênior pode ser aprovado sem uma revisão completa, enquanto um commit grande de um desenvolvedor júnior pode nunca ser revisado e aprovado.

## Diretrizes para o Revisor

* Presuma intenções positivas do autor.
* Escreva comentários claros e elaborados.
* Identifique subjetividade, escolha de codificação e melhores práticas. É bom discutir estilo de codificação e escolhas de codificação subjetivas em algum outro fórum e não no PR. Um PR não deve se tornar um terreno para discutir escolhas de codificação subjetivas e ter longas discussões sobre isso.
* Se você não entender o código adequadamente, evite comentar, por exemplo, "Este código é incompreensível". É melhor ter uma ligação com o autor e obter um entendimento básico do trabalho deles.
* Seja sugestivo e não prescritivo. Um revisor deve sugerir mudanças e não prescrever mudanças, deixe o autor decidir se realmente deseja aceitar as mudanças propostas.

## Cultura e Revisões de Código

Nós, na ISE, podemos nos deparar com situações em que as revisões de código não são ideais e frequentemente observamos comportamentos de revisão de código não inclusivos. É importante estar ciente do fato de que a cultura e o estilo de comunicação de uma geografia específica também influenciam como as pessoas interagem em pull requests.
Nesses casos, presumir a intenção positiva do autor e do revisor é um bom ponto de partida para começar a analisar a qualidade das revisões de código.

## Lidando com o Fenômeno do Impostor

O fenômeno do impostor é um padrão psicológico em que um indivíduo duvida de suas habilidades, talentos ou realizações e tem um medo interno persistente de ser exposto como uma "fraude" - [Wikipedia](https://en.wikipedia.org/wiki/Impostor_phenomenon).

Alguém que experimenta o fenômeno do impostor pode achar particularmente estressante enviar código para revisão. É importante perceber que todos podem ter contribuições significativas e não deixar que as fraquezas percebidas impeçam as contribuições.

Algumas dicas para superar o fenômeno do impostor para autores:

* Revise as diretrizes destacadas acima e certifique-se de que sua mudança de código esteja em conformidade com elas.
* Peça ajuda a um colega - programe em par com um colega experiente de quem você possa aprender.

Algumas dicas para superar o fenômeno do impostor para revisores:

* Qualquer pessoa pode ter percepções valiosas.
* Um novo par de olhos é sempre bem-vindo.
* Estude a revisão até que você a tenha entendido claramente, verifique os casos extremos e procure maneiras de melhorá-la.
* Se algo não estiver claro, uma pergunta específica simples deve ser feita.
* Se você aprendeu algo, sempre pode elogiar o autor.
* Se possível, faça parceria com alguém para revisar o código, para que você possa estabelecer uma conexão pessoal e ter uma discussão mais profunda sobre o código.

## Ferramentas

A seguir estão algumas ferramentas que podem ajudar a estabelecer uma cultura de revisão de código inclusiva dentro de nossas equipes.

* [Anonymous GitHub](https://github.com/tdurieux/anonymous_github)
* [Blind Code Reviews](https://github.com/zombie/blind-reviews)
* [Gitmask](https://www.gitmask.com)
* [inclusivelint](https://github.com/inclusivelint)
