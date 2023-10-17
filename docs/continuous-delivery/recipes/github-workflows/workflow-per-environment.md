# Fluxos de Trabalho no GitHub

Um fluxo de trabalho é um processo automatizado configurável composto por um ou mais trabalhos, onde cada um desses trabalhos pode ser uma ação no GitHub. Atualmente, é suportado um formato de arquivo YAML para definir um fluxo de trabalho no GitHub.

Informações adicionais sobre ações do GitHub e Fluxos de Trabalho no GitHub estão nos links postados na seção de [referências](#references) abaixo.

## Fluxo de Trabalho por Ambiente

A abordagem geral é ter um pipeline, onde o código é construído, testado e implantado, e o artefato é então promovido para o próximo ambiente, eventualmente sendo implantado na produção.

Existem várias maneiras no GitHub de configurar um ambiente. Uma maneira de fazer isso é ter um fluxo de trabalho para vários ambientes, mas a complexidade aumenta à medida que processos e trabalhos adicionais são adicionados a um fluxo de trabalho, o que não significa que não pode ser feito para pipelines pequenos. O ponto positivo de ter um único fluxo de trabalho é que, quando um artefato flui de um ambiente para outro, os valores de estado e ambiente entre os ambientes de implantação podem ser facilmente passados.

![Workflow-Designs-Dependent-Workflows](images/Workflow-Designs-Dependent-Workflows.png)

Uma maneira de contornar a complexidade de um único fluxo de trabalho é ter fluxos de trabalho separados para diferentes ambientes, garantindo que apenas os artefatos criados e validados sejam promovidos de um ambiente para outro, bem como que o fluxo de trabalho seja pequeno o suficiente para depurar problemas em qualquer um dos fluxos de trabalho. Nesse caso, os valores de estado e ambiente precisam ser passados de um ambiente de implantação para outro. Múltiplos fluxos de trabalho também ajudam a manter as implantações nos ambientes independentes, reduzindo assim o tempo para implantar e encontrar problemas mais cedo do que tarde no processo. Além disso, como os ambientes são independentes entre si, qualquer falha na implantação em um ambiente não bloqueia as implantações em outros ambientes. Um trade-off neste método é que, com diferentes fluxos de trabalho para cada ambiente, a manutenção aumenta à medida que a complexidade dos fluxos de trabalho aumenta ao longo do tempo.

![Workflow-Designs-Independent-Workflows](images/Workflow-Designs-Independent-Workflows.png)

## Referências

- [Ações do GitHub](https://docs.github.com/en/actions)
- [Fluxos de Trabalho do GitHub](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions)
