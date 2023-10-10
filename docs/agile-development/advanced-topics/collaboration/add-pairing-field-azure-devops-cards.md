# Como adicionar um Campo Personalizado de Emparelhamento em Histórias de Usuário do Azure DevOps

Este documento descreve os benefícios de adicionar um campo personalizado do tipo _Identidade_ nas histórias de usuário do [Azure DevOps](https://learn.microsoft.com/en-us/azure/devops/user-guide/what-is-azure-devops), pré-requisitos e um guia passo a passo.

## Benefícios de adicionar um campo personalizado

Ter os nomes de ambos os indivíduos [trabalhando em uma história](README.md) visíveis nos cartões do Azure DevOps pode ser útil durante as cerimônias de sprint e levar a uma maior responsabilidade pelo responsável pelo emparelhamento. Por exemplo, é mais fácil acompanhar os indivíduos designados para histórias como parte de um par durante o planejamento do sprint usando o campo "nomes de emparelhamento". Durante o stand-up, isso também pode ajudar o Líder de Processo a filtrar histórias atribuídas ao indivíduo (tanto como proprietário quanto como responsável pelo emparelhamento) e mostrá-las no quadro. Além disso, o campo de emparelhamento pode fornecer um ponto de dados adicional para relatórios e taxas de queima.

## Pré-requisitos

Antes de personalizar o Azure DevOps, revise [Configurar e personalizar o Azure Boards](https://learn.microsoft.com/en-us/azure/devops/boards/configure-customize).

Para adicionar um campo personalizado às histórias de usuário no Azure DevOps, as alterações devem ser feitas como uma **Configuração Organizacional**. Este documento, portanto, pressupõe o uso de uma Organização existente no Azure DevOps e que a conta de usuário usada para fazer essas alterações é membro do [Grupo de Administradores da Coleção de Projetos](https://learn.microsoft.com/en-us/azure/devops/organizations/security/set-project-collection-level-permissions).

### Alterar as configurações da organização

1. Duplicar o processo atualmente em uso.

    Navegue até as **Configurações da Organização**, dentro da aba Boards / Process.

    ![Configurações da Organização](./images/azure-devops-organization-settings.png)

2. Selecione o tipo de **Processo**, clique no ícone com três pontos **...** e clique em **Criar processo herdado**.

    ![Criar Processo Herdado](./images/azure-devops-create-inherited-process.png)

3. Clique no processo herdado recém-criado.

    Como você pode ver no exemplo abaixo, nós o chamamos de 'Emparelhamento'.

    ![Selecionar Processo Herdado](./images/azure-devops-pairing-process.png)

4. Clique no tipo de item de trabalho **História de Usuário**.

    ![Modificar História de Usuário](./images/azure-devops-user-story-process.png)

5. Clique em **Novo Campo**.

    ![Adicionar o novo campo](./images/azure-devops-new-field.png)

6. Dê um **Nome** e selecione **Identidade** em Tipo. Clique em **Adicionar Campo**.

    ![Adicionar o novo campo](./images/azure-devops-add-field-to-user-story.png)

    Isso completa a mudança nas Configurações da Organização. O restante das instruções deve ser concluído nas Configurações do Projeto.

### Alterar as configurações do projeto

1. Vá para o Projeto que será modificado, selecione **Configurações do Projeto**.

    ![Alterar configurações do projeto](./images/azure-devops-project-settings.png)

2. Selecione **Configuração do Projeto**.

    ![Alterar configurações do projeto](./images/azure-devops-project-configuration.png)

3. Clique na **página de personalização de processo**.

    ![Alterar configurações do projeto](./images/azure-devops-process-customization.png)

4. Clique em **Projetos** e depois clique em **Alterar processo**.

    ![Alterar configurações do projeto](./images/azure-devops-change-process.png)

5. Altere o **processo alvo** para Emparelhamento e clique em Salvar.

    ![Alterar configurações do projeto](./images/azure-devops-change-project-process.png)

6. Vá para **Boards**.

    ![Alterar configurações do projeto](./images/azure-devops-boards.png)

7. Clique no ícone de engrenagem para abrir Configurações.

    ![Alterar configurações do projeto](./images/azure-devops-board-settings.png)

8. Adicione o campo ao cartão.

    Clique no ícone verde + para adicionar e selecionar o campo de Emparelhamento. Marque a caixa para exibir campos, mesmo quando estiverem vazios. **Salvar e fechar**.

    ![Alterar configurações do projeto](./images/azure-devops-add-field-to-card.png)

9. Veja o cartão modificado.

    Observe o novo campo de Emparelhamento. A História agora pode ser atribuída a um Proprietário e a um Responsável pelo Emparelhamento!

    ![Alterar configurações do projeto](./images/azure-devops-pairing-field.png)
