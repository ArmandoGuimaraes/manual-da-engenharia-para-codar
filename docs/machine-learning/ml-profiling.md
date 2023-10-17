# Perfilagem de Código em Aprendizado de Máquina e MLOps

Projetos de Ciência de Dados, especialmente aqueles que envolvem técnicas de Aprendizado Profundo, geralmente consomem muitos recursos. Uma iteração de treinamento de modelo pode levar várias horas. Embora o processamento de grandes volumes de dados realmente leve tempo, pequenos bugs e implementações subótimas de algumas partes funcionais podem causar um consumo extra de recursos.

A perfilagem pode ser usada para identificar gargalos de desempenho e ver quais funções são mais custosas no código do aplicativo. Com base nas saídas do perfilador, é possível se concentrar nas ineficiências mais significativas e fáceis de resolver, e, assim, alcançar um melhor desempenho do código.
Embora a perfilagem siga os mesmos princípios de qualquer outro projeto de software, o objetivo deste documento é fornecer exemplos de perfilagem para os cenários mais comuns em projetos de MLOps / Ciência de Dados.

Abaixo estão alguns cenários comuns em projetos de MLOps / Ciência de Dados, juntamente com sugestões sobre como perfilá-los.

- [Perfilagem genérica em Python](#perfilagem-genérica-em-python)
- [Perfilagem de treinamento de modelo PyTorch](#perfilagem-de-treinamento-de-modelo-pytorch)
- [Perfilagem de pipeline de Azure Machine Learning](#perfilagem-de-pipeline-de-azure-machine-learning)

## Perfilagem genérica em Python

Normalmente, uma solução de MLOps / Ciência de Dados contém código Python simples que atende a diferentes finalidades (por exemplo, processamento de dados), juntamente com código de treinamento de modelo especializado. Embora muitos frameworks de Aprendizado de Máquina forneçam seu próprio perfilador, às vezes também é útil perfilar toda a solução.

Existem dois tipos de perfiladores: determinísticos (todos os eventos são rastreados, por exemplo, [cProfile](https://docs.python.org/3/library/profile.html)) e estatísticos (amostragem em intervalos regulares, por exemplo, [py-spy](https://pypi.org/project/py-spy/)). O exemplo abaixo mostra um exemplo de perfilador determinístico.

Existem muitas opções de perfilagem de código Python genérico determinístico. Uma das opções padrão de perfilagem é o profiler embutido [cProfile](https://docs.python.org/3/library/profile.html). Usando o *cProfile*, é possível perfilar facilmente um script Python ou apenas um trecho de código. Essa ferramenta de perfilagem produz um arquivo que pode ser visualizado usando ferramentas de código aberto ou analisado usando a classe `stats.Stats`. A última opção requer a configuração de parâmetros de filtragem e classificação para uma melhor experiência de análise.

Abaixo, você encontra um exemplo de uso do *cProfile* para perfilar um trecho de código.

```python
import cProfile

# Iniciar a perfilagem
profiler = cProfile.Profile()
profiler.enable()

# -- SEU CÓDIGO AQUI ---

# Parar a perfilagem
profiler.disable()

# Escrever os resultados do perfilador em um arquivo HTML
profiler.dump_stats("resultados_do_perfilador.prof")
```

Você também pode executar o *cProfile* fora do script Python usando o seguinte comando:

```bash
python -m cProfile [-o arquivo_de_saida] [-s ordem_de_classificação] (-m módulo | meu_script.py)
```

> Observação: geralmente, uma época de treinamento de modelo é suficiente para a perfilagem. Não é necessário executar mais épocas e aumentar o custo.

Consulte [The Python Profilers](https://docs.python.org/3/library/profile.html) para obter mais detalhes.

## Perfilagem de Treinamento de Modelo PyTorch

O PyTorch 1.8 inclui um [profiler PyTorch](https://pytorch.org/blog/introducing-pytorch-profiler-the-new-and-improved-performance-tool/) atualizado que é fornecido junto com a distribuição do PyTorch e não requer instalação adicional. Usando o *profiler PyTorch*, é possível registrar operações no lado da CPU, bem como lançamentos de kernel CUDA no lado da GPU. O profiler pode visualizar os resultados da análise usando o plugin TensorBoard e fornecer sugestões sobre gargalos e melhorias potenciais no código.

```python
import torch

with torch.profiler.profile(
    # Limitar o número de etapas de treinamento incluídas na perfilagem
    schedule=torch.profiler.schedule(wait=1, warmup=1, active=3, repeat=2),
    # Salvar automaticamente os resultados da perfilagem no disco
    on_trace_ready=torch.profiler.tensorboard_trace_handler,
    with_stack=True
) as profiler:
    for step, data in enumerate(trainloader, 0):
        # -- O CÓDIGO DA ETAPA DE TREINAMENTO VAI AQUI ---
        profiler.step()
```

O `tensorboard_trace_handler` pode ser usado para gerar arquivos de resultado para o TensorBoard. Eles podem ser visualizados instalando o plugin TensorBoard e executando o TensorBoard no diretório de log.

```bash
pip install torch_tb_profiler
tensorboard --logdir=<Caminho do Diretório de Log>
# Acesse `http://localhost:6006/#pytorch_profiler`
```

> Nota: certifique-se de fornecer os parâmetros corretos para o `torch.profiler.schedule`. Geralmente, você desejará perfilar várias etapas de treinamento em vez de toda a época.

Mais informações sobre o *profiler PyTorch*:

- [Receita do Profiler PyTorch](https://pytorch.org/tutorials/recipes/recipes/profiler_recipe.html)
- [Apresentando o PyTorch Profiler - a nova e melhorada ferramenta de desempenho](https://pytorch.org/blog/introducing-pytorch-profiler-the-new-and-improved-performance-tool/)

## Perfilagem de Pipelines Azure Machine Learning

Em nossos projetos, frequentemente usamos [Azure Machine Learning](https://azure.microsoft.com/en-us/services/machine-learning/) pipelines para treinar modelos de Machine Learning. A maioria dos profilers também pode ser usada em conjunto com o Azure Machine Learning. Para um profiler ser usado com o Azure Machine Learning, ele deve atender aos seguintes critérios:

- Ativar ou desativar o profiler deve ser possível passando um parâmetro para o script executado pelo Azure Machine Learning.
- O profiler deve produzir um arquivo como saída.

Em geral, o procedimento para usar profilers com o Azure Machine Learning é o seguinte:

1. (Opcional) Se você estiver usando a perfilagem em um pipeline do Azure Machine Learning, pode adicionar uma flag booleana `--profile` como um parâmetro do pipeline.
2. Use um dos profilers descritos acima ou qualquer outro profiler que possa produzir um arquivo como saída.
3. Dentro do seu script Python, crie uma pasta de saída para os resultados do profiler, por exemplo:

    ```python
    output_dir = "./outputs/profiler_results"
    os.makedirs(output_dir, exist_ok=True)
    ```

4. Execute o seu pipeline de treinamento.
5. Assim que o pipeline estiver concluído, acesse o portal do Azure ML e abra os detalhes da etapa que contém o código de treinamento. Os resultados podem ser encontrados na guia `Outputs+logs`, na pasta `outputs/profiler_results`.
6. Você pode querer baixar os resultados e visualizá-los localmente.

> Nota: não é recomendável executar profilers simultaneamente. Os perfis também consomem recursos, portanto, a execução simultânea pode afetar significativamente os resultados.
