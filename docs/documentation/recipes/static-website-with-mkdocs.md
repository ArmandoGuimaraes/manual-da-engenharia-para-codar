# Como criar um site estático para a sua documentação com base no MkDocs e MkDocs-Material

[MkDocs](https://www.mkdocs.org/) é uma ferramenta desenvolvida para criar sites estáticos a partir de arquivos markdown brutos. Outras alternativas incluem [Sphinx](https://www.sphinx-doc.org/en/master/) e [Jekyll](https://jekyllrb.com/).

Utilizamos o MkDocs para criar o site estático do [ISE Code-With Engineering Playbook](https://microsoft.github.io/code-with-engineering-manual/) a partir do conteúdo no [repositório do GitHub](https://github.com/microsoft/code-with-engineering-manual). Em seguida, o implantamos no [GitHub Pages](https://pages.github.com/).

Optamos pelo MkDocs por várias razões:

1. É fácil de configurar e tem uma ótima aparência mesmo na versão padrão.
2. Funciona bem com markdown, que é o formato que já usamos no Manual.
3. Usa uma pilha Python, o que é amigável para muitos colaboradores do Manual.

Para efeito de comparação, o Sphinx gera principalmente documentações a partir do formato restructured-text (rst), e o Jekyll é escrito em Ruby.

Para configurar um site MkDocs, os principais recursos necessários são:

1. Um arquivo `mkdocs.yaml`, semelhante ao que temos [no Manual](https://github.com/microsoft/code-with-engineering-manual/blob/main/mkdocs.yml). Este é o arquivo de configuração que define a aparência do site, a navegação, os plugins utilizados e muito mais.
2. Uma pasta chamada `docs` (o valor padrão para o diretório) que contém os arquivos de origem da documentação.
3. Uma [Ação do GitHub](https://docs.github.com/actions/learn-github-actions/understanding-github-actions) para gerar automaticamente o site (por exemplo, a cada commit na branch principal), semelhante a [esta do Manual](https://github.com/microsoft/code-with-engineering-manual/blob/main/.github/workflows/mkdocs.yml).
4. Uma lista de plugins usados durante a fase de construção do site. Especificamos os nossos [aqui](https://github.com/microsoft/code-with-engineering-manual/blob/main/requirements-docs.txt). E estes são os plugins que utilizamos:

    - [Material para MkDocs](https://squidfunk.github.io/mkdocs-material/): Aparência e experiência de usuário baseadas no Material Design.
    - [pymdown-extensions](https://facelessuser.github.io/pymdown-extensions/): Melhora a aparência do conteúdo baseado em markdown.
    - [mdx_truly_sane_lists](https://github.com/radude/mdx_truly_sane_lists): Para definir o nível de recuo das listas sem precisar refatorar toda a documentação que já tínhamos no Manual.

A configuração local é muito fácil. Consulte [Começando com o MkDocs](https://www.mkdocs.org/getting-started/) para obter detalhes.

Para publicar o site, há uma [integração fácil com o GitHub para armazenar o site como uma Página do GitHub](https://www.mkdocs.org/user-guide/deploying-your-docs/).

## Links adicionais

- [Plugins do MkDocs](https://github.com/mkdocs/mkdocs/wiki/MkDocs-Plugins)
- [Os melhores plugins e personalizações do MkDocs](https://chrieke.medium.com/the-best-mkdocs-plugins-and-customizations-fc820eb19759)
