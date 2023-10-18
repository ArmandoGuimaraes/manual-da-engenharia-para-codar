# Introdução ao Git LFS e VFS for Git

**Git LFS** e **VFS for Git** são soluções para usar o Git com arquivos binários (grandes) e árvores de código-fonte grandes.

## Git LFS

O Git é muito bom em rastrear alterações em arquivos baseados em texto, como código, mas não é tão eficaz em rastrear arquivos binários. Por exemplo, se você armazenar um arquivo de imagem do Photoshop (PSD) em um repositório, a cada alteração, o arquivo completo é armazenado novamente no histórico. Isso pode tornar o histórico do repositório Git muito grande, o que torna o clone do repositório cada vez mais demorado.

Uma solução para trabalhar com arquivos binários é usar o Git LFS (ou Git Large File System). Esta é uma extensão para o Git e deve ser instalada separadamente, e só pode ser usada com uma plataforma de repositório que suporta LFS. GitHub.com e Azure DevOps, por exemplo, são plataformas que têm suporte para LFS.

A maneira como funciona, em resumo, é que um arquivo de espaço reservado é armazenado no repositório com informações para o sistema LFS. Parece algo assim:

```shell
version https://git-lfs.github.com/spec/v1
oid a747cfbbef63fc0a3f5ffca332ae486ee7bf77c1d1b9b2de02e261ef97d085fe
size 4923023
```

O arquivo real é armazenado em um armazenamento separado. Dessa forma, o Git rastreará as alterações neste arquivo de espaço reservado, não no arquivo grande. A combinação do uso do Git e do Git LFS ocultará isso do desenvolvedor. Você continuará a trabalhar com o repositório e os arquivos como antes.

Ao trabalhar com esses arquivos grandes por conta própria, você ainda verá o histórico do Git crescer em sua própria máquina, pois o Git ainda começará a rastrear esses arquivos grandes localmente, mas quando você clonar o repositório, o histórico será realmente pequeno. Portanto, é benéfico para outras pessoas que não trabalham diretamente nos arquivos grandes.

### Prós do Git LFS

* Usa o fluxo de trabalho de ponta a ponta do Git para todos os arquivos.
* Git LFS suporta bloqueio de arquivos para evitar conflitos em ativos indiferenciáveis.
* Git LFS é totalmente suportado no Azure DevOps Services.

### Contras do Git LFS

* Todos que contribuem para o repositório precisam instalar o Git LFS.
* Se não configurado corretamente:
  * Arquivos binários enviados pelo Git LFS não são visíveis, pois o Git só baixará os dados que descrevem o arquivo grande.
  * Enviar binários grandes empurrará o arquivo binário completo para o repositório.
