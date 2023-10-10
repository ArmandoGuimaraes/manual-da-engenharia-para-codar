# Colaboração Virtual e Programação em Par

A programação em par é o método de trabalho padrão que a maioria das grandes organizações de engenharia usa para codificação "mãos no teclado". Dois desenvolvedores, trabalhando de forma síncrona, olhando para a mesma tela e tentando codificar e projetar juntos, o que frequentemente resulta em um código melhor e mais claro do que qualquer um poderia produzir individualmente.

A programação em par funciona bem nas circunstâncias corretas, mas perde parte de seu charme quando executada em um ambiente completamente virtual. O ambiente virtual ainda envolve dois desenvolvedores olhando para a mesma tela e discutindo seus projetos, mas frequentemente há questões logísticas a serem tratadas, incluindo latência, problemas de configuração de microfone, considerações de espaço de trabalho e pessoais, e muitos outros pequenos problemas individualmente triviais que pioram a experiência.

Os padrões de trabalho virtual são diferentes dos padrões presenciais aos quais estamos acostumados. A programação em par, em sua essência, é baseada nos seguintes princípios:

1. Gerar clareza através da comunicação
2. Produzir maior qualidade através da colaboração
3. Criar propriedade através da contribuição igual

A programação em par é uma forma de alcançar esses resultados. O Teste de Equipe Vermelha (RTT, Red Team Testing) é um método de programação alternativo que usa os mesmos princípios, mas com algumas das vantagens que os métodos de trabalho virtual oferecem.

## Teste de Equipe Vermelha (Red Team Testing - RTT)

O Teste de Equipe Vermelha toma seu nome do paradigma de "Equipe Vermelha" e "Equipe Azul" de testes de penetração e é uma forma colaborativa e paralela de trabalhar virtualmente. No RTT, dois desenvolvedores decidem conjuntamente sobre a interface, arquitetura e design do programa, e então se separam para a fase de implementação. Um desenvolvedor escreve testes usando a interface pública, tentando realizar testes de casos extremos, validação de entrada e outros testes de estresse na interface. O segundo desenvolvedor está simultaneamente escrevendo a implementação que eventualmente será testada.

O RTT tem a mesma filosofia que qualquer outro ciclo de vida de Desenvolvimento Orientado a Testes: toda implementação é separada da interface, e a interface pode ser testada sem conhecimento da implementação.

![diagrama-ptt](imagens/PTTdiagram.PNG)

## Etapas

1. Fase de Design: Ambos os desenvolvedores projetam a interface juntos. Isso inclui:
    * Assinaturas e nomes de métodos
    * Escrever documentação ou docstrings para o que os métodos pretendem fazer.
    * Decisões de arquitetura que influenciariam os testes (padrões de fábrica, etc.)

2. Fase de Implementação: Os desenvolvedores se separam e paralelizam o trabalho, continuando a se comunicar.
    * O Desenvolvedor A projetará a implementação dos métodos, aderindo ao design previamente decidido.
    * O Desenvolvedor B escreverá testes simultaneamente para as mesmas assinaturas de método, sem conhecer os detalhes da implementação.

3. Fase de Integração e Teste: Ambos os desenvolvedores fazem o commit de seu código e executam os testes.
    * Cenário utópico: Todos os testes são executados e passam corretamente.
    * Cenário realista: Os testes falharam ou quebraram devido a falhas nos testes. Isso leva a um esclarecimento adicional do design e uma discussão sobre por que os testes falharam.

4. Os desenvolvedores repetirão as três fases até que o código esteja funcional e testado.

## Quando seguir a estratégia RTT

O RTT funciona bem em circunstâncias específicas. Se a colaboração precisa acontecer virtualmente, e toda a comunicação é virtual, o RTT reduz a necessidade de comunicação constante enquanto mantém os benefícios de uma sessão de design conjunta. Isso considera o elemento humano: a comunicação virtual é mais exaustiva do que a comunicação presencial.

O RTT também funciona bem quando há consenso completo, ou nenhum consenso, sobre qual propósito o código serve. Como criar o design em conjunto e concordar em implementar e testar contra ele fazem parte do método RTT, o RTT cria clareza forçada através da iteração e comunicação.

## Benefícios

O RTT tem muitos dos mesmos benefícios que a Programação em Par e o Desenvolvimento Orientado a Testes, mas tenta atualizá-los para um ambiente virtual.

* A implementação e os testes de código podem ser feitos em paralelo, a grandes distâncias ou em fusos horários diferentes, o que reduz o tempo total necessário para terminar de escrever o código.

* O RTT mantém o paradigma de programação em par, enquanto reduz a necessidade de comunicação constante por vídeo ou entre desenvolvedores.

* O RTT permite um foco detalhado em design e alinhamento de engenharia antes de implementar qualquer código, levando a interfaces mais limpas e simples.

* O RTT incentiva que os testes sejam priorizados ao lado da implementação, em vez de seguir ou ser influenciado pela implementação do código.

* A documentação é inerentemente uma parte do RTT, já que tanto o implementador quanto o testador precisam de documentação correta e atualizada na fase de implementação.

## O que você precisa para o RTT funcionar bem

* A demanda por comunicação constante e bom trabalho em equipe pode representar um desafio; atualizações diárias entre os membros da equipe são essenciais para man
