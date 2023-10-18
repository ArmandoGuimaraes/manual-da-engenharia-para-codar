# Loki

O Loki é um sistema de agregação de logs horizontalmente escalável, altamente disponível e multi-inquilino, criado pela Grafana Labs, inspirado nas lições aprendidas com o Prometheus. O Loki é comumente chamado de 'Prometheus, mas para logs', o que faz todo sentido. Ambas as ferramentas seguem a mesma arquitetura, que consiste em um agente coletando métricas em cada um dos componentes do sistema de software, um servidor que armazena os logs e também o painel Grafana, que acessa o servidor Loki para criar suas visualizações e consultas. Dito isso, o Loki possui três componentes principais:

## Promtail

É a parte do agente do Loki. Pode ser usado para capturar logs de vários lugares, como `/var/log/`, por exemplo. A configuração do Promtail é um arquivo YAML chamado `config-promtail.yml`. Neste arquivo, são descaminhados todos os caminhos e fontes de log que serão agregados no servidor Loki.

## Servidor Loki

O servidor Loki é responsável por receber e armazenar todos os logs recebidos de todos os sistemas diferentes. O servidor Loki também é responsável pelas consultas feitas no Grafana, por exemplo.

## Painéis do Grafana

Os painéis do Grafana são responsáveis por criar as visualizações e realizar consultas. Afinal, será uma página da web na qual as pessoas com o acesso correto podem fazer login para ver, consultar e criar alertas para os logs agregados.

## Por que usar o Loki

A principal razão para usar o Loki em vez de outras ferramentas de agregação de logs é que o Loki otimiza o armazenamento necessário. Ele faz isso seguindo o mesmo padrão do Prometheus, que indexa as etiquetas e cria fragmentos do próprio log, usando menos espaço do que simplesmente armazenar os logs brutos.

## Referências

- [Site Oficial do Loki](https://grafana.com/oss/loki/)
- [Inserindo logs no Loki](https://grafana.com/docs/loki/latest/getting-started/get-logs-into-loki/)
- [Adicionando uma fonte do Loki ao Grafana](https://grafana.com/docs/grafana/latest/datasources/loki/#adding-the-data-source)
- [Melhores Práticas do Loki](https://grafana.com/docs/loki/latest/best-practices/)
