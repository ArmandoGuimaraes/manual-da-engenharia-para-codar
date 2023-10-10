# Programação em Par Sem Esforço com GitHub Codespaces e VSCode

A programação em par costumava ser uma técnica de desenvolvimento de software na qual dois programadores trabalham juntos em um único computador, compartilhando um teclado e um mouse, para projetar, codificar, testar e depurar software conjuntamente. É um dos padrões explorados na seção [por que colaborar?](./why-collaboration.md) deste playbook. No entanto, com equipes que trabalham principalmente de forma remota, compartilhar um computador físico tornou-se um desafio, mas abriu a porta para uma abordagem mais eficiente da programação em par.

Por meio da utilização eficaz de uma variedade de ferramentas e técnicas, implementamos com sucesso tanto as metodologias de programação em par quanto em enxame. Como tal, estamos ansiosos para compartilhar alguns dos valiosos insights e conhecimentos adquiridos com essa experiência.

## Como tornar a programação em par uma experiência indolor?

### Sessões de Trabalho

Para aprimorar as capacidades de programação em par, você pode criar sessões de trabalho regulares que estão abertas a todos os membros da equipe. Isso facilita uma colaboração suave e eficiente, pois todos podem simplesmente participar e trabalhar juntos antes de se dividirem em grupos menores. Essa abordagem se mostrou particularmente benéfica para novos membros da equipe que, de outra forma, poderiam se sentir sobrecarregados por uma grande base de código. Ela emula o conceito do "[bebedouro humilde](https://www.inverse.com/innovation/how-companies-have-optimized-the-humble-office-water-cooler)", que promove um senso de conexão entre os membros da equipe por meio de seu trabalho compartilhado.

Além disso, agendar essas sessões de trabalho com antecedência garante colaboração intencional e fornece clareza sobre as responsabilidades da história do usuário. Para esse fim, atribua uma única pessoa a cada história do usuário para garantir propriedade clara e eliminar ambiguidades. Ao fazer isso, isso poderia eliminar o problema comum de engenheiros hesitarem em modificar o código fora de suas tarefas atribuídas devido ao sentimento de falta de propriedade. Essas sessões de trabalho são fundamentais para promover uma dinâmica de equipe coesa, permitindo o compartilhamento eficaz de conhecimento e a resolução coletiva de problemas.

### GitHub Codespaces

[GitHub Codespaces](https://code.visualstudio.com/docs/remote/codespaces) é um componente vital em um ambiente de desenvolvimento eficiente, particularmente no contexto da programação em par. Priorize a configuração de um Codespace como a etapa inicial do projeto, precedendo tarefas como compilação do projeto na máquina local ou instalação de plugins do VSCode. Para isso, certifique-se de atualizar a documentação do Codespace antes de incorporar quaisquer instruções de início rápido para ambientes locais. Além disso, demonstre consistentemente demos no ambiente de codespaces para garantir sua integração proeminente em nosso fluxo de trabalho.

Com sua infraestrutura baseada em nuvem, o GitHub Codespaces apresenta uma abordagem altamente eficiente e simplificada para codificação colaborativa em tempo real. Como resultado, novos membros da equipe podem acessar facilmente o projeto GitHub e começar a codificar em segundos, sem necessidade de instalação em suas máquinas locais. Esta solução integrada e contínua para programação em par oferece um fluxo de trabalho simplificado, permitindo que você direcione sua atenção para a produção de código exemplar, livre das distrações de processos de configuração complicados.

### VSCode Live Share

[VSCode Live Share](https://code.visualstudio.com/learn/collaboration/live-share) é especificamente projetado para programação em par e permite que você trabalhe na mesma base de código, em tempo real, com os membros da sua equipe. O árduo processo de configurar configurações complexas, lidar com configurações confusas, forçar os olhos para trabalhar em telas pequenas ou trocar fisicamente de teclados não é um problema com o LiveShare. Esta solução inovadora permite o compartilhamento contínuo do seu ambiente de desenvolvimento com os membros da sua equipe, facilitando experiências suaves de codificação colaborativa.

Totalmente integrado ao Visual Studio Code e ao Visual Studio, o LiveShare oferece o benefício adicional de compartilhamento de terminal, colaboração em sessão de depuração e controle da máquina host. Quando combinado com o GitHub Codespaces, ele apresenta um conjunto de ferramentas potente para programação em par eficaz.

> Dica: Compartilhe extensões do VSCode (incluindo Live Share) usando um [devcontainer.json](https://code.visualstudio.com/docs/devcontainers/create-dev-container) base. Isso garante que todos os membros da equipe tenham disponível o mesmo conjunto de extensões e lhes permita focar na resolução das necessidades de negócios desde o primeiro dia.

## Recursos

* [GitHub Codespaces](https://code.visualstudio.com/docs/remote/codespaces).
* [VSCode Live Share](https://code.visualstudio.com/learn/collaboration/live-share).
* [Criar um Dev Container](https://code.visualstudio.com/docs/devcontainers/create-dev-container).
* [Como as empresas otimizaram o bebedouro humilde do escritório](https://www.inverse.com/innovation/how-companies-have-optimized-the-humble-office-water-cooler).
