# Experiência do Desenvolvedor (DevEx)

A experiência do desenvolvedor refere-se a quão fácil ou difícil é para um desenvolvedor realizar tarefas essenciais necessárias para implementar uma mudança. Uma experiência do desenvolvedor positiva significaria que essas tarefas são relativamente fáceis para a equipe (veja as medidas abaixo).

As tarefas essenciais são identificadas abaixo.

- Build - Verificar que as mudanças estão livres de erros de sintaxe e compilar.
- Teste - Verificar que todos os testes automatizados passam.
- Iniciar - Iniciar o sistema de ponta a ponta para simular a execução em um ambiente implantado.
- Depuração - Anexar o depurador à solução iniciada, definir pontos de interrupção, percorrer o código e inspecionar variáveis.

Se esforços forem investidos para tornar essas atividades o mais fáceis possível, **os retornos desse esforço aumentarão à medida que o projeto continuar e a equipe crescer**.

## Definindo o Termo "Ponta a Ponta"

Este documento faz várias referências à execução de uma solução de ponta a ponta (também conhecida como E2E). O termo "ponta a ponta" para os fins deste documento está limitado ao software de propriedade, desenvolvido e implantado pela equipe. Sistemas de propriedade de outras equipes ou fornecedores de terceiros não estão incluídos no escopo de E2E para os fins deste documento.

## Metas

- Maximizar o tempo que os engenheiros gastam escrevendo código que atende aos critérios de aceitação da história e aos critérios de "feito".
- Minimizar o tempo gasto na configuração manual e na configuração das ferramentas.
- Minimizar regressões e novos defeitos tornando o teste de ponta a ponta fácil.

## Impacto

A experiência do desenvolvedor pode ter um impacto significativo na eficiência da execução diária da equipe. Uma experiência positiva pode pagar dividendos ao longo da vida do projeto, especialmente quando novos desenvolvedores se juntam à equipe.

- Aumento da Velocidade - A equipe gasta menos tempo em atividades que não agregam valor, como configuração de ambiente de desenvolvimento local, espera em ambientes remotos para testes e retrabalho (correção de defeitos).
- Melhoria na Qualidade - Quando é fácil depurar e testar, os desenvolvedores farão mais disso. Isso se traduzirá em menos defeitos sendo introduzidos.
- Facilitação da Integração e Adoção - Quando as tarefas essenciais de desenvolvimento são automatizadas, há menos documentação a ser escrita e, consequentemente, menos leitura para começar!

**Mais importante ainda, o cliente continuará a colher esses benefícios muito tempo após o envolvimento inicial com o código.**

## Medidas

### Tempo para o Primeiro Resultado de Ponta a Ponta (também conhecido como Contrato F5)

Supondo um laptop/PC que nunca executou a solução, quanto tempo leva para configurar e executar todo o sistema de ponta a ponta e ver um resultado.

### Tempo para o Primeiro Commit

Quanto tempo leva para fazer uma mudança que pode ser verificada/testada localmente. Uma mudança verificada/testada localmente é aquela que passa nos casos de teste sem introduzir regressões ou mudanças quebradas.

## Participação

Fornecer uma experiência positiva do desenvolvedor é um esforço de equipe. No entanto, certos membros podem assumir a responsabilidade por diferentes áreas para ajudar a manter toda a equipe responsável.

### Líder de Desenvolvimento - Estabeleça o Padrão

Os exemplos a seguir mostram como o Líder de Desenvolvimento pode estabelecer o padrão para a experiência do desenvolvedor.

- Determina o ambiente de desenvolvimento (IDE sugerida, hospedagem, etc.).
- Determina o ambiente de controle de origem e o número de repositórios necessários.
- Dado o ambiente de desenvolvimento e a estrutura do repositório, estabelece as expectativas para a equipe em termos de etapas para executar as tarefas essenciais de desenvolvimento.
- Nomeia o Campeão DevEx.

