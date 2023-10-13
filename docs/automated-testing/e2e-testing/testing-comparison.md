# Teste Unitário vs Teste de Integração vs Teste de Sistema vs Teste E2E

A tabela abaixo ilustra as características mais críticas e diferenças entre Teste Unitário, Teste de Integração, Teste de Sistema e Teste de Ponta a Ponta (E2E), e quando aplicar cada metodologia em um projeto.

|                       | Teste Unitário        | Teste de Integração                          | Teste de Sistema                                          | Teste E2E                                                    |
|-----------------------|-----------------------|----------------------------------------------|-----------------------------------------------------------|---------------------------------------------------------------|
| **Escopo**            | Módulos, APIs         | Módulos, interfaces                           | Aplicação, sistema                                        | Todos os subsistemas, dependências de rede, serviços e bancos de dados |
| **Tamanho**           | Pequeno               | Pequeno a médio                              | Grande                                                    | Extra Grande                                                  |
| **Ambiente**          | Desenvolvimento       | Teste de integração                          | Teste de QA                                               | Similar à produção                                            |
| **Dados**             | Dados simulados       | Dados de teste                               | Dados de teste                                            | Cópia de dados de produção reais                               |
| **Sistema Sob Teste** | Teste unitário isolado| Interfaces e fluxo de dados entre os módulos | Sistema específico como um todo                           | Fluxo da aplicação do início ao fim                            |
| **Cenários**          | Perspectivas do desenvolvedor | Perspectivas de desenvolvedores e testadores de TI | Perspectivas de desenvolvedores e testadores de QA       | Perspectivas do usuário final                                  |
| **Quando**            | Após cada build       | Após o teste unitário                        | Antes do teste E2E e após os testes unitários e de integração | Após o teste de sistema                                       |
| **Automatizado ou Manual** | Automatizado      | Manual ou automatizado                        | Manual ou automatizado                                     | Manual                                                        |
