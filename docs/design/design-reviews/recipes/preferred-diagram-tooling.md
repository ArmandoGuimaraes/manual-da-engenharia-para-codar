# Ferramentas de Diagramação Preferidas

Em cada etapa do processo de engajamento, os diagramas desempenham um papel fundamental na revisão de design. A ferramenta preferida para criar e manter diagramas é escolher **uma** das seguintes opções:

- Microsoft Visio
- Microsoft PowerPoint
- O formato `.drawio.png` (ou `.drawio`) do [diagrams.net](http://diagrams.net) (anteriormente [draw.io](http://draw.io))

Em todos os casos, recomendamos armazenar as imagens PNG exportadas desses diagramas no repositório, juntamente com os arquivos de origem, para que possam ser facilmente referenciadas na documentação e revisadas mais facilmente durante os PRs. O formato `.drawio.png` armazena ambos ao mesmo tempo.

## Microsoft Visio

Ele contém muitas formas prontas para uso, incluindo ícones do Azure, o aplicativo desktop está disponível para PC, e existe um ótimo aplicativo web. A maioria dos diagramas no Azure Architecture Center são diagramas do Visio.

## Microsoft PowerPoint

Os diagramas podem ser facilmente reutilizados em apresentações, uma licença do PowerPoint é bastante comum, o aplicativo desktop está disponível para PC e para Mac, e existe um ótimo aplicativo web.

## `.drawio.png`

Existem diferentes aplicativos de desktop, web e extensões do VS Code.
Esta ferramenta pode ser usada como o Visio ou LucidChart, sem as preocupações de licenciamento/armazenamento remoto.
Além disso, o Diagrams.net possui uma coleção de ícones do Azure/Office/Microsoft, além de outros techs conhecidos, portanto, não é apenas útil para swimlanes e fluxogramas, mas também para diagramas de arquitetura.

`.drawio.png` deve ser preferido sobre o formato `.drawio`.
O formato `.drawio.png` utiliza a camada de metadados dentro do formato de arquivo PNG para ocultar a representação gráfica vetorial SVG e, em seguida, renderiza o .png ao salvar.
Essa utilização inteligente tanto da camada de metadados quanto da camada de imagem permite que qualquer pessoa edite ainda mais o arquivo PNG.
Além disso, ele é renderizado como um PNG normal em navegadores e outros visualizadores, tornando-o fácil de transferir e incorporar.
Além disso, pode ser editado facilmente no VSCode usando a [Extensão do VSCode Draw.io Integration](https://marketplace.visualstudio.com/items?itemName=hediet.vscode-drawio).
