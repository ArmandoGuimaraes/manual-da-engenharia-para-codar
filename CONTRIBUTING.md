# Contribuindo

Demonstrar Fundamentos de Engenharia é essencial para o que fazemos na Engenharia e é um dos principais valores que trazemos para nossos clientes, ajudando-os a evoluir enquanto colaboramos em seus cenários de negócios.

O objetivo deste repositório é fornecer orientações aos engenheiros de software e cientistas de dados da ISE (formando uma Equipe de Desenvolvimento) em relação aos fundamentos de engenharia. Descreve práticas recomendadas com base em aprendizados contínuos de projetos em diferentes áreas. Pode ser usado como uma ferramenta no início de um projeto que pode ser compartilhado com os clientes para ajudar a configurar o projeto para o sucesso. Este repositório *não* é destinado como um repositório de código, em vez disso, ele fornece orientações e links para repositórios de código de exemplo apropriados que representam bons exemplos.

Este projeto aceita contribuições e sugestões.

## O que contribuir

- Padrões e práticas que funcionaram bem em projetos, relacionados aos nossos fundamentos de engenharia.
- Conteúdo que pode ser visível ao público (evite informações confidenciais).
- Conteúdo que é geralmente aplicável (evite referências a processos internos ou informações muito específicas).
- Pequenos trechos de código ou links para repositórios de código aberto (evite grandes ativos de código).

