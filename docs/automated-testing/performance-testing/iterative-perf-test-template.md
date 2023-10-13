# Modelo de Iteração de Teste de Desempenho

> Este documento fornece um modelo para capturar os resultados de testes de desempenho. Os testes de desempenho são feitos em iterações e cada iteração deve ter um objetivo claro. Os resultados de qualquer iteração são imutáveis, independentemente de o objetivo ter sido alcançado ou não. Se a iteração falhou ou o objetivo não foi alcançado, então uma nova iteração de teste é realizada com as correções apropriadas. É recomendável manter o registro das iterações gravadas para manter uma linha do tempo de como o sistema evoluiu e quais mudanças afetaram o desempenho de que maneira. Sinta-se à vontade para modificar este modelo conforme necessário.

## Modelo de Iteração

### Objetivo

> Mencione em tópicos o objetivo para esta iteração de teste. O objetivo deve ser pequeno e mensurável dentro desta iteração.

### Detalhes do Teste

- **Data**: *Data e hora em que esta iteração começou e terminou*
- **Duração**: *Tempo que levou para concluir esta iteração.*
- **Código da Aplicação**: *ID do commit e link para o commit dos códigos que estão sendo testados nesta iteração*
- **Configuração de Referência:**
  - **Configuração da Aplicação**: *Em tópicos, mencione a configuração da aplicação que deve ser registrada*
  - **Configuração do Sistema**: *Em tópicos, mencione a configuração da infraestrutura*

> Registre diferentes tipos de configurações. Geralmente, as mudanças de configuração específicas da aplicação entre iterações, enquanto as configurações do sistema ou infraestrutura raramente mudam.

### Itens de Trabalho

> Lista de links para itens de trabalho relevantes (tarefa, história, bug) sendo testados nesta iteração.

### Resultados

```md
Em tópicos, documente os resultados do teste.
- Anexe quaisquer documentos que apoiem os resultados do teste.
- Adicione links para o painel de métricas e logs, como Application Insights.
- Capture capturas de tela para métricas e inclua-as nos resultados. Um bom candidato para isso é o uso de CPU/Memória/Disco.
```

### Observações

> As observações são percepções derivadas dos resultados dos testes. Mantenha as observações breves e em forma de tópicos. Mencione resultados que apoiem o objetivo da iteração. Se alguma das observações resultar em um item de trabalho (tarefa, história, bug), adicione o link para o item de trabalho junto com a observação.
