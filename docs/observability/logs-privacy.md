# Orientações para Privacidade

## Visão Geral

Para garantir a privacidade dos usuários do seu sistema, bem como cumprir várias regulamentações como o GDPR, alguns tipos de dados não devem existir em registros (logs).
Isso inclui informações sensíveis dos clientes, Informações de Identificação Pessoal (IIP), e qualquer outro dado que não tenha sido legalmente autorizado.

### Práticas Recomendadas

1. Separe os componentes e minimize as partes do sistema que registram dados sensíveis.
2. Evite colocar dados sensíveis em URLs, uma vez que os URLs de solicitação geralmente são registrados por proxies e servidores da web.
3. Evite usar dados de IIP para depuração do sistema o máximo possível. Por exemplo, use IDs em vez de nomes de usuário.
4. Use Registros Estruturados e inclua uma lista de negação (deny-list) para propriedades sensíveis.
5. Faça um esforço extra para identificar declarações de registro com dados sensíveis durante a revisão de código, pois é comum os revisores pularem a leitura de declarações de registro. Isso pode ser adicionado como uma caixa de seleção adicional se você estiver usando Modelos de Solicitação de Pull (Pull Request Templates).
6. Inclua mecanismos para detectar dados sensíveis em registros, em seus pipelines organizacionais para QA ou Testes Automatizados.

### Ferramentas e Métodos de Implementação

Use essas ferramentas e métodos para desidentificação de dados sensíveis em registros.

#### Application Insights

O Application Insights oferece interceptação de telemetria em alguns dos SDKs, que pode ser feita implementando a interface `ITelemetryProcessor`.
O `ITelemetryProcessor` processa as informações de telemetria antes de serem enviadas para o Application Insights e pode ser útil em muitas situações, como filtragem e modificações. Abaixo está um exemplo de interceptação de telemetria do tipo 'trace':

```csharp
using Microsoft.ApplicationInsights.DataContracts;

namespace Exemplo
{
    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.Extensibility;

    internal class RedactTelemetryInitializer : ITelemetryInitializer
    {
        public void Initialize(ITelemetry telemetry)
        {
            var requestTelemetry = telemetry as TraceTelemetry;
            if (requestTelemetry == null) return;
            # redija os e-mails do parâmetro de mensagem
            requestTelemetry.Message = Regex.Replace(requestTelemetry.Message, @"[^@\s]+@[^@\s]+\.[^@\s]+", "[e-mail removido]");
        }
    }
}
```

#### Elastic Stack

O Elastic Stack (anteriormente conhecido como "ELK stack") permite a interceptação de registros por meio dos [plugins de filtro](https://www.elastic.co/guide/en/logstash/current/filter-plugins.html) do Logstash.
O uso de alguns dos plugins existentes, como 'mutate', 'alter' e 'prune', pode ser suficiente para a maioria dos casos de desidentificação e ocultação de IIPs.
Para casos de uso mais robustos e personalizados, pode-se usar um plugin 'ruby', executando código Ruby arbitrário.
Os plugins de filtro também estão disponíveis em algumas alternativas ao Logstash, como [Fluentd](https://docs.fluentd.org/filter) e [Fluent Bit](https://docs.fluentbit.io/manual/pipeline/filters).

#### Presidio

[Presidio](https://github.com/microsoft/presidio) oferece API de proteção e anonimização de dados. Ele fornece módulos de identificação e anonimização rápidos para entidades privadas em texto.
O Presidio permite o uso de reconhecedores de IIPs predefinidos ou personalizados, aproveitando o Reconhecimento de Entidades Nomeadas, expressões regulares, lógica baseada em regras e soma de verificação com contexto relevante em vários idiomas.
Ele pode ser usado junto com os métodos de interceptação de logs mencionados acima para ajudar a garantir que os dados sensíveis sejam gerenciados e governados adequadamente.
O Presidio é contido para uma API HTTP REST e também pode ser instalado como um pacote Python, para ser chamado a partir de código Python.
Em vez de lidar com a anonimização no código da aplicação, ambas as APIs podem ser usadas por meio de chamadas externas.
O Elastic Stack, por exemplo, pode lidar com a ocultação de IIPs usando o filtro 'ruby' para chamar o Presidio na API HTTP REST ou chamando um script Python que consome o Presidio como um pacote:

`logstash.conf`

```ruby
input {
    ...
}

filter {
   ruby {
    code => 'require "open3"
             message = event.get("message")
             # Chama um script Python que aciona o analisador e

 anonimizador do Presidio e imprime o resultado.
             cmd =  "python /caminho/para/o/script/de/anonimização/do/presidio.py \"#{message}\""
             # Obtém a saída padrão do script.
             stdin, stdout, stderr = Open3.popen3(cmd)
             # Substitui a mensagem pelo texto anonimizado.
             event.set("message", stdout.read)
             filter_matched(event)'
   }
}

output {
    ...
}
```

