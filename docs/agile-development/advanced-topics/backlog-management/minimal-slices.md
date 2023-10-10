# Fatias Minimalistas

## Sempre entregue seu trabalho usando fatias mínimas valiosas

- Divida seu item de trabalho em pequenos pedaços que são contribuídos em commits incrementais.

- Contribua com seus pedaços frequentemente. Siga uma abordagem iterativa, fornecendo atualizações e mudanças regularmente para a equipe. Isso permite feedback instantâneo e descoberta precoce de problemas, garantindo que você está desenvolvendo na direção certa, tanto tecnicamente quanto funcionalmente.

- NÃO trabalhe independentemente em sua tarefa sem fornecer atualizações para sua equipe.

### Exemplo

Imagine que você está trabalhando na adição de funcionalidade de construção de aplicativos UWP (Universal Windows Platform) para um serviço de integração contínua que já possui suporte para Android/iOS.

#### Abordagem Ruim

Após seis semanas de trabalho, você criou um PR com toda a funcionalidade necessária, incluindo a interface do portal (configurações de construção), API REST do backend (funcionalidade de construção UWP), telemetria, testes unitários e de integração, etc.

#### Boa Abordagem

Você dividiu seu recurso em histórias de usuário menores (que por sua vez foram divididas em várias tarefas) e começou a trabalhar nelas uma de cada vez:

- Como usuário, posso construir aplicativos UWP com sucesso usando o serviço atual
- Como usuário, posso ver a telemetria ao construir os aplicativos
- Como usuário, tenho a capacidade de selecionar a configuração de construção (debug, release)
- Como usuário, tenho a capacidade de selecionar a plataforma de destino (arm, x86, x64)
- ...

Você também dividiu suas histórias em tarefas menores e enviou PRs com base nessas tarefas.
Por exemplo, você tem as seguintes tarefas para a primeira história de usuário acima:

- Habilitar a plataforma UWP no backend
- Adicionar botão `construir` à interface do usuário (construir o primeiro arquivo de solução encontrado)
- Adicionar um menu suspenso `selecionar arquivo de solução` à interface do usuário
- Implementar testes unitários
- Implementar testes de integração para verificar se a construção foi bem-sucedida
- Atualizar documentação
- ...

### Recursos

- [Regras do Minimalismo](http://minifesto.org/)
