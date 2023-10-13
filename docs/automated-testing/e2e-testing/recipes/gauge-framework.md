# Framework Gauge

Gauge é um framework gratuito e de código aberto para escrever e executar testes E2E. Algumas características-chave do Gauge que o tornam único incluem:

- Sintaxe simples, flexível e rica baseada em Markdown.
- Suporte consistente entre plataformas/linguagens para escrever código de teste.
- Uma arquitetura modular com suporte a plugins.
- Extensível através de plugins e personalizável.
- Suporta execução orientada a dados e fontes de dados externas.
- Ajuda você a criar suítes de teste sustentáveis.
- Suporta Visual Studio Code, Intellij IDEA, suporte IDE.

## O que é uma Especificação

As especificações do Gauge são escritas usando uma sintaxe Markdown. Por exemplo:

```bash
# Procurar pelo blob de dados

## Procurar por arquivo
* Ir para o blob do Azure
```

Nesta especificação, *Procurar pelo blob de dados* é o **cabeçalho da especificação**, *Procurar por arquivo* é um **cenário** com um passo *Ir para o blob do Azure*.

## O que é uma Implementação

Você pode implementar os passos em uma especificação usando uma linguagem de programação, por exemplo:

```bash
from getgauge.python import step
import os
from step_impl.utils.driver import Driver
@step("Ir para o blob do Azure")
def gotoAzureStorage():
  URL = os.getenv('STORAGE_ENDPOINT')
  Driver.driver.get(URL)
```

O runner do Gauge lê e executa passos e suas implementações para cada cenário na especificação e gera um relatório de cenários aprovados ou reprovados.

```bash
# Procurar pelo blob de dados

## Procurar por arquivo  ✔

Relatório html gerado com sucesso para => reports/html-report/index.html
Especificações:       1 executadas      1 aprovadas        0 reprovadas        0 ignoradas
Cenários:    1 executados      1 aprovados        0 reprovados        0 ignorados
```

## Reutilizando Passos

O Gauge ajuda você a se concentrar no teste do fluxo de uma aplicação. O Gauge faz isso tornando os passos tão reutilizáveis quanto possível. Com o Gauge, você não precisa construir frameworks personalizados usando uma linguagem de programação.

Por exemplo, os passos do Gauge podem passar parâmetros para uma implementação usando um texto entre aspas.

```bash
# Procurar pelo blob de dados

## Procurar por arquivo
* Ir para o blob do Azure
* Procurar por "store_data.csv"
```

A implementação agora pode usar "store_data.csv" da seguinte forma:

```bash
from getgauge.python import step
import os
@step("Procurar por <query>")
def searchForQuery(query):
  write(query)
  press("Enter")
```

Você pode então reutilizar este passo dentro ou entre cenários com diferentes parâmetros:

```bash
# Procurar pelo blob de dados

## Procurar por dados da loja #1
* Ir para o blob do Azure
* Procurar por "store_1.csv"

## Procurar por dados da loja #2
* Ir para o blob do Azure
* Procurar por "store_2.csv"
```

Ou combinar mais de um passo em **conceitos**

```bash
# Procurar no armazenamento do Azure por <query>
* Ir para o blob do Azure
* Procurar por "store_1.csv"
```

O conceito, Procurar no armazenamento do Azure por `<query>` pode ser usado como um passo em uma especificação.

```bash
# Procurar pelo blob de dados

## Procurar por dados da loja #1
* Procurar no armazenamento do Azure por "store_1.csv"

## Procurar por dados da loja #2
* Procurar no armazenamento do Azure por "store_2.csv"
```

## Teste Orientado a Dados

O Gauge também suporta teste orientado a dados usando tabelas Markdown, bem como arquivos csv externos, por exemplo:

```bash
# Procurar pelo blob de dados

| query   |
|---------|
| store_1 |
| store_2 |
| store_3 |

## Procurar por dados das lojas
* Procurar no armazenamento do Azure por <query>
```

