# Fundamentos de Dados e DataOps

A maioria dos projetos envolve algum tipo de armazenamento de dados, processamento de dados e DataOps. Para esses projetos, assim como para todos os projetos, seguimos as diretrizes gerais apresentadas em outras seções sobre segurança, testes, observabilidade, CI/CD etc.

## Objetivo

O objetivo desta seção é descrever brevemente como aplicar os fundamentos a projetos de dados pesados ou a partes do projeto.

## Isolamento

Por favor, tenha cuidado com os [níveis de isolamento](https://en.wikipedia.org/wiki/Isolation_(database_systems)) que você está usando. Mesmo com um banco de dados que oferece serializabilidade, é possível que, dentro de uma transação ou conexão, você esteja usando um nível de isolamento mais baixo do que o banco de dados oferece. Em particular, a leitura não confirmada (ou consistência eventual) pode ter muitos efeitos colaterais imprevisíveis e introduzir bugs difíceis de entender. Sistemas eventualmente consistentes devem ser tratados como último recurso para atender aos requisitos de escalabilidade; o uso de lotes, fragmentação e armazenamento em cache são todas soluções recomendadas para aumentar a escalabilidade. Se nenhuma dessas opções for viável, considere avaliar os bancos de dados "New SQL" como CockroachDB ou TiDB, antes de utilizar uma opção que dependa da consistência eventual.

Existem outros níveis de isolamento, fora dos níveis de isolamento mencionados no link acima. Alguns deles têm nuances diferentes dos 4 principais níveis e podem ser difíceis de comparar. Isolamento de Snapshot, serialização estrita, "ler o próprio escrito", leituras monotônicas, staleness limitada, consistência causal e linearização são todos outros termos que você pode explorar para aprender mais sobre o assunto.

## Controle de Concorrência

Seus sistemas devem (quase sempre) aproveitar alguma forma de controle de concorrência, para garantir a correção entre solicitações concorrentes e evitar corridas de dados. As duas formas de controle de concorrência são **pessimista** e **otimista**.

Uma transação **pessimista** envolve uma primeira solicitação para "bloquear os dados" e uma segunda solicitação para escrever os dados. Entre essas solicitações, nenhuma outra solicitação que acesse esses dados terá sucesso. Consulte [2 Phase Locking](https://en.wikipedia.org/wiki/Two-phase_locking) (também conhecido como 2 Phase Commit) para obter mais informações.

A abordagem (mais) recomendada é a concorrência **otimista**, onde um usuário pode ler o objeto em uma versão específica e atualizar o objeto apenas se ele não tiver sido alterado. Isso é normalmente feito via [Cabeçalho Etag](https://en.wikipedia.org/wiki/HTTP_ETag).

Uma maneira simples de fazer isso no lado do banco de dados é incrementar um número de versão em cada atualização. Isso pode ser feito em uma única instrução executada da seguinte forma:

> AVISO: o código abaixo não funcionará quando estiver usando um nível de isolamento igual ou inferior ao "read uncommitted" (consistência eventual).

```SQL
-- Trate isso como código fictício e ajuste conforme necessário.

UPDATE <nome_da_tabela>
SET campo1 = valor1, ..., campoN = valorN, versao = $nova_versao
WHERE ID = $id AND versao = $versao
```

## Camadas de Dados (Qualidade de Dados)

Desenvolva um entendimento comum da qualidade de seus conjuntos de dados, para que todos entendam a qualidade dos dados, os casos de uso esperados e as limitações.

Um modelo comum de qualidade de dados é `Bronze`, `Silver`, `Gold`

- **Bronze:** Esta é uma área de desembarque para seus conjuntos de dados brutos com pouca ou nenhuma transformação de dados aplicada, e, portanto, são otimizados para gravações / ingestão. Trate esses conjuntos de dados como um armazenamento imutável, apenas para anexar.
- **Silver:** São conjuntos de dados limpos e semi-processados. Eles se conformam a um esquema conhecido e a invariantes de dados predefinidos e podem ter mais augmentação de dados aplicada. Normalmente, são usados por cientistas de dados.
- **Gold:** São conjuntos de dados altamente processados e altamente otimizados para leitura, principalmente para o consumo de usuários de negócios. Normalmente, são estruturados em suas tabelas padrão de fatos e dimensões.

Divida seu data lake em três grandes áreas contendo seus conjuntos de dados Bronze, Silver e Gold.

> Nota: Áreas de armazenamento adicionais para dados malformados, dados intermediários (sandbox) e bibliotecas/pacotes/binários também são úteis ao projetar a organização do armazenamento.

## Validação de Dados

Valide os dados no início do seu pipeline

- Adicione validação de dados entre os conjuntos de dados Bronze e Silver. Validando no início do seu pipeline, você pode garantir que todos os conjuntos de dados estejam em conformidade com um esquema específico e invariantes de dados conhecidos. Isso também pode potencialmente evitar falhas no pipeline de dados em caso de alterações inesperadas nos dados de entrada.
- Os dados que não passam por esta etapa de validação podem ser encaminhados para um registro dedicado de dados malformados para fins de diagnóstico.
- Pode ser tentador adicionar validação antes de desembarcar na área Bronze do seu data lake. Isso geralmente não é recomendado. Os conjuntos de dados Bronze existem para garantir que você tenha uma cópia o mais próxima possível dos dados do sistema de origem. Isso pode ser usado para repetir o pipeline de dados tanto para testes (ou seja, testar a lógica de validação de dados) quanto para fins de recuperação de dados (ou seja, a corrupção de dados é introduzida devido a um erro no código de transformação de dados e, portanto, o pipeline precisa ser repetido).

## Pipelines de Dados Idempotentes

Torne seus pipelines de dados reutilizáveis e idempotentes

- Conjuntos de dados Silver e Gold podem ser corrompidos por vários motivos, como bugs não intencionais, alterações inesperadas nos dados de entrada e outros. Ao tornar os pipelines de dados reutilizáveis

 e idempotentes, você pode se recuperar desse estado por meio da implantação de correções de código e repetição dos pipelines de dados.
- A idempotência também garante que a duplicação de dados seja mitigada ao repetir seus pipelines de dados.

## Testes

Garanta que o código de transformação de dados seja testável

- Abstrair o código de transformação de dados do código de acesso aos dados é fundamental para garantir que testes unitários possam ser escritos contra a lógica de transformação de dados. Um exemplo disso é mover o código de transformação de dados de cadernos para pacotes.
- Embora seja possível executar testes em cadernos, ao extrair o código para pacotes, você aumenta a produtividade do desenvolvedor, aumentando a velocidade do ciclo de feedback.

## CI/CD, Controle de Origem e Revisões de Código

- Todos os artefatos necessários para construir o pipeline de dados do zero devem estar sob controle de origem. Isso inclui artefatos de infraestrutura como código, objetos de banco de dados (definições de esquema, funções, procedimentos armazenados etc.), dados de referência/aplicação, definições de pipeline de dados e lógica de validação e transformação de dados.
- Quaisquer novos artefatos (código) introduzidos no repositório devem passar por revisões de código, tanto automáticas (verificação de lint, verificação de credenciais etc.) quanto revisões por pares.
- Deve haver um processo seguro e repetível (CI/CD) para mover as alterações por meio de desenvolvimento, teste e, finalmente, produção.

## Segurança e Configuração

- Mantenha um local central e seguro para configurações sensíveis, como strings de conexão de banco de dados, que podem ser acessadas pelos serviços apropriados dentro do ambiente específico.
- No Azure, isso geralmente é resolvido por meio da segurança de segredos em um Key Vault por ambiente e, em seguida, os serviços relevantes consultam o KeyVault para obter a configuração.

## Observabilidade

Monitore infraestrutura, pipelines e dados

- Uma solução de monitoramento adequada deve estar em vigor para garantir que as falhas sejam identificadas, diagnosticadas e abordadas de forma oportuna. Além da infraestrutura base e das execuções de pipelines, os dados também devem ser monitorados. Uma área comum que deve ter monitoramento de dados é o registro de dados malformados.

## Amostras de Tecnologia de Ponta a Ponta e Azure

O repositório [DataOps for the Modern Data Warehouse](https://github.com/Azure-Samples/modern-data-warehouse-dataops) contém amostras de tecnologia específicas e de ponta a ponta sobre como implementar o DataOps no Azure.

![CI/CD](images/CI_CD_process.png?raw=true "CI/CD")
Imagem: CI/CD para pipelines de dados no Azure - do repositório DataOps para o Modern Data Warehouse
