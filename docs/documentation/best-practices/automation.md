# Substituindo Documentação por Automação

Você pode documentar como configurar sua máquina de desenvolvimento com a versão correta do framework necessária para executar o código, quais extensões são úteis para desenvolver a aplicação com seu editor ou como configurar seu editor para iniciar e depurar a aplicação. Se for possível, uma solução melhor é fornecer meios para automatizar a instalação de ferramentas, o início da aplicação, etc.

Alguns exemplos são fornecidos abaixo:

## Contêineres de Desenvolvimento no Visual Studio Code

A extensão [Visual Studio Code Remote - Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) permite que você use um contêiner Docker como um ambiente de desenvolvimento completo. Isso permite que você abra qualquer pasta dentro (ou montada em) um contêiner e aproveite o conjunto completo de recursos do Visual Studio Code.

Informações adicionais: [Desenvolvendo Dentro de um Contêiner](https://code.visualstudio.com/docs/remote/containers).

## Configurações de Inicialização e Tarefas no Visual Studio Code

[Configurações de Inicialização](https://code.visualstudio.com/Docs/editor/debugging#_launch-configurations) permitem que você configure e salve detalhes de configuração de depuração.

[Tarefas](https://code.visualstudio.com/Docs/editor/tasks) podem ser configuradas para executar scripts e iniciar processos, de modo que muitas dessas ferramentas existentes podem ser usadas dentro do VS Code sem a necessidade de entrar em um terminal ou escrever novo código.