Se você não tiver certeza se o seu conteúdo corresponderá a este manual, fique à vontade para entrar em contato com um dos [Campeões de Fundamentos de Engenharia](https://github.com/microsoft/code-with-engineering-playbook/blob/main/.github/CODEOWNERS) para discutir a contribuição.

## Orientação geral

- A qualidade é mais importante do que a quantidade.
- Escreva de uma forma que você gostaria que alguém explicasse algo para você. Use [`inglês simples`](http://www.plainenglish.co.uk/how-to-write-in-plain-english.html).
- Seja amigável, técnico, profissional e conciso.
- Comunique-se como engenheiros, não como marketing; queremos compartilhar o que aprendemos, não "vender" nossas ideias.
- Não recrie conteúdo introdutório, faça um link para ele.
- Adicione contexto em torno de padrões, receitas, etc., para que o leitor entenda quando e onde um padrão pode ser aplicável e onde não pode.
- Pense em como os leitores podem descobrir seu conteúdo, é fácil de encontrar?

> NOTA: Por favor, discuta com um dos [mantenedores](#mantenedores) antes de adicionar uma nova seção de nível superior. Tente encontrar um lugar adequado em uma seção existente, se possível.

Aceitamos a manutenção do repositório e contribuições que:

- Tornam o conteúdo mais legível.
- Tornam o conteúdo mais descobrível.
- Corrigem conteúdo que ficou desatualizado.

ou qualquer coisa que melhore a qualidade do manual.

## Como contribuir

A maioria das contribuições requer que você concorde com um Acordo de Licença de Contribuidor (CLA) declarando que você tem o direito de, e realmente concede, nos dar os direitos de usar sua contribuição. Para detalhes, visite <https://cla.microsoft.com>.

Quando você enviar um pull request, um bot de CLA determinará automaticamente se você precisa fornecer um CLA e decorará o PR de forma adequada (por exemplo, etiqueta, comentário). Basta seguir as instruções fornecidas pelo bot. Você só precisará fazer isso uma vez em todos os repositórios que usam nosso CLA.

Este projeto adotou o [Código de Conduta de Código Aberto da Microsoft](https://opensource.microsoft.com/codeofconduct/).
Para obter mais informações, consulte o [FAQ do Código de Conduta](https://opensource.microsoft.com/codeofconduct/faq/) ou entre em contato com [opencode@microsoft.com](mailto:opencode@microsoft.com) com quaisquer perguntas ou comentários adicionais.

### Permissões e Contribuições

Existem duas maneiras pelas quais você pode ajudar a atualizar o conteúdo:

- **Uma única vez:** \\
Se você não é um colaborador regular do projeto, mas gostaria de contribuir com algumas alterações, a melhor maneira de fazer isso é [enviando um PR](#enviando-um-pr).

- **Contribuições periódicas e regulares:** \\
Se você planeja atualizar o conteúdo de forma semi-regular ou regular, você pode ser adicionado ao grupo de Colaboradores do projeto. Entre em contato com um dos [mantenedores](#mantenedores) para ser adicionado ao grupo. Você ainda precisará enviar um PR contra a main para mesclar suas alterações.

> NOTA: Você precisa fazer parte da `organização Microsoft` no GitHub para ser adicionado ao grupo de colaboradores. Você também precisa fazer parte das `equipes Microsoft/code-with-engineering-playbook-collaborators` no github.

Como este não é considerado um repositório interno da Microsoft no GitHub, você não pode usar o alias github da Microsoft para enviar alterações. Você deve usar sua conta pessoal do github, que está vinculada à conta da Microsoft. Se você usar a conta github da Microsoft, verá este erro ao tentar criar um PR - **"Você não pode contribuir para repositórios fora de sua empresa `Microsoft EMU`"**.

### Enviando um PR

- Adicione suas alterações a um novo ramo `<alias do github>/<título>` ou bifurcando o repositório.
- Abra um PR com uma descrição clara das alterações

.
- Marque o PR com a área apropriada e vincule quaisquer problemas que o PR feche.
- Adicione revisores (você precisará de 2 revisores para mesclar) ao PR, por exemplo, os [mantenedores](#mantenedores) ou qualquer pessoa da [equipe de campeões de EF](https://github.com/microsoft/code-with-engineering-playbook/blob/main/.github/CODEOWNERS).

### Verificações de link

Antes que um pull request possa ser mesclado, todos os links nos documentos serão verificados para garantir que ainda estão ativos.

Ocasionalmente, você descobrirá que o verificador de links falhará em links que você pode acessar bem com um navegador. Também pode falhar em links que não estão nos documentos que você enviou alterações.

Quando isso ocorrer, faça o seguinte:

1. Verifique se o link está OK, se ele redirecionar, altere o caminho para ser o link final.
1. Se o link não estiver ok, corrija o link (mesmo que não esteja no seu documento) se você encontrar um bom link equivalente. Se você não encontrar um bom link equivalente, entre em contato com um dos [mantenedores](#mantenedores) para uma solução.
1. Execute novamente o trabalho ou peça para que o trabalho seja executado novamente (se você for um colaborador de primeira viagem).
1. Se o verificador de links ainda falhar e você confirmou que o link está ok, exclua o link da verificação no arquivo `.markdownlinkcheck.json` na raiz do repositório.

## Executando Localmente (*Remotamente*)

Para executar o site localmente, você deve ter [VSCode](https://code.visualstudio.com/), [Docker](https://www.docker.com/) e o [Remote Development Extension Pack](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack) instalados em sua máquina.

Após clonar este repositório, abra-o no VSCode. Abra o prompt de comando do VSCode usando `CTRL/CMD + SHIFT + P` e execute o comando `Remote-Container: Rebuild and Reopen in Container` (certifique-se de que o Docker também esteja rodando em sua máquina).

Depois de algum tempo, o VSCode deve reabrir este repositório no contêiner de desenvolvimento remoto e instalar as dependências necessárias.

Finalmente, inicie o site localmente usando o comando `mkdocs serve` a partir da raiz do repositório.

## Mantenedores

Para qualquer dúvida ou preocupação, entre em contato com [Tess Ferrandez](https://github.com/TessFerrandez), [Shiran Rubin](https://github.com/shiranr) ou [Federica Nocera](https://github.com/fnocera).

## Avisos Legais

A Microsoft e quaisquer colaboradores concedem a você uma licença para a documentação da Microsoft e outros conteúdos neste repositório sob a [Licença Pública Internacional Creative Commons Attribution 4.0](https://creativecommons.org/licenses/by/4.0/legalcode), consulte o arquivo [LICENSE](LICENSE), e concedem a você uma licença para qualquer código no repositório sob a [Licença MIT](https://opensource.org/licenses/MIT), consulte o arquivo [LICENSE-CODE](LICENSE-CODE).

Microsoft, Windows, Microsoft Azure e/ou outros produtos e serviços da Microsoft referenciados na documentação podem ser marcas comerciais ou marcas registradas da Microsoft nos Estados Unidos e/ou em outros países. As licenças para este projeto não concedem a você direitos de usar quaisquer nomes, logotipos ou marcas comerciais da Microsoft. As diretrizes gerais de marca registrada da Microsoft podem ser encontradas em <https://www.microsoft.com/en-us/legal/intellectualproperty/trademarks>.

Informações de privacidade podem ser encontradas em <https://privacy.microsoft.com/en-us/>.

A Microsoft e quaisquer colaboradores reservam todos os outros direitos, seja sob seus respectivos direitos autorais, patentes ou marcas comerciais, seja por implicação, preclusão ou de outra forma.
