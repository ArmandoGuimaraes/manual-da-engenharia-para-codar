# Regras de Engajamento

Ao realizar a análise de segurança de aplicativos, espera-se que o testador siga as *Regras de Engajamento* conforme estabelecido abaixo. Isso é feito para padronizar o escopo dos testes de aplicativos e fornecer uma compreensão concreta do que é considerado "fora de escopo" para a análise de segurança.

## Regras de Engajamento - Para aqueles que solicitam revisão

* Os Web Application Firewalls podem estar ativos e configurados, mas não ative nenhum bloqueio automático. Isso pode retardar significativamente a pessoa que realiza o teste.
* Da mesma forma, se um serviço estiver em execução em uma máquina virtual, verifique se serviços como `fail2ban` estão desativados.
* Você não pode fazer alterações na aplicação em execução até que o teste esteja completo. Isso é para evitar quebrar acidentalmente um ataque válido em andamento.
* Os resultados da revisão não são considerados como "finais". Uma revisão de segurança deve sempre ser realizada por uma equipe de segurança coordenada pelo cliente antes de colocar uma aplicação em produção. Se um cliente precisar de mais assistência, eles podem acionar o Suporte Premier.

## Regras de Engajamento - Para aqueles que realizam os testes

* Não tente realizar ataques de Negação de Serviço ou de outra forma derrubar os serviços. Varreduras ativas intensivas são toleradas (e são assumidas como parte do teste de carga), mas derrubadas deliberadas não são permitidas.
* Não interaja com seres humanos. O phishing de credenciais ou outros ataques do lado do cliente estão fora dos limites. Detalhar ataques de XSS e semelhantes é encorajado como parte do teste, mas não os utilize contra usuários internos ou clientes.
* Ataque a partir de um único ponto. Especialmente se a aplicação estiver atualmente nas mãos do cliente, forneça o endereço IP ou nome de host do host de ataque para evitar disparar alarmes.
