# Comando básicos do Git

## Configure o Git

Configure informações de usuário para todos os repositórios locais.

Configura o nome que você quer ligado às suas transações de commit:

`$ git config --global user.name "[nome]"`

Configura o email que você quer ligado às suas transações de commit:

`$ git config --global user.email "[endereco-de-email]"`

## Crie repositórios

Inicie um novo repositório ou obtenha de uma URL existente

Cria um novo repositório local com um nome especificado:

`$ git init [nome-do-projeto]`

Baixa um projeto e seu histórico de versão inteiro:

`$ git clone [url]`

## Refatore nomes de arquivos

Mude e remova os arquivos versionados

Remove o arquivo do diretório de trabalho e o prepara a remoção:

`$ git rm [arquivo]`

Remove o arquivo do controle de versão mas preserva o arquivo localmente:

`$ git rm --cached [arquivo]`

Muda o nome do arquivo e o prepara para o commit:

`$ git mv [arquivo-original] [arquivo-renomeado]`

## Suprima o monitoramento

Ignore arquivos e diretórios temporários.

Um arquivo de texto chamado `.gitignore` suprime o versionamento acidental de arquivos e diretórios correspondentes aos padrões especificados, como no exemplo abaixo:

```
*.log
build/
temp-*
```

Lista todos os arquivos ignorados neste projeto:

`$ git ls-files --others --ignored --exclude-standard`

## Salve fragmentos

Arquive e restaure mudanças incompletas.

Armazena temporariamente todos os arquivos monitorados modificados:

`$ git stash`

Restaura os arquivos recentes em stash:

`$ git stash pop`

Lista todos os conjuntos de alterações em stash:

`$ git stash list`

Descarta os conjuntos de alterações mais recentes em stash:

`$ git stash drop`

## Faça mudanças

Revise edições e crie uma transação de commit.

Lista todos os arquivos novos ou modificados para serem commitados:

`$ git status`

Mostra diferenças no arquivo que ainda não foram preparadas:

`$ git diff`

Faz o snapshot de um arquivo na preparação para versionamento:

`$ git add [arquivo]`

Mostra a diferença entre arquivos preparados e suas últimas versões:

`$ git diff --staged`

Retira o arquivo da área de preparação, mas preserva seu conteúdo:

`$ git reset [arquivo]`

Grava o snapshot permanentemente do arquivo no histórico de versão:

`$ git commit -m "[mensagem descritiva]"`

Edita a mensagem do último commit:

`$ git commit --amend`

Reescreva o histórico de múltiplos commits:

`$ git rebase -i HEAD~[numero-de-commit-a-serem-combinados]`

## Mudanças em grupo

Nomeie uma série de commits e combine os esforços completos

Lista todos os branches locais no repositório atual:

`$ git branch`

Cria um novo branch:

`$ git branch [nome-do-branch]`

Muda para o branch especificado e atualiza o diretório de trabalho:

`$ git switch -c [nome-do-branch]`

Combina o histórico do branch especificado ao branch atual:

`$ git merge [nome-do-branch]`

Exclui o branch especificado:

`$ git branch -d [nome-do-branch]`

Você também pode utilizar o comando `checkout` para trabalhar com branches.

Cria um novo branch e muda para branch criado:

`$ git checkout -b [nome-do-branch]`

Muda para o branch especificado, criado previamente:

`$ git checkout [nome-do-branch]`

## Revise o histórico

Navegue e inspecione a evolução dos arquivos do projeto.

Lista o histórico de versões para o branch atual:

`$ git log`

Lista o histórico de versões para um arquivo, incluindo mudanças de nome:

`$ git log --follow [arquivo]`

Mostra a diferença de conteúdo entre dois branches:

`$ git diff [primerio-branch]...[segundo-branch]`

Retorna mudanças de metadata e conteúdo para o commit especificado:

`$ git show [commit]`

## Desfaça commits

Apague enganos e crie um histórico substituto.

Desfaz todos os commits depois de [commit], preservando mudanças locais:

`$ git reset [commit]`

Descarta todo histórico e mudanças para o commit especificado:

`$ git reset --hard [commit]`

## Sincronize mudanças

Registre um repositório remoto e troque o histórico de versão.

Baixe todo o histórico de um repositório remoto:

`$ git fetch [nome-remoto]`

Combina o branch remoto ao branch local atual:

`$ git merge [nome-remoto]/[branch]`

Envia todos os commits do branch local para o GitHub:

`$ git push [alias] [branch]`

Baixa o histórico e incorpora as mudanças:

`$ git pull`

## Criando separações com Tags

Crie marcas claras no código com tags.

Cria uma nova tag:

`$ git tag -a [nome-tag]`

Sincronize a tag para repositório remoto:

`$ git push --tags`

## Entenda a diferença entre pull e pull rebase

Imagine que você está fazendo commits na branch `main` com outras pessoas da equipe. O estado atual do seu repositório remoto atual seja:

```
M1<---M2<---M3
```

Após fazer o clone do repositório, você faz mais dois commits locais (C1 e C2). Na sua máquina, o repositório ficará parecido com o seguinte:

```
                  .---C1<---C2
                 /
M1<---M2<---M3<-´
```

Porém, outros desenvolvedores já fizeram o `push` de mais 3 commits no `main` enquanto você tem ainda seus 2 commits que estão apenas no seu repositório local. Na branch `main` remota você está assim:

```
M1<---M2<---M3<---M4<---M5<---M6
```

Com o `git pull`, o git vai gerar um novo commit que concentra as alterações que ocorreram frutos deste merge dos commits C1 e C2 com os commits M4, M5 e M6. O comando git pull faz, por trás dos bastidores, duas coisas: um git fetch e um git merge.

Na etapa de git _fetch_, você irá receber as alterações e terá no seu repositório a seguinte situação:

```
                  .---C1<---C2
                 /
M1<---M2<---M3<-´---M4<---M5<---M6
```

Na etapa de _merge_, seu repositório gerará o commit M7, resultado deste merge:

```
                  .----C1<---C2<----.
                 /                   \
M1<---M2<---M3<-´---M4<---M5<---M6<---`M7
```

Mas, em cenários diferentes, nem sempre o git gera este commit de merge. Muitas vezes apenas você alterou a branch master, incluindo novos commits. Então ao fazer `git pull`, o git faz uma operação _fast forward_, simplesmente falando que seu último commit é o final da branch `main`:

```
                  .----C1<---C2
                 /            |
M1<---M2<---M3<-´             |
                           [main]
```

O git _rebase_ é uma espécie de merge também, mas usa uma lógica diferente. Ao invés de gerar um novo commit, ele reaplica cada um dos commits da branch local "em cima" (no topo) do último commit da branch remota.

Ou seja, se temos 2 commits (C1 e C2), eles serão aplicados a partir do commit M6.

Sendo assim, ao realizar `git pull --rebase`, temos a situação:

```
                                     .----C1’<---C2’
                                    /
M1<---M2<---M3<---M4<---M5<---M6<-´
```

Note que o C1 e C2 agora de C1’ e C2’. Motivo? Após o rebase, estes commits não são os mesmos commits originais. Serão commits totalmente novos, até mesmo com o SHA (identificador único e hash) diferentes.

Depois do _rebase_, nosso histórico ficará assim:

```
M1<---M2<---M3<---M4<---M5<---M6<---C1’<---C2’
```
