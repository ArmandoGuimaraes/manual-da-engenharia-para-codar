# Rotação de Segredos

A rotação de segredos é o processo de atualização dos segredos usados pela aplicação.
A melhor maneira de autenticar-se nos serviços Azure é usando uma identidade gerenciada, mas existem alguns cenários em que isso não é uma opção. Nesses casos, são usadas chaves de acesso ou segredos. Você deve periodicamente rotacionar chaves de acesso ou segredos.

## Por que a Rotação de Segredos

Segredos são um ativo e, como tal, têm o potencial de vazar ou serem roubados. Ao rotacionar os segredos, estamos revogando quaisquer segredos que possam ter sido comprometidos. Portanto, os segredos devem ser rotacionados com frequência.

## Identidade Gerenciada

As identidades gerenciadas do Azure são emitidas automaticamente pela Azure para identificar recursos individuais e podem ser usadas para autenticação em vez de segredos e senhas. O atrativo em usar Identidades Gerenciadas é a eliminação da gestão de segredos e credenciais. Eles não são necessários nas máquinas dos desenvolvedores ou verificados no controle de origem, e não precisam ser rotacionados. As identidades gerenciadas são consideradas mais seguras do que as alternativas e são a escolha recomendada.

## Aplicando a Rotação de Segredos

Se a Identidade Gerenciada do Azure não puder ser usada. Esta e as próximas seções explicarão como alcançar a rotação de segredos:

Para promover a rotação frequente de um segredo - defina um processo automatizado de rotação periódica de segredos.
O processo de rotação de segredos pode resultar em tempo de inatividade quando a aplicação é reiniciada para introduzir o novo segredo. Uma solução comum para isso é ter duas versões de segredo disponíveis, também chamadas de Rotação de Segredo Azul/Verde. Ao ter um segundo segredo à mão, podemos iniciar uma segunda instância da aplicação com esse segredo antes que o segredo anterior seja revogado, evitando assim qualquer tempo de inatividade.

## Estruturas e Ferramentas de Rotação de Segredos

1. Para a rotação de um segredo para recursos que usam um conjunto de credenciais de autenticação [clique aqui](https://learn.microsoft.com/en-us/azure/key-vault/secrets/tutorial-rotation)
1. Para a rotação de um segredo para recursos que têm dois conjuntos de credenciais de autenticação [clique aqui](https://learn.microsoft.com/en-us/azure/key-vault/secrets/tutorial-rotation-dual?tabs=azure-cli)

## Conclusão

Atualizar segredos é importante para garantir que seu segredo permaneça em segredo sem causar tempo de inatividade em sua aplicação.