A escolha do IDE NÃO pretende impor que todos os membros da equipe devem usar o mesmo IDE. No entanto, essa escolha direcionará onde o investimento em integração estreita será priorizado. Por exemplo, se o Visual Studio Code for a IDE **sugerida**, a equipe se concentrará em integrar as tarefas e as configurações de inicialização do VS Code em detrimento de integrações semelhantes para outros IDEs. Os membros da equipe ainda devem se sentir à vontade para usar o IDE de sua preferência, desde que isso não prejudique a equipe.

### Campeão DevEx - Identificar Melhorias Iterativas

O Campeão DevEx assume a responsabilidade de manter a equipe responsável por proporcionar uma experiência positiva do desenvolvedor. O seguinte esboça as responsabilidades do Campeão DevEx.

- Buscar ativamente oportunidades para melhorar a experiência do desenvolvedor da solução.
- Trabalhar com o Líder de Desenvolvimento para aprimorar de forma iterativa as expectativas da equipe em relação à experiência do desenvolvedor.

- Selecionar histórias de ação para aprimorar áreas de melhoria e priorizar em relação aos objetivos de entrega do projeto, envolvendo-se diretamente com o Proprietário do Produto e o Cliente.
- Atuar como especialista no assunto para o resto da equipe. Ajudar a equipe a determinar como implementar as expectativas do DevEx e identificar desvios.

### Membros da Equipe - Reforcem as Expectativas

Os membros da equipe também podem ajudar a manter uns aos outros responsáveis por proporcionar uma experiência positiva do desenvolvedor. Os exemplos a seguir mostram áreas em que os membros da equipe podem ajudar a identificar quando as expectativas do DevEx da equipe não estão sendo atendidas.

- Solicitações de Pull (Pull requests). Experimente as mudanças localmente para ver se elas estão aderindo às expectativas do DevEx da equipe.
- Revisões de Design. Procure propostas que possam afetar negativamente a experiência do DevEx da solução. Isso pode incluir:
  - Introdução de novas tecnologias cuja test

abilidade é limitada a etapas manuais em um ambiente implantado.
  - Adição de um novo repositório.

### Novos Membros da Equipe - Identifiquem Melhorias Iterativas

