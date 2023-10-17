# IA Responsável na ISE

## Princípios de IA Responsável da Microsoft

Todo projeto de ML na ISE passa por uma avaliação de IA Responsável (RAI) para garantir que ele cumpra os [6 princípios de IA Responsável da Microsoft](https://www.microsoft.com/en-us/ai/responsible-ai):

- Equidade
- Confiabilidade e Segurança
- Privacidade e Segurança
- Inclusão
- Transparência
- Responsabilidade

Todo projeto passa pelo processo de RAI, seja para construir um novo modelo de ML do zero ou colocar um modelo existente em produção.

## Processo de IA Responsável da ISE

O processo começa assim que iniciamos um projeto prospectivo. Começamos preenchendo um documento de revisão de IA Responsável e uma avaliação de impacto, que oferece uma maneira estruturada de explorar tópicos como:

- O problema pode ser resolvido com uma solução não técnica (por exemplo, social)?
- O problema pode ser resolvido sem IA? Tecnologia mais simples seria suficiente?
- A equipe terá acesso a especialistas no domínio (por exemplo, médicos, refugiados) na área onde a IA é aplicável?
- Quem são as partes interessadas neste projeto? Quem é impactado pela IA? Existem grupos vulneráveis afetados?
- Quais são os possíveis benefícios e danos para cada parte interessada?
- Como a tecnologia pode ser mal utilizada e o que pode dar errado?
- A equipe analisou adequadamente os dados de entrada para garantir que os dados de treinamento sejam adequados para aprendizado de máquina?
- Os dados de treinamento representam com precisão os dados que serão usados como entrada na produção?
- Existe uma boa representação de todos os usuários?
- Existe um mecanismo de fallback (um humano na sequência ou uma maneira de reverter decisões com base no modelo)?
- Os dados usados pelo modelo para treinamento ou pontuação contêm informações pessoais identificáveis (PII)? Quais medidas foram tomadas para remover dados sensíveis?
- O modelo impacta decisões consequentes, como impedir que as pessoas obtenham empregos, empréstimos, assistência médica etc., ou nos casos em que isso pode ocorrer, considerações éticas apropriadas foram discutidas?
- Medidas de re-treinamento foram consideradas?
- Como podemos abordar quaisquer preocupações que surjam e como podemos mitigar riscos?

Neste ponto, pesquisamos [ferramentas e recursos disponíveis](https://www.microsoft.com/en-us/ai/responsible-ai-resources), como [InterpretML](https://interpret.ml/) ou [Fairlearn](https://github.com/fairlearn/fairlearn), que podemos usar no projeto. Podemos alterar o escopo do projeto ou redefinir a [definição do problema de ML](ml-problem-formulation-envisioning.md) se necessário.

Os documentos de revisão de IA Responsável permanecem documentos vivos que revisamos e atualizamos durante o desenvolvimento do projeto, por meio do [estudo de viabilidade](ml-feasibility-study.md), à medida que o modelo é desenvolvido e preparado para a produção, e à medida que novas informações surgem. Os documentos podem ser usados e expandidos assim que o modelo é implantado e monitorado em produção.
