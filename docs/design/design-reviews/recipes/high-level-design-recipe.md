# Receita de Design de Alto Nível / Plano de Jogo

## Por que isso é valioso?

O design em nível macroscópico mostra as interações entre sistemas e serviços que serão usados para realizar o projeto. Ele tem o objetivo de garantir que haja um entendimento em alto nível do plano do que será construído, quais componentes prontos serão usados e quais componentes externos precisarão interagir com o entregável.

## Coisas a ter em mente

* Assim como em todos os outros aspectos do projeto, as revisões de design devem proporcionar um ambiente amigável e seguro para que qualquer membro da equipe se sinta à vontade para propor um design para revisão e possa aproveitar a oportunidade para crescer e aprender com o feedback construtivo / não julgamental de colegas e especialistas no assunto (consulte [Acordos da Equipe](../../../agile-development/advanced-topics/team-agreements/README.md)).
* Tente ilustrar diferentes personas envolvidas nos casos de uso e como/quais caixas são seus pontos de entrada.
* Prefira imagens a parágrafos. Os diagramas não se destinam a gerar código, portanto, eles devem ser bastante abstratos.
  * Os artefatos devem indicar a direção das chamadas (são de saída, entrada ou bidirecionais?) e destacar os limites do sistema, onde as portas podem precisar ser abertas ou pode ser necessário trabalho de infraestrutura adicional para permitir que as chamadas sejam feitas.
  * Diagramas de sequência são úteis para mostrar o fluxo de chamadas entre componentes + sistemas.
  * Diagramas de caixa genéricos que representam o fluxo de dados ou a origem/destino das chamadas são úteis. No entanto, o título deve definir claramente o que as setas indicam. Na maioria dos casos, um diagrama mostrará o fluxo de dados ou as direções das chamadas, mas não ambos.
  * Visualize os aspectos contrastantes do sistema/diagrama para facilitar a comunicação. Por exemplo, tecnologias diferentes empregadas, componentes modificados vs. intocados ou componentes de nuvem local vs. internet. Cores, caixas de agrupamento e iconografia podem ser usadas para diferenciação.
  * Prefira facilidade de entendimento para comunicar ideias em vez de estrita correção UML.
* As revisões de design devem ser leves e não devem parecer uma sobrecarga adicional no processo.

## Exemplos

![Diagrama de Sequência](images/high-level-sequence-diagram.png)

![Diagrama de Fluxo de Chamadas](images/high-level-box-diagram.png)

![Diagrama de Sistema](images/high-level-system-diagram.png)
