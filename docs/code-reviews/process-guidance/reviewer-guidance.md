# Orientações para o Revisor

Uma vez que partes das revisões podem ser automatizadas por meio de linters e similares, os revisores humanos podem se concentrar na correção arquitetural e funcional. Os revisores humanos devem se concentrar em:

- A correção da lógica de negócios incorporada no código.
- A correção de novos testes ou testes alterados.
- A "legibilidade" e manutenibilidade das decisões gerais de design refletidas no código.
- A lista de verificação de erros comuns mantida pela equipe para cada linguagem de programação.

As revisões de código devem seguir as orientações e listas de verificação abaixo para garantir revisões de código positivas e eficazes.

## Orientações gerais

### Compreenda o código que você está revisando

- Leia cada linha alterada.
- Se tivermos uma revisão do interessado, não é necessário executar o PR, a menos que isso ajude a entender o código.
- O AzDO ordena os arquivos para você, mas você deve ler o código em alguma sequência lógica para facilitar o entendimento.
- Se você não entender completamente uma alteração em um arquivo por falta de contexto, clique para visualizar o arquivo completo e leia o código circundante ou verifique as alterações e visualize-as no IDE.
- Peça ao autor para esclarecer.

### Leve seu tempo e mantenha o foco no escopo

Você não deve revisar o código apressadamente, mas também não deve demorar muito em uma única sessão. Se você tiver muitos pull requests (PRs) para revisar ou se a complexidade do código for alta, a recomendação é fazer pausas entre as revisões para se recuperar e se concentrar naqueles com os quais você tem mais experiência.

Lembre-se sempre de que o objetivo de uma revisão de código é verificar se os objetivos da tarefa correspondente foram alcançados. Se você tiver preocupações com o código relacionado ou adjacente que não está no escopo do PR, aborde essas preocupações como tarefas separadas (por exemplo, bugs ou dívidas técnicas). Não bloqueie o PR atual devido a problemas que estão fora do escopo.

## Fomente uma cultura de revisão de código positiva

As revisões de código desempenham um papel crítico na qualidade do produto e não devem representar um local para discussões longas ou, pior ainda, uma batalha de egos. O que importa é encontrar um erro, não quem o cometeu, não quem o encontrou, não quem o corrigiu. A única coisa que importa é ter o melhor produto possível.

## Seja considerado

- Seja positivo - incentive e aprecie as boas práticas.
- Prefira prefixar um "ponto de polimento" com "Ajuste:".
- Evite linguagem que aponte o dedo, como "você", e use preferencialmente "nós" ou "esta linha" - as revisões de código não são pessoais e a linguagem importa.
- Prefira fazer perguntas em vez de fazer afirmações. Pode haver uma boa razão para o autor fazer algo.
- Se você fizer um comentário direto, explique por que o código precisa ser alterado, de preferência com um exemplo.
- Falando em mudanças, você pode sugerir alterações a um PR usando o recurso de sugestão (disponível no [GitHub](https://docs.github.com/en/github/collaborating-with-issues-and-pull-requests/commenting-on-a-pull-request#adding-line-comments-to-a-pull-request) e no Azure DevOps) ou criando um PR para o branch do autor.
- Se alguns comentários de ida e volta não resolverem uma discordância, conversem rapidamente entre si (pessoalmente ou por telefone) ou crie uma discussão em grupo, o que pode levar a uma série de melhorias para os próximos PRs. Não se esqueça de atualizar o PR com o que vocês concordaram e por quê.

## Primeira análise de design

### Visão geral do Pull Request

- A descrição do PR faz sentido?
- Todas as alterações se encaixam logicamente neste PR ou existem alterações não relacionadas?
- Se necessário, as alterações feitas estão refletidas nas atualizações do README ou em outros documentos? Especialmente se as alterações afetarem como o usuário compila o código.

### Alterações visíveis pelo usuário

- Se o código envolver uma alteração visível pelo usuário, há um GIF/foto que explique a funcionalidade? Se não houver, pode ser crucial validar o PR para garantir que a alteração faça o que é esperado.
- Certifique-se de que as alterações de interface do usuário pareçam boas sem comportamentos inesperados.

### Design

- As interações das várias partes de código no PR fazem sentido?
- O código reconhece e incorpora arquiteturas e padrões de codificação?

## Análise de Qualidade de Código

### Complexidade

- As funções são muito complexas?
- O princípio da responsabilidade única é seguido? Uma função ou classe deve fazer apenas uma 'coisa'.
- Uma função deve ser dividida em várias funções?
- Se um método tem mais de 3 argumentos, ele é potencialmente excessivamente complexo.
- O código adiciona funcionalidade que não é necessária?
- O código pode ser facilmente compreendido pelos leitores de código?

### Nomeação/legibilidade

- O desenvolvedor escolheu bons nomes para funções, variáveis, etc.?

### Tratamento de Erros

- Os erros são tratados com elegância e de forma explícita quando necessário?

### Funcionalidade

- Há programação paralela neste PR que pode causar condições de corrida? Leia atentamente essa lógica.
- O código pode ser otimizado? Por exemplo: existem mais chamadas ao banco de dados do que o necessário?
- Como a funcionalidade se encaixa na imagem maior? Pode ter efeitos negativos no sistema como um todo?
- Existem falhas de segurança?
- O nome de uma variável revela alguma informação específica do cliente?
- PII (Informações Pessoais Identificáveis) e EUII (Informações Empresariais Identificáveis) são tratados corretamente? Estamos registrando informações PII?

### Estilo

- Existem comentários desnecessários? Se o código não for claro o suficiente para se explicar, então o código deve ser simplificado. Os comentários podem estar lá para explicar por que algum código existe.
- O código segue o guia de estilo/convenções que

 acordamos? Usamos formatação automática como black e prettier.

### Testes

- Os testes sempre devem ser incluídos no mesmo PR que o código em si ("Vou adicionar testes depois" não é aceitável).
- Certifique-se de que os testes sejam sensatos e que sejam feitas suposições válidas.
- Certifique-se de que os casos limite sejam tratados também.
- Os testes podem ser uma ótima fonte para entender as alterações. Pode ser uma estratégia olhar primeiro os testes para ajudar a entender melhor as alterações.
