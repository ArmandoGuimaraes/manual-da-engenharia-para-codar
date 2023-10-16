# Evidências e Medidas

## Evidências

Muitos dos itens de garantia de qualidade de código podem ser automatizados ou aplicados por meio de políticas em sistemas modernos de controle de versão e rastreamento de itens de trabalho. A verificação das políticas na branch principal no [Azure DevOps](https://azure.microsoft.com/pt-br/services/devops/) (AzDO) ou no [GitHub](https://github.com/), por exemplo, pode ser evidência suficiente de que uma equipe de projeto está conduzindo revisões de código.

* [ ] Todas as branches principais em todos os repositórios têm políticas de branch. - [Configurar políticas de branch](../tools.md#Configuring Branch Policies)
* [ ] Todas as compilações geradas a partir dos repositórios do projeto incluem linters apropriados e executam testes unitários.
* [ ] Cada item de trabalho de correção de bug deve incluir um link para o pull request que o introduziu, assim que o erro for diagnosticado. Isso ajuda na aprendizagem.
* [ ] Cada item de trabalho de correção de bug deve incluir uma observação sobre como o bug poderia (ou não) ter sido detectado em uma revisão de código.
* [ ] A equipe do projeto atualiza regularmente suas listas de verificação de revisão de código para refletir problemas comuns que eles encontraram.
* [ ] Os líderes de desenvolvimento devem revisar uma amostra de pull requests e/ou ser co-revisores com outros desenvolvedores para ajudar todos a melhorar suas habilidades como revisores de código.

## Medidas

A equipe pode coletar métricas de revisões de código para medir sua eficiência. Algumas métricas úteis incluem:

* Eficiência na Remoção de Defeitos (DRE) - uma medida da capacidade da equipe de desenvolvimento de remover defeitos antes do lançamento.
* Métricas de tempo:
  * Tempo gasto preparando-se para sessões de inspeção de código.
  * Tempo gasto em sessões de revisão.
* Linhas de código (LOC) inspecionadas por unidade de tempo/reunião.

É uma solução perfeitamente razoável rastrear essas métricas manualmente, por exemplo, em uma planilha do Excel. Também é possível utilizar os recursos de plataformas de gerenciamento de projetos - por exemplo, o AzDO permite painéis de métricas, incluindo [rastreamento de bugs](https://learn.microsoft.com/pt-br/azure/devops/boards/backlogs/manage-bugs?view=azure-devops&tabs=new-web-form). Você pode encontrar plugins prontos para várias plataformas - consulte o [GitHub Marketplace](https://github.com/marketplace), por exemplo - ou optar por implementar esses recursos por conta própria.

Lembre-se de que, uma vez que defeitos removidos graças a revisões são muito menos custosos em comparação com a detecção deles em produção, o custo das revisões de código é, na verdade, negativo!

Para obter mais informações, consulte os links na seção de [recursos](#resources).

## Recursos

* [Um Guia para Inspeções de Código](http://www.ganssle.com/inspections.pdf)