* O Git não pode mesclar as alterações de duas versões diferentes de um arquivo binário; o bloqueio de arquivos mitiga isso.
* O Azure Repos não oferece suporte ao uso do SSH para repositórios com arquivos rastreados pelo Git LFS - para obter mais informações, consulte a [documentação de autenticação do Git LFS](https://github.com/git-lfs/git-lfs/blob/master/docs/api/authentication.md).

### Instalação e uso do Git LFS

Acesse [https://git-lfs.github.com](https://git-lfs.github.com) e faça o download e instale o programa a partir de lá.

Para cada repositório que você deseja usar o LFS, você precisa seguir estas etapas:

* Configurar o LFS para o repositório:

```shell
git lfs install
```

* Indicar quais arquivos devem ser considerados como arquivos grandes (ou arquivos binários). Como exemplo, para considerar todos os arquivos do Photoshop como grandes:

```shell
git lfs track "*.psd"
```

Existem maneiras mais refinadas de indicar arquivos em uma pasta e mais. Consulte a [Documentação do Git LFS](https://github.com/git-lfs/git-lfs/tree/master/docs?utm_source=gitlfs_site&utm_medium=docs_link&utm_campaign=gitlfs) para mais informações.

Com esses comandos, um arquivo `.gitattribute` é criado, que contém essas configurações e deve fazer parte do repositório.

A partir daqui, você usa os comandos Git padrão para trabalhar no repositório. O resto será tratado pelo Git e Git LFS.

### Comandos Comuns do LFS

Instalar o Git LFS

```bash
git lfs install       # windows
sudo apt-get git-lfs  # linux
```

Veja as [instruções de instalação do Git LFS](https://github.com/git-lfs/git-lfs/wiki/Installation) para a instalação em outros sistemas.

Rastrear arquivos .mp4 com o Git LFS

```bash
git lfs track '*.mp4'
```

Atualizar o arquivo `.gitattributes` listando os arquivos e padrões a serem rastreados

```bash
*.mp4 filter=lfs diff=lfs merge=lfs -text
docs/images/* filter=lfs diff=lfs merge=lfs -text
```

Listar todos os padrões rastreados

```bash
git lfs track
```

Listar todos os arquivos rastreados

```bash
git lfs ls-files
```

Baixar arquivos para o seu diretório de trabalho

```bash
git lfs pull
git lfs pull --include="caminho/para/arquivo"
```

## VFS for Git

Imagine um repositório grande contendo vários projetos, por exemplo, um por recurso. Como desenvolvedor, você pode estar trabalhando apenas em alguns recursos e, portanto, não deseja baixar todos os projetos do repositório. Por padrão, com o Git, no entanto, clonar o repositório significa que você baixará *todos* os arquivos/projetos.

O VFS for Git (ou Virtual File System for Git) resolve esse problema, pois baixará apenas o que você precisa para sua máquina local, mas se você olhar no sistema de arquivos, por exemplo, com o Windows Explorer, ele mostrará todas as pastas e arquivos, incluindo os tamanhos de arquivo corretos.

A plataforma Git deve oferecer suporte ao GVFS para que isso funcione. GitHub.com e Azure DevOps oferecem suporte a isso nativamente.

### Instalação e uso do VFS for Git

A Microsoft criou o VFS for Git e o tornou de código aberto. Ele pode ser encontrado em

 [https://github.com/microsoft/VFSForGit](https://github.com/microsoft/VFSForGit). Está disponível apenas para Windows.

Os instaladores necessários podem ser encontrados em [https://github.com/Microsoft/VFSForGit/releases](https://github.com/Microsoft/VFSForGit/releases).

Na página de lançamentos, você encontrará dois downloads importantes:

* Instalador do Git 2.28.0.0, que é um requisito para executar o VFS for Git. Isso não é o mesmo que a instalação padrão do Git para Windows!
* Instalador SetupGVFS.

Baixe esses arquivos e instale-os em sua máquina.

Para usar o VFS for Git em um repositório, um arquivo `.gitattributes` precisa ser adicionado ao repositório com esta linha:

```shell
* -text
```

Para clonar um repositório em sua máquina usando o VFS for Git, use `gvfs` em vez de `git`, assim:

```shell
gvfs clone [URL] [dir]
```

Depois disso, você terá uma pasta que contém uma pasta `src` que contém o conteúdo do repositório. Isso é feito por uma prática de colocar todas as saídas dos sistemas de construção fora desta árvore. Isso facilita o gerenciamento de arquivos `.gitignore` e mantém o Git eficiente com muitos arquivos.

Para trabalhar com o repositório, basta usar os comandos Git como antes.

Para remover um repositório VFS for Git de sua máquina, certifique-se de que o processo VFS esteja parado e execute este comando na pasta principal:

```shell
gvfs unmount
```

Isso interromperá o processo e o desregistrará; depois disso, você pode remover com segurança a pasta.

### Referências

* [Primeiros passos com o Git LFS](https://git-lfs.github.com/)
* [Manual do Git LFS](https://github.com/git-lfs/git-lfs/tree/master/docs)
* [Git LFS no Azure Repos](https://learn.microsoft.com/en-us/azure/devops/repos/git/manage-large-files?view=azure-devops)
