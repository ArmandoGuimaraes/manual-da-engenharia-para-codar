# Tarefas de Plataforma Cruzada

Existem várias opções para aliviar problemas de compatibilidade entre plataformas cruzadas.

- Execução de tarefas em um contêiner
- Uso do sistema de tarefas no VS Code, que fornece opções para permitir que comandos sejam executados específicos para um sistema operacional.

## Docker ou Baseado em Contêiner

Usar contêineres como máquinas de desenvolvimento permite que os desenvolvedores comecem com uma configuração mínima e abstrai o ambiente de desenvolvimento do sistema operacional host, fazendo com que ele seja executado em um contêiner. DevContainers também podem ajudar a padronizar a experiência do desenvolvedor local em toda a equipe.

Aqui estão alguns bons recursos para começar a executar tarefas em DevContainers:

- [Desenvolvendo dentro de um contêiner](https://code.visualstudio.com/docs/remote/containers).
- [Tutorial sobre Desenvolvimento em Contêineres](https://code.visualstudio.com/docs/remote/containers-tutorial)
- Para projetos de exemplo e modelos de contêineres de desenvolvimento, consulte [Receitas de Contêineres de Desenvolvimento do VS Code](https://github.com/microsoft/vscode-dev-containers)
- [Biblioteca de Contêineres de Desenvolvimento](devcontainers.md)

## Tarefas no VS Code

### Executando o Node.js

O exemplo abaixo oferece insights sobre como executar o executável Node.js como um comando com o arquivo tasks.json e como ele pode ser tratado de maneira diferente no Windows e no Linux.

```json
{
  "label": "Executar Node",
  "type": "process",
  "windows": {
    "command": "C:\\Program Files\\nodejs\\node.exe"
  },
  "linux": {
    "command": "/usr/bin/node"
  }
}
```

Neste exemplo, para executar o Node.js, existe um comando específico para o Windows e um comando específico para o Linux. Isso permite propriedades específicas da plataforma. Quando essas são definidas, elas serão usadas em vez das propriedades padrão quando o comando for executado no sistema operacional Windows ou no Linux.

### Tarefas Personalizadas

Nem todos os scripts ou tarefas podem ser detectados automaticamente no espaço de trabalho. Às vezes, pode ser necessário definir suas próprias tarefas personalizadas. Neste exemplo, temos um script para executar a fim de configurar alguns ambientes corretamente. O script é armazenado em uma pasta dentro do seu espaço de trabalho e chamado de test.sh para Linux e macOS e test.cmd para Windows. Com o arquivo tasks.json, a execução deste script pode ser tornada possível com uma tarefa personalizada que define o que fazer em sistemas operacionais diferentes.

```json
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "Executar testes",
      "type": "shell",
      "command": "./scripts/test.sh",
      "windows": {
        "command": ".\\scripts\\test.cmd"
      },
      "group": "test",
      "presentation": {
        "reveal": "always",
        "panel": "new"
      }
    }
  ]
}

```

O comando aqui é um comando de shell e diz ao sistema para executar test.sh ou test.cmd. Por padrão, ele executará test.sh com o caminho fornecido. Este exemplo aqui também define propriedades específicas do Windows e diz para executar test.cmd em vez do padrão.

### Referências

Documentação do VS Code - [propriedades específicas do sistema operacional](https://vscode-docs.readthedocs.io/en/stable/editor/tasks/#operating-system-specific-properties)
