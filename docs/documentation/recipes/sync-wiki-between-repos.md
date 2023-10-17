# Como Sincronizar uma Wiki entre Repositórios

Este é um guia rápido para espelhar uma Wiki de Projeto em outro repositório.

```bash
# Clonar a wiki
git clone <URL do repositório da wiki de origem>

# Adicionar o repositório espelhado como um controle remoto
cd <pasta de trabalho do repositório da wiki de origem>
git remote add espelho <URL do repositório espelhado que deve existir previamente>
```

Agora, cada vez que você desejar sincronizar, execute o seguinte para obter as últimas atualizações do repositório da wiki de origem:

```bash
# Obter tudo
git pull -v
```

> **Aviso**: Verifique se a saída do comando pull mostra "From URL do repositório de origem". Se isso mostrar a URL do repositório espelhado, você esqueceu de redefinir o rastreamento. Execute `git branch -u origin/wikiMaster` e depois continue.

Em seguida, execute o seguinte para enviá-lo para o repositório espelhado e redefinir o branch para rastrear o repositório de origem novamente:

```bash
# Enviar todos os branches para o controle remoto espelhado
git push -u espelho

# Ressincronizar localmente com o repositório de origem
git branch -u origin/wikiMaster

```

A saída deve ser semelhante a esta quando executada:

```powershell
PS C:\Git\MinhaWikiProjeto> git pull -v
POST git-upload-pack (909 bytes)
remote: Repositórios Azure
remote: Encontrados 5 objetos para enviar. (0 ms)
Descompactando objetos: 100% (5/5), concluído.
De https://.....  wikiMaster -> origin/wikiMaster
Atualizando 7412b94..a0f543b
Encaminhamento rápido
 .../dffffds.md | 4 ++++
 1 arquivo alterado, 4 inserções(+)


PS C:\Git\MinhaWikiProjeto> git push -u espelho
Enumerando objetos: 9, concluído.
Contando objetos: 100% (9/9), concluído.
Compactação de objetos usando até 8 threads
Compactação de objetos: 100% (5/5), concluído.
Gravando objetos: 100% (5/5), 2.08 KiB | 2.08 MiB/s, concluído.
Total 5 (delta 4), reutilizados 0 (delta 0)
remote: Analisando objetos... (5/5) (6 ms)
remote: Armazenando packfile... concluído (48 ms)
remote: Armazenando índice... concluído (59 ms)
Para https://......
   7412b94..a0f543b  wikiMaster -> wikiMaster
Branch 'wikiMaster' configurado para rastrear o branch remoto 'wikiMaster' do 'espelho'.


PS C:\Git\MinhaWikiProjeto> git branch -u origin/wikiMaster
Branch 'wikiMaster' configurado para rastrear o branch remoto 'wikiMaster' do 'origin'.
```