Isso executará o cenário para todas as linhas da tabela.

Nos exemplos acima, refatoramos uma especificação para ser concisa e flexível sem alterar a implementação.

## Outros Recursos

Esta é uma breve introdução a alguns recursos do Gauge. Consulte a documentação do Gauge para recursos adicionais, como:

- [Relatórios](https://docs.gauge.org/getting_started/view-a-report.html)
- [Tags](https://docs.gauge.org/execution.html?#filter-specifications-and-scenarios-by-using-tags)
- [Execução Paralela](https://docs.gauge.org/execution.html#filter-specifications-and-scenarios-by-using-tags)
- [Ambientes](https://docs.gauge.org/configuration.html#using-environments-in-a-gauge-project)
- [Capturas de tela](https://docs.gauge.org/writing-specifications.html#taking-custom-screenshots)
- [Plugins](https://docs.gauge.org/plugin.html)
- E muito mais

## Instalando o Gauge

Este guia de introdução o levará pelos recursos principais do Gauge. Ao final deste guia, você será capaz de instalar o Gauge e aprender como criar seu primeiro projeto de automação de teste com o Gauge.

## Instruções de Instalação para Windows OS

### Passo 1: Instalando o Gauge no Windows

Esta seção fornece instruções específicas sobre como configurar o Gauge em um ambiente Microsoft Windows.
Baixe o seguinte [pacote de instalação](https://github.com/getgauge/gauge/releases/download/v1.0.6/gauge-1.0.6-windows.x86_64.exe) para obter a versão estável mais recente do Gauge.

### Passo 2: Instalando a extensão Gauge para Visual Studio Code

Siga os passos para adicionar o plugin Gauge Visual Studio Code a partir do IDE.

1. Instale a seguinte [extensão Gauge para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=getgauge.gauge).

### Solução de Problemas de Instalação

Se, ao executar sua primeira especificação do Gauge, você receber o erro de pacotes Python ausentes, abra a janela do terminal de linha de comando e execute este comando:

```bash
python.exe -m pip install getgauge==0.3.7 --user
```

## Instruções de Instalaçãopara macOS

### Passo 1: Instalando o Gauge no macOS

Esta seção fornece instruções específicas sobre como configurar o Gauge em um ambiente macOS.

1. Instale o brew se ainda não o tiver feito: Acesse o [site do brew](https://brew.sh/) e siga as instruções lá.
2. Execute o comando brew para instalar o Gauge:

   ```bash
   > brew install gauge
   ```

   Se o HomeBrew estiver funcionando corretamente, você deverá ver algo semelhante ao seguinte:

```bash
==> Buscando gauge
==> Baixando https://ghcr.io/v2/homebrew/core/gauge/manifests/1.4.3
######################################################################## 100.0%
==> Baixando https://ghcr.io/v2/homebrew/core/gauge/blobs/sha256:05117bb3c0b2efeafe41e817cd3ad86307c1d2ea7e0e835655c4b51ab2472893
==> Baixando de https://pkg-containers.githubusercontent.com/ghcr1/blobs/sha256:05117bb3c0b2efeafe41e817cd3ad86307c1d2ea7e0e835655c4b51ab2472893?se=2022-12-13T12%3A35%3A00Z&sig=I78SuuwNgSMFoBTT
######################################################################## 100.0%
==> Despejando gauge--1.4.3.ventura.bottle.tar.gz
    /usr/local/Cellar/gauge/1.4.3: 6 arquivos, 18.9MB
```

### Passo 2: Instalando a extensão Gauge para Visual Studio Code

Siga os passos para adicionar o plugin Gauge Visual Studio Code a partir do IDE:

1. Instale a seguinte [extensão Gauge para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=getgauge.gauge).

### Solução de Problemas Pós-Instalação

Se, ao executar sua primeira especificação do Gauge, você receber o erro de pacotes Python ausentes, abra a janela do terminal de linha de comando e execute este comando:

```bash
python.exe -m pip install getgauge==0.3.7 --user
```
