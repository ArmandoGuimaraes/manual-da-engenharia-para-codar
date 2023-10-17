# Testando Código de Data Science e MLOps

O objetivo deste documento é fornecer exemplos de testes para as operações mais comuns em projetos de MLOps/Data Science. Testar o código usado em projetos de MLOps ou ciência de dados segue os mesmos princípios de qualquer outro projeto de software.

Alguns cenários podem parecer diferentes ou mais difíceis de testar. A melhor maneira de abordar isso é sempre ter uma sessão de design de testes, onde o foco está nas entradas/saídas, exceções e na verificação do comportamento das transformações de dados. Projetar os testes primeiro facilita o teste, pois força um estilo mais modular, onde cada função tem um propósito, e a extração de funcionalidades comuns para funções e módulos.

A seguir, estão algumas operações comuns em projetos de MLOps ou Data Science, juntamente com sugestões sobre como testá-las.

* [Salvando e carregando dados](#salvando-e-carregando-dados)
* [Transformação de dados](#transformação-de-dados)
* [Carregamento ou previsão de modelo](#carregamento-ou-previsão-de-modelo)
* [Validação de dados](#validação-de-dados)
* [Teste de modelo](#teste-de-modelo)

## Salvando e carregando dados

Ler e escrever em arquivos CSV, ler imagens ou carregar arquivos de áudio são cenários comuns encontrados em projetos de MLOps.

### Exemplo: Verificar se uma função de carregamento chama read_csv se o arquivo existir

`utils.py`

```python
def load_data(filename: str) -> pd.DataFrame:
    if os.path.isfile(filename):
        df = pd.read_csv(filename, index_col='ID')
        return df
    return None
```

Não há necessidade de testar a função `read_csv` ou as funções `isfile`, podemos deixar os testes para os desenvolvedores do **pandas** e **os**.

A única coisa que precisamos testar aqui é a lógica nesta função, ou seja, que `load_data` carrega o arquivo se o arquivo existir com a coluna de índice correta e não carrega o arquivo se ele não existir, e que ela retorna os resultados esperados.

Uma maneira de fazer isso seria fornecer um arquivo de amostra, chamar a função e verificar se a saída é **None** ou um **DataFrame**. Isso requer que arquivos separados estejam presentes ou ausentes para que os testes sejam executados. Isso pode fazer com que o mesmo teste seja executado em uma máquina e depois falhe em um servidor de compilação, o que não é um comportamento desejado.

Uma maneira muito melhor é **mockear** as chamadas para `isfile` e `read_csv`. Em vez de chamar a função real, retornaremos um valor de retorno predefinido ou chamaremos um stub que não tenha efeitos colaterais. Dessa forma, nenhum arquivo é necessário no repositório para executar o teste, e o teste sempre funcionará da mesma maneira, independente da máquina em que for executado.

> Observação: Abaixo, estamos fazendo mocks das funções específicas do `os` e do `pd` referenciadas no arquivo `utils`. As outras funções são deixadas inalteradas e funcionarão normalmente.

`test_utils.py`

```python
import utils
from unittest.mock import patch


@patch('utils.os.path.isfile')
@patch('utils.pd.read_csv')
def test_load_data_chama_read_csv_se_existir(mock_isfile, mock_read_csv):
    # organizar
    # sempre retorne True para isfile
    utils.os.path.isfile.return_value = True
    filename = 'arquivo.csv'

    # agir
    _ = utils.load_data(filename)

    # afirmar
    # verifique se read_csv é chamado com os parâmetros corretos
    utils.pd.read_csv.assert_called_once_with(filename, index_col='ID')
```

Da mesma forma, podemos verificar se a função é chamada 0 ou várias vezes. No exemplo abaixo, verificamos que ela não é chamada se o arquivo não existe:

```python
@patch('utils.os.path.isfile')
@patch('utils.pd.read_csv')
def test_load_data_nao_chama_read_csv_se_nao_existir(mock_isfile, mock_read_csv):
    # organizar
    # o arquivo não existe
    utils.os.path.isfile.return_value = False
    filename = 'arquivo.csv'

    # agir
    _ = utils.load_data(filename)

    # afirmar
    # verifique se read_csv não é chamado
    assert utils.pd.read_csv.call_count == 0
```

### Exemplo: Usando os mesmos dados de amostra para vários testes

Se mais de um teste utilizar os mesmos dados de amostra, fixtures são uma boa maneira de reutilizar esses dados de amostra. Os dados de amostra podem ser o conteúdo de um arquivo JSON, um CSV, um DataFrame ou até mesmo uma imagem.

> Observação: Os dados de amostra ainda são codificados, se possível, e não precisam ser grandes. Adicione apenas a quantidade necessária de dados de amostra para que os testes sejam legíveis.

Use o fixture para retornar os dados de amostra e adicione isso como um parâmetro nos testes em que deseja usar os dados de amostra.

```python
import pytest

@pytest.fixture
def house_features_json():
  return {'area': 25, 'price': 2500, 'rooms': np.nan}

def test_clean_features_cleans_nan_values(house_features_json):
  cleaned_features = clean_features(house_features_json)
  assert cleaned_features['rooms'] == 0

def test_extract_features_extracts_price_per_area(house_features_json):
  extracted_features = extract_features(house_features_json)
  assert extracted_features['price_per_area'] == 100
```

Neste exemplo, o fixture `house_features_json` fornece os dados de amostra em formato JSON que são usados em dois testes diferentes. Isso ajuda a reutilizar os dados de amostra de maneira eficaz.

## Transformando dados

Para limpar e transformar dados, teste entradas e saídas fixas, mas tente limitar cada teste a uma verificação.

Por exemplo, crie um teste para verificar o formato de saída dos dados.

```python
def test_resize_image_gera_o_tamanho_correto():
  # Organizar
  imagem_original = np.ones((10, 5, 2, 3))

  # Agir
  imagem_redimensionada = utils.redimensionar_imagem(imagem_original, 100, 100)

  # Afirmar
  assert imagem_redimensionada.shape[:2] == (100, 100)
```

e um para verificar se qualquer preenchimento é feito adequadamente

```python
def test_resize_image_preenche_corretamente():
  # Organizar
  imagem_original = np.ones((10, 5, 2, 3))

  # Agir
  imagem_redimensionada = utils.redimensionar_imagem(imagem_original, 100, 100)

  # Afirmar
  assert imagem_redimensionada[0][0][0][0] == 0
  assert imagem_redimensionada[0][0][2][0] == 1
```

Para testar diferentes entradas e saídas esperadas automaticamente, use a parametrização.

```python
@pytest.mark.parametrize('altura_orig, largura_orig, altura_esperada, largura_esperada',
                         [
                             # menor do que o alvo
                             (10, 10, 20, 20),
                             # maior do que o alvo
                             (20, 20, 10, 10),
                             # mais largo do que o alvo
                             (10, 20, 10, 10)
                         ])
def test_resize_image_gera_o_tamanho_correto(altura_orig, largura_orig, altura_esperada, largura_esperada):
  # Organizar
  imagem_original = np.ones((altura_orig, largura_orig, 2, 3))

  # Agir
  imagem_redimensionada = utils.redimensionar_imagem(imagem_original, altura_esperada, largura_esperada)

  # Afirmar
  assert imagem_redimensionada.shape[:2] == (altura_esperada, largura_esperada)
```

## Carregar o Modelo ou Prever

Ao realizar testes de **unidade**, devemos simular o carregamento do modelo e as previsões do modelo da mesma forma que simular o acesso a arquivos.

Pode haver casos em que você deseja carregar seu modelo para realizar testes preliminares ou testes de integração.

Como esses testes geralmente demoram um pouco mais para serem executados, é importante separá-los dos testes de unidade, para que os desenvolvedores da equipe ainda possam executar os testes de unidade como parte do desenvolvimento orientado por testes.

Uma maneira de fazer isso é usando marcas (marks).

```python
@pytest.mark.longrunning
def test_integracao_entre_dois_sistemas():
    # isso pode demorar um pouco
```

Execute todos os testes que não estão marcados como `longrunning`

```bash
pytest -v -m "not longrunning"
```

### Testes Básicos de Unidade para Modelos de Aprendizado de Máquina

Os testes de unidade de ML não têm a intenção de verificar a precisão ou o desempenho de um modelo. Os testes de unidade para um modelo de ML destinam-se a verificar a qualidade do código, por exemplo:

- O modelo aceita as entradas corretas e produz as saídas com a forma correta?
- Os pesos do modelo são atualizados ao executar o `fit`?

Para fazer isso, os testes de modelos de ML não seguem estritamente as melhores práticas dos testes de unidade padrão - nem todas as chamadas externas são simuladas. Esses testes estão muito mais próximos de um [teste de integração estreito](https://martinfowler.com/bliki/IntegrationTest.html).
No entanto, os benefícios de ter testes simples para o modelo de ML ajudam a evitar que um modelo mal configurado passe horas em treinamento, enquanto ainda produz resultados ruins.

Exemplos de como implementar esses testes (para modelos de Aprendizado Profundo) incluem:

- Construir um modelo e comparar a forma das camadas de entrada com a de uma fonte de dados de exemplo. Em seguida, compare a forma da camada de saída com a saída esperada.
- Inicialize o modelo e registre os pesos de cada camada. Em seguida, execute uma única época de treinamento em um conjunto de dados fictício e compare os pesos do "modelo treinado" - verifique apenas se os valores mudaram.
- Treine o modelo em um conjunto de dados fictício por uma única época e, em seguida, valide com dados fictícios - valide apenas se a previsão está formatada corretamente, este modelo não será preciso.

## Validação de Dados

Uma parte importante dos testes de unidade é incluir casos de teste para validação de dados. Por exemplo, nenhum dado fornecido, imagens que não estão no formato esperado, dados contendo valores nulos ou valores discrepantes para garantir que o pipeline de processamento de dados seja robusto.

## Teste de Modelo

Além de testar o código de unidade, também podemos testar, depurar e validar nossos modelos de diferentes maneiras durante o processo de treinamento.

Algumas opções a serem consideradas nesta fase:

- [Testes adversariais e de limite para aumentar a robustez](https://medium.com/@deepmindsafetyresearch/towards-robust-and-verified-ai-specification-testing-robust-training-and-formal-verification-69bd1bc48bda)
- Verificar a precisão das classes sub-representadas.