Os novos membros da equipe estão em posição única para identificar instâncias de [Sabedoria Coletiva](https://pt.wikipedia.org/wiki/Sabedoria_coletiva) não documentada. O seguinte esboça as responsabilidades dos novos membros da equipe no que diz respeito ao DevEx:

- Se encontrar documentação ausente, incompleta ou incorreta ao integrar-se, você deve registrar o problema como um novo defeito(s) e atribuí-lo ao proprietário do produto para triagem.
- Se não houver documentação de integração, anote as etapas que você seguiu em uma nova história do usuário. Atribua a nova história ao proprietário do produto para triagem.

## Orientação de Facilitação

A seguir, são apresentados exemplos de várias estratégias que podem ser adotadas para promover uma experiência positiva do desenvolvedor. Espera-se que cada equipe defina o que significa uma experiência de desenvolvimento positiva no contexto de seu projeto. Além disso, refine isso ao longo do tempo por meio de mecanismos de feedback, como retrospectivas de sprint e de projeto.

### Estabeleça Atalhos (Hotkeys)

Atribua atalhos para cada uma das tarefas essenciais.

| Tarefa               | Windows      |
|----------------------|--------------|
| Build                | CTRL+SHIFT+B |
| Teste                | CTRL+R,T     |
| Iniciar com Depuração | F5           |

### O Contrato F5

O Contrato F5 visa a capacidade de executar a solução de ponta a ponta com as seguintes etapas.

1. Clonar - git clone [URL do meu repositório aqui]
2. Configurar - defina quaisquer valores de configuração que precisem ser exclusivos para o indivíduo (ou seja, atualize um arquivo .env)
3. Pressione F5 - inicie a solução com depuração anexada.

A maioria dos IDEs possui algum tipo de executor de tarefas que pode ser usado para automatizar as etapas de compilação, execução e anexação. Tente aproveitar essas ferramentas para que as etapas possam ser executadas com o mínimo de etapas manuais possível.

### Campeão DevEx em Busca Ativa de Melhorias

O Campeão DevEx deve procurar ativamente áreas em que a equipe tem oportunidade de melhorar. Por exemplo, eles precisam implantar suas alterações em um ambiente fora de seu laptop antes de validar se o que fizeram funcionou. Em vez de depurar localmente, eles têm que fazer isso repetidamente para obter uma solução funcional? Isso leva vários minutos a cada iteração? Isso bloqueia outros desenvolvedores devido à contenção no ambiente?

As seguintes cerimônias podem ser usadas pelo Campeão DevEx para encontrar oportunidades potenciais:

- Retrospectivas. Estão sendo levantados feedbacks relacionados às tarefas essenciais sendo difíceis ou difíceis de controlar?
- Bloqueios de Standup. Indivíduos estão sendo bloqueados ou enfrentando dificuldades com as tarefas essenciais?

À medida que as oportunidades são identificadas, o Campeão DevEx pode traduzi-las em histórias acionáveis para o backlog do produto.

### Torne as Tarefas Multiplataforma

Para as tarefas essenciais padronizadas durante o compromisso, certifique-se de que diferentes plataformas sejam consideradas. Os membros da equipe podem ter sistemas operacionais diferentes, e garantir que as tarefas sejam multiplataforma proporcionará uma oportunidade adicional de melhorar a experiência.

- Consulte o [guia de tarefas multiplataforma](cross-platform-tasks.md) para obter orientações sobre como as tarefas podem ser configuradas para incluir diferentes plataformas.

### Crie um Guia de Integração

Ao dar as boas-vindas a novos membros da equipe ao compromisso, há muitas áreas para eles se adaptarem e para se atualizarem, incluindo base de código, padrões de codificação, acordos da equipe e cultura da equipe. Ao adotar uma prática forte de integração, como um guia de integração em um local centralizado que explique o escopo do projeto, processos, detalhes de configuração e software necessário, os novos membros podem ter todos os recursos necessários para serem eficientes, bem-sucedidos e membros valiosos da equipe desde o início.

Consulte o [modelo de guia de integração](onboarding-guide-template.md) para obter orientações sobre como um guia de integração pode se parecer.

### Padronize Tarefas Essenciais

Aplique uma estratégia comum em componentes de solução para realizar as tarefas essenciais

- Padronize a configuração dos componentes da solução
- Padronize a maneira como os testes são executados para cada componente
- Padronize a maneira como cada componente é iniciado e interrompido localmente
- Padronize como documentar as tarefas essenciais para cada componente

Essa padronização permitirá que a equipe automatize mais facilmente essas tarefas em todos os componentes no nível da solução. Consulte as Tarefas Essenciais em Nível de Solução abaixo.

### Tarefas Essenciais em Nível de Solução

Automatize a capacidade de executar cada tarefa essencial em todos os componentes da solução. Um exemplo seria mapear a ação de compilação no IDE para executar a tarefa de compilação para cada componente na solução. Mais importante ainda, configure a ação de inicialização do IDE para iniciar todos os componentes dentro da solução. Isso proporcionará eficiência significativa para a equipe de engenharia ao lidar com soluções de vários componentes.

Quando isso não é implementado, os engenheiros precisam repetir manualmente cada uma das tarefas essenciais para cada componente na solução. Nessa situação, o número de etapas necessárias para executar cada tarefa essencial é multiplicado pelo número de componentes no sistema.

[Etapas de configuração + Etapas de compilação + Etapas de inicialização/depuração + Etapas de interrupção + Etapas de execução de teste + Documentação de tudo isso] * [muitos componentes da solução] = MUITAS ETAPAS



VS.

[Etapas de configuração + Etapas de compilação + Etapas de inicialização/depuração + Etapas de interrupção + Etapas de execução de teste + Documentação de tudo isso] * [1 solução] = NÚMERO MÍNIMO DE ETAPAS

### Observabilidade

[A observabilidade](../observability/README.md) alivia desafios imprevistos para o desenvolvedor em um sistema distribuído complexo. Identifica gargalos de projeto mais rapidamente e com mais precisão, aprimorando o desempenho à medida que o desenvolvedor busca implantar alterações de código. A adição de observabilidade melhora a experiência ao identificar e resolver erros ou código quebrado. Isso resulta em menos falhas de produção atuais e futuras ou menos graves.

Existem muitas estratégias de observabilidade que um desenvolvedor pode usar ao lado das melhores práticas de engenharia. Esses recursos melhoram o DevEx, garantindo uma visão compartilhada do sistema complexo ao longo de todo o ciclo de vida. A observabilidade no código por meio de registro, tratamento de exceções e exposição de métricas de aplicativo relevantes, por exemplo, promove a visibilidade consistente do desempenho em tempo real. Os pilares da observabilidade, [registro](../observability/pillars/logging.md), [métricas](../observability/pillars/metrics.md) e [rastreamento](../observability/pillars/tracing.md), detalham quando habilitar cada um dos três tipos específicos de observabilidade.

### Minimize o Número de Repositórios

Dividir uma solução em vários repositórios pode afetar negativamente as medidas acima. Isso também pode afetar negativamente outras áreas, como Solicitações de Pull, Teste Automatizado, Integração Contínua e Entrega Contínua. Semelhante às instâncias do IDE, o impacto negativo é multiplicado pelo número de repositórios.

[Etapas de clonagem + Etapas de ramificação + Etapas de confirmação + Etapas de CI + Revisões e mesclagens de Solicitações de Pull] * [muitos repositórios de código-fonte] = MUITAS ETAPAS

VS.

[Etapas de clonagem + Etapas de ramificação + Etapas de confirmação + Etapas de CI + Revisões e mesclagens de Solicitações de Pull] * [1 repositório de código-fonte] = NÚMERO MÍNIMO DE ETAPAS

#### Solicitações de Pull Atômicas

Quando a solução está encapsulada dentro de um único repositório, ela também permite que as solicitações de pull representem uma alteração em várias camadas. Isso é especialmente útil quando uma alteração requer modificações em um contrato compartilhado entre vários componentes. Por exemplo, uma história exige que um ponto de extremidade da API seja alterado. Com essa estratégia, a API e o cliente da web podem ser atualizados com a mesma solicitação de pull. Isso evita que o branch principal seja quebrado temporariamente enquanto aguarda a mesclagem das solicitações de pull dependentes.

### Minimize as Dependências Remotas para o Desenvolvimento Local

Quanto menos dependências em componentes que não podem ser executados na máquina de um desenvolvedor, menos etapas são necessárias para começar. Portanto, menos dependências afetarão positivamente as medidas acima.

As seguintes estratégias podem ser usadas para reduzir essas dependências

#### Use um Emulador

Se disponível, os emuladores são implementações de tecnologias que normalmente só estão disponíveis em ambientes de nuvem. Um bom exemplo é o [emulador CosmosDB](https://learn.microsoft.com/pt-br/azure/cosmos-db/local-emulator).

#### Use DI + Alternância para Simular Dependências Remotas

Quando a solução depende de uma tecnologia que não pode ser executada na máquina de um desenvolvedor, a configuração e o teste dessa solução podem ser desafiadores. Uma estratégia que pode ser empregada é criar a capacidade de substituir essa dependência por uma que possa ser executada localmente.

Abstraia a camada que tem a dependência remota por trás de uma interface de propriedade da solução (não da dependência remota). Crie uma implementação dessa interface usando uma tecnologia que possa ser executada localmente. Crie uma fábrica que decide qual instância usar. Essa decisão pode ser baseada na configuração do ambiente (ou seja, na alternância). Em seguida, a classe original que depende da tecnologia remota deve depender da fábrica para fornecer qual instância usar.

Grande parte dessa estratégia pode ser simplificada com uma técnica adequada de injeção de dependência e/ou um framework.

Veja o exemplo abaixo que troca a implementação do Azure Service Bus por RabbitMQ, que pode ser executado localmente.

```typescript
interface IPublisher {
    send(message: string): void
}
class RabbitMQPublisher implements IPublisher {
    send(message: string) {
        //todo: enviar a mensagem via RabbitMQ
    }
}
class AzureServiceBusPublisher implements IPublisher {
    send(message: string) {
        //todo: enviar a mensagem via Azure Service Bus
    }
}
interface IPublisherFactory{
    create(): IPublisher
}
class PublisherFactory{
    create(): IPublisher {
        // use a variável de ambiente para determinar qual instância deve ser usada
        if(process.env.UseAsb){
            return new AzureServiceBusPublisher();
        }
        else{
            return new RabbitMqPublisher();
        }
    }
}
class MeuServico {
    //injete a fábrica
    constructor(private readonly publisherFactory: IPublisherFactory){
    }
    enviarMensagem(message: string): void{
        //use a fábrica para determinar qual inst

ância enviará a mensagem
        const publisher = this.publisherFactory.create();
        publisher.send(message);
    }
}
```

#### Alternância de Programação de Recursos

Durante o desenvolvimento local, uma estratégia que pode ser empregada é usar um arquivo de configuração, variável de ambiente ou algum outro mecanismo para direcionar a solução para usar uma versão de recursos que podem ser executados localmente. Isso é chamado de alternância de programação de recursos.

Para habilitar a alternância, crie uma interface que defina os serviços que dependem de recursos que podem não estar disponíveis localmente. Em seguida, crie uma implementação dessa interface que depende da configuração do ambiente (ou seja, alternância) para determinar como se comportar. Certifique-se de injetar a implementação da interface onde ela é necessária.

## Pontos de Atenção

### Qualidade da Experiência do Desenvolvedor

A experiência do desenvolvedor é uma métrica subjetiva, mas valiosa. Realize pesquisas de satisfação do desenvolvedor regularmente para entender como as mudanças afetam a equipe.

### Atenção às Documentações

Documentações atualizadas e precisas são fundamentais para uma boa experiência do desenvolvedor. Garanta que todas as documentações relacionadas às tarefas essenciais estejam sempre atualizadas e sejam de fácil acesso para a equipe.

### Cultura de Feedback

Promova uma cultura de feedback aberto na equipe, onde os membros possam compartilhar suas experiências e sugestões para melhorar a experiência do desenvolvedor. Incentive os desenvolvedores a relatar problemas de DevEx e implementar melhorias iterativas.

## Conclusão

Uma experiência positiva do desenvolvedor é fundamental para aumentar a eficiência da equipe de desenvolvimento e garantir a qualidade do software. Ao priorizar a automação, padronização e simplificação das tarefas essenciais, as equipes podem proporcionar uma experiência do desenvolvedor excepcional. Isso não só beneficia os membros da equipe atual, mas também contribui para a integração suave de novos membros e o sucesso contínuo do projeto.

Referências:

1. [Princípios de DevOps da Microsoft](https://learn.microsoft.com/pt-br/azure/devops/learn/devops-at-microsoft/devops-tech-principles)
2. [SRE - Site Reliability Engineering](https://sre.google/)
3. [12 Factor App](https://12factor.net/)
4. [Padrões e Práticas de Engenharia da Microsoft](https://learn.microsoft.com/pt-br/azure/devops/learn/devops-at-microsoft/devops-tech-principles)
