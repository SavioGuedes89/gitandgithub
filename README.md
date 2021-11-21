#  GIT E GITHUB
Entendendo o GIT E GITHUB na pratica! 

## Instalando o Git

http://git-scm.com/downloads

## Configuração Inicial do Git

### Sua Identidade

A primeira coisa que você deve fazer ao instalar Git é configurar seu nome de usuário e endereço de e-mail. Isto é importante porque cada commit usa esta informação, e ela é carimbada de forma imutável nos commits que você começa a criar:

```cmd
git config --global user.name "Fulano de Tal"
git config --global user.email fulanodetal@exemplo.br
```

### Seu Editor

Agora que a sua identidade está configurada, você pode escolher o editor de texto padrão que será chamado quando Git precisar que você entre uma mensagem. Se não for configurado, o Git usará o editor padrão, que normalmente é o Vim. Se você quiser usar um editor de texto diferente, como o Emacs, você pode fazer o seguinte:

```cmd
git config --global core.editor emacs
```

### Testando Suas Configurações

Se você quiser testar as suas configurações, você pode usar o comando `git config --list` para listar todas as configurações que o Git conseguir encontrar naquele momento:

```cmd
git config --list
user.name=Fulano de Tal
user.email=fulanodetal@exemplo.br
color.status=auto
color.branch=auto
color.interactive=auto
color.diff=auto
...
```

## Obtendo um Repositório Git

Você pode obter um projeto Git utilizando duas formas principais. 1. Você pode pegar um diretório local que atualmente não está sob controle de versão e transformá-lo em um repositório Git, ou 2. Você pode fazer um clone de um repositório Git existente em outro lugar.

### Inicializando um Repositório em um Diretório Existente

Para você começar a monitorar um projeto existente com Git, você deve ir para o diretório desse projeto. Se você nunca fez isso, use o comando a seguir, que terá uma pequena diferença dependendo do sistema em que está executando:

para Linux:
```cmd
cd /home/user/your_repository
```
para Mac:
```cmd
$ cd /Users/user/your_repository
```
para Windows:
```cmd
$ cd /c/user/your_repository
```
depois digite:
```cmd
$ git init
```

Isso cria um novo subdiretório chamado .git que contém todos os arquivos necessários de seu repositório – um esqueleto de repositório Git. Neste ponto, nada em seu projeto é monitorado ainda. (Veja [ch10-git-internals] para mais informações sobre quais arquivos estão contidos no diretório .git que foi criado.)

Se você quer começar a controlar o versionamento dos arquivos existentes (ao contrário de um diretório vazio), você provavelmente deve começar a monitorar esses arquivos e fazer um commit inicial. Você pode fazer isso com alguns comandos git add que especificam os arquivos que você quer monitorar, seguido de um git commit:

```cmd 
git add *.c
git add LICENSE
git commit -m 'initial project version'
```

### Clonando um Repositório Existente

Caso você queira obter a cópia de um repositório Git existente – por exemplo, um projeto que você queira contribuir – o comando para isso é `git clone`.

Você clona um repositório com `git clone [url]`. Por exemplo, caso você queria clonar a biblioteca Git Linkable chamada `libgit2`, você pode fazer da seguinte forma:

```cmd 
git clone https://github.com/libgit2/libgit2
```

O Git possui diversos protocolos de transferência que você pode utilizar. O exemplo anterior usa o protocolo `https://`, mas você também pode ver `git://` ou `user@server:path/to/repo.git`, que usam o protocolo de transferência SSH. Em Getting Git on a Server é apresentado todas as opções disponíveis com as quais o servidor pode ser configurado para acessar o seu repositório Git, e os prós e contras de cada uma.

## Gravando Alterações em Seu Repositório

### Verificando os Status de Seus Arquivos

A principal ferramenta que você vai usar para determinar quais arquivos estão em qual estado é o comando git status. Se você executar esse comando imediatamente após clonar um repositório, você vai ver algo assim:

```cmd 
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
nothing to commit, working directory clean
```

Isso significa que você tem um diretório de trabalho limpo - em outras palavras, nenhum de seus arquivos rastreados foi modificado.

Digamos que você adiciona um novo arquivo no seu projeto, um simples arquivo README. Se o arquivo não existia antes, e você executar git status, você verá seu arquivo não rastreado da seguinte forma:

```cmd 
$ echo 'My Project' > README
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Untracked files:
  (use "git add <file>..." to include in what will be committed)

    README

nothing added to commit but untracked files present (use "git add" to track)
```

Você pode ver que o seu novo arquivo README é um arquivo não rastreado, porque está abaixo do subtítulo “Untracked files” na saída do seu status. "Não rastreado" basicamente significa que o Git vê um arquivo que você não tinha no snapshot (commit) anterior; o Git não vai passar a incluir o arquivo nos seus commits a não ser que você o mande fazer isso explicitamente

O comportamento do Git é dessa forma para que você não inclua acidentalmente arquivos binários gerados automaticamente ou outros arquivos que você não deseja incluir. Você quer incluir o arquivo README, então vamos comeaçar a rastreá-lo.

### Rastreando Arquivos Novos

Para começar a rastrear um novo arquivo, você deve usar o comando git add. Para começar a rastrear o arquivo README, você deve executar o seguinte:

```cmd
git add README
```

Executando o comando status novamente, você pode ver que seu README agora está sendo rastreado e preparado (staged) para o commit:

```cmd
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    new file:   README

```
É possível saber que o arquivo está preparado porque ele aparece sob o título `Changes to be committed`. Se você fizer um commit neste momento, a versão do arquivo que existia no instante em que você executou `git add`, é a que será armazenada no histórico de snapshots. 

### Preparando Arquivos Modificados

Vamos modificar um arquivo que já estava sendo rastreado. Se você modificar o arquivo `CONTRIBUTING.md`, que já era rastreado, e então executar `git status` novamente, você deve ver algo como:

```cmd
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    new file:   README

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   CONTRIBUTING.md
```

O arquivo `CONTRIBUTING.md` aparece sob a seção `Changes not staged for commit` — que indica que um arquivo rastreado foi modificado no diretório de trabalho mas ainda não foi mandado para o stage (preparado).Para mandá-lo para o stage, você precisa executar o comando `git add`.

#### git add
O git add é um comando de múltiplos propósitos: serve para começar a rastrear arquivos e também para outras coisas, como marcar arquivos que estão em conflito de mesclagem como resolvidos. Pode ser útil pensar nesse comando mais como “adicione este conteúdo ao próximo commit”.
#### ------

Vamos executar git add agora, para mandar o arquivo CONTRIBUTING.md para o stage, e então executar git status novamente:

```cmd
$ git add CONTRIBUTING.md
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    new file:   README
    modified:   CONTRIBUTING.md
```

### Ignorando Arquivos

Frequentemente você terá uma classe de arquivos que não quer que sejam adicionados automaticamente pelo Git e nem mesmo que ele os mostre como não-rastreados. Geralmente, esses arquivos são gerados automaticamente, tais como arquivos de log ou arquivos produzidos pelo seu sistema de compilação (build).

Nesses casos, você pode criar um arquivo chamado `.gitignore`, contendo uma lista de padrões de nomes de arquivo que devem ser ignorados. Aqui está um exemplo de arquivo `.gitignore`:    

```cmd
$ cat .gitignore
*.[oa]
*~
```
A primeira linha diz ao Git para ignorar todos os arquivos que terminem com “.o” ou “.a” – arquivos objeto ou de arquivamento, que podem ser produtos do processo de compilação. A segunda linha diz ao Git para ignorar todos os arquivos cujo nome termine com um til (`~`), que é usado por muitos editores de texto, como o Emacs, para marcar arquivos temporários.

As regras para os padrões que podem ser usados no arquivo .gitignore são as seguintes:

- Linhas em branco ou começando com # são ignoradas.

-Os padrões que normalmente são usados para nomes de arquivos funcionam.

-Você pode iniciar padrões com uma barra (/) para evitar recursividade.

-Você pode terminar padrões com uma barra (/) para especificar um diretório.

-Você pode negar um padrão ao fazê-lo iniciar com um ponto de exclamação (!).

Padrões de nome de arquivo são como expressões regulares simplificadas usadas em ambiente shell. Um asterisco (`*`) casa com zero ou mais caracteres; `[abc]` casa com qualquer caracter dentro dos colchetes (neste caso, a, b ou c); um ponto de interrogação (`?`) casa com um único caracter qualquer; e caracteres entre colchetes separados por hífen (`[0-9]`) casam com qualquer caracter entre eles (neste caso, de 0 a 9). Você também pode usar dois asteriscos para criar uma expressão que case com diretórios aninhados; `a/**/z casaria com a/z, a/b/z, a/b/c/z`, e assim por diante.

Aqui está outro exemplo de arquivo .gitignore:

```text
# ignorar arquivos com extensão .a
*.a

# mas rastrear o arquivo lib.a, mesmo que você esteja ignorando os arquivos .a acima
!lib.a

# ignorar o arquivo TODO apenas no diretório atual, mas não em subdir/TODO
/TODO

# ignorar todos os arquivos no diretório build/
build/

# ignorar doc/notes.txt, mas não doc/server/arch.txt
doc/*.txt

# ignorar todos os arquivos .pdf no diretório doc/
doc/**/*.pdf
```

### Visualizando Suas Alterações Dentro e Fora do Stage

Se o comando `git status` for vago demais para você — você quer saber exatamente o que você alterou, não apenas quais arquivos foram alterados — você pode usar o comando `git diff`.

Digamos que você altere o arquivo README e o mande para o stage e então altere o arquivo CONTRIBUTING.md sem mandá-lo para o stage. Se você executar o comando git status, você verá mais uma vez alguma coisa como o seguinte:

```cmd
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    modified:   README

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   CONTRIBUTING.md
```

Para ver o que você alterou mas ainda não mandou para o stage, digite o comando git diff sem nenhum argumento:

```cmd
$ git diff
diff --git a/CONTRIBUTING.md b/CONTRIBUTING.md
index 8ebb991..643e24f 100644
--- a/CONTRIBUTING.md
+++ b/CONTRIBUTING.md
@@ -65,7 +65,8 @@ branch directly, things can get messy.
 Please include a nice description of your changes when you submit your PR;
 if we have to read the whole diff to figure out why you're contributing
 in the first place, you're less likely to get feedback and have your change
-merged in.
+merged in. Also, split your changes into comprehensive chunks if your patch is
+longer than a dozen lines.

 If you are starting to work on a particular area, feel free to submit a PR
 that highlights your work in progress (and note in the PR title that it's
 ```

 Esse comando compara o que está no seu diretório de trabalho com o que está no stage. O resultado permite que você saiba quais alterações você fez que ainda não foram mandadas para o stage.

 Se você quiser ver as alterações que você mandou para o stage e que entrarão no seu próximo commit, você pode usar `git diff --staged`. Este comando compara as alterações que estão no seu stage com o seu último commit:

```cmd
$ git diff --staged
diff --git a/README b/README
new file mode 100644
index 0000000..03902a1
--- /dev/null
+++ b/README
@@ -0,0 +1 @@
+My Project
```

### Fazendo Commit das Suas Alterações

Agora que sua área de stage está preparada do jeito que você quer, você pode fazer commit das suas alterações.
Lembre-se que qualquer coisa que ainda não foi enviada para o stage — qualquer arquivo que você tenha criado ou alterado e que ainda não tenha sido adicionado com `git add` — não entrará nesse commit.
O jeito mais simples de fazer commit é digitar `git commit`:

```cmd
git commit
```
Fazendo isso, será aberto o editor de sua escolha.

O editor mostra o seguinte texto (este é um exemplo da tela do Vim):

```text
# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
# On branch master
# Your branch is up-to-date with 'origin/master'.
#
# Changes to be committed:
#	new file:   README
#	modified:   CONTRIBUTING.md
#
~
~
~
".git/COMMIT_EDITMSG" 9L, 283C
```

Quando você sair do editor, o Git criará seu commit com essa mensagem (com os comentários e diferenças removidos).

Alternativamente, você pode digitar sua mensagem de commit diretamente na linha de comando, depois da opção -m do comando commit, assim:

```cmd
$ git commit -m "Story 182: Fix benchmarks for speed"
[master 463dc4f] Story 182: Fix benchmarks for speed
 2 files changed, 2 insertions(+)
 create mode 100644 README
```

#### Você acaba de criar seu primeiro commit!

### Pulando a Área de Stage

Mesmo sendo incrivelmente útil para preparar commits exatamente como você quer, a área de stage algumas vezes é um pouco mais complexa do que o necessário. Se você quiser pular a área de stage, o Git fornece um atalho simples. A opção `-a`, do comando `git commit`, faz o Git mandar todos arquivos rastreados para o `stage` automaticamente, antes de fazer o commit, permitindo que você pule a parte do `git add`:

```cmd
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   CONTRIBUTING.md

no changes added to commit (use "git add" and/or "git commit -a")
$ git commit -a -m 'added new benchmarks'
[master 83e38c7] added new benchmarks
 1 file changed, 5 insertions(+), 0 deletions(-)
```

Perceba que, nesse caso, você não tem que executar git add antes, para adicionar o arquivo CONTRIBUTING.md ao commit. Isso ocorre porque a opção -a inclui todos os arquivos alterados. Isso é conveniente, mas cuidado; algumas vezes esta opção fará você incluir alterações indesejadas.

### Removendo Arquivos

Para remover um arquivo do Git, você tem que removê-lo dos seus arquivos rastreados (mais precisamente, removê-lo da sua área de stage) e então fazer um commit.O comando `git rm` faz isso, e também remove o arquivo do seu diretório de trabalho para que você não o veja como um arquivo não-rastreado nas suas próximas interações.

Se você simplesmente remover o arquivo do seu diretório, ele aparecerá sob a área “Changes not staged for commit” (isto é, fora do stage) da saída do seu git status:
```cmd
$ rm PROJECTS.md
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        deleted:    PROJECTS.md

no changes added to commit (use "git add" and/or "git commit -a")
```

Mas, se você executar git rm, o arquivo será preparado para remoção (retirado do stage):

```cmd
$ git rm PROJECTS.md
rm 'PROJECTS.md'
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    deleted:    PROJECTS.md
```

Da próxima vez que você fizer um commit, o arquivo será eliminado e não será mais rastreado. Se o arquivo tiver sido alterado ou se já tiver adicionado à área de stage, você terá que forçar a remoção com a opção `-f`. Essa é uma medida de segurança para prevenir a exclusão acidental de dados que ainda não tenham sido gravados em um snapshot e que não poderão ser recuperados do histórico. 

Outra coisa útil que você pode querer fazer é manter o arquivo no seu diretório de trabalho, mas removê-lo da sua área de `stage`. Em outras palavras, você pode querer manter o arquivo no seu disco rígido, mas não deixá-lo mais sob controle do Git. Isso é particularmente útil se você esquecer de adicionar alguma coisa ao seu arquivo `.gitignore` e, acidentalmente, mandá-la para o `stage`, como um grande arquivo de log ou um monte de arquivos compilados `.a`. Para fazer isso, use a opção `--cached`: 

```cmd
git rm --cached README
```
Você pode passar arquivos, diretórios e padrões de nomes para o comando `git rm`. Isso quer dizer que você pode fazer coisas como:

```cmd
$ git rm log/\*.log
```
Note a barra invertida (`\`) na frente do `*`. Isso é necessário porque o Git faz sua própria expansão de nomes de arquivos em adição a que é feita pela sua shell. Esse comando remove todos os arquivos que tenham a extensão `.log` do diretório `log/`. Ou, você pode fazer algo como o seguinte:

```cmd
$ git rm \*~  
```

### Movendo Arquivos

Comando `mv`. Se você quiser renomear um arquivo no Git, você pode executar alguma coisa como:

```cmd
$ git mv arq_origem arq_destino
```
```cmd
$ git mv README.md README
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    renamed:    README.md -> README

```

Contudo, isso é equivalente a executar algo como:

```cmd
$ mv README.md README
$ git rm README.md
$ git add README
```

## Vendo o histórico de Commits

Depois de você ter criado vários commits ou se você clonou um repositório com um histórico de commits pré-existente, você vai provavelmente querer olhar para trás e ver o que aconteceu. A ferramenta mais básica e poderosa para fazer isso é o comando `git log`.

Quando você executa git log neste projeto, você deve receber um retorno que se parece com algo assim:

```cmd
$ git log
commit ca82a6dff817ec66f44342007202690a93763949
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Mon Mar 17 21:52:11 2008 -0700

    changed the version number

commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Sat Mar 15 16:40:33 2008 -0700

    removed unnecessary test

commit a11bef06a3f659402fe7563abf99ad00de2209e6
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Sat Mar 15 10:31:28 2008 -0700

    first commit
```

Por padrão, sem argumentos, `git log` lista os commits feitos neste repositório em ordem cronológica inversa; isto é, o commit mais recente aparece primeiro.

Uma das opções que mais ajuda é `-p`, que mostra as diferenças introduzidas em cada commit. Você pode também usar `-2`, que lista no retorno apenas os dois últimos itens:

```cmd
$ git log -p -2
commit ca82a6dff817ec66f44342007202690a93763949
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Mon Mar 17 21:52:11 2008 -0700

    changed the version number

diff --git a/Rakefile b/Rakefile
index a874b73..8f94139 100644
--- a/Rakefile
+++ b/Rakefile
@@ -5,7 +5,7 @@ require 'rake/gempackagetask'
 spec = Gem::Specification.new do |s|
     s.platform  =   Gem::Platform::RUBY
     s.name      =   "simplegit"
-    s.version   =   "0.1.0"
+    s.version   =   "0.1.1"
     s.author    =   "Scott Chacon"
     s.email     =   "schacon@gee-mail.com"
     s.summary   =   "A simple gem for using Git in Ruby code."

commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Sat Mar 15 16:40:33 2008 -0700

    removed unnecessary test

diff --git a/lib/simplegit.rb b/lib/simplegit.rb
index a0a60ae..47c6340 100644
--- a/lib/simplegit.rb
+++ b/lib/simplegit.rb
@@ -18,8 +18,3 @@ class SimpleGit
     end

 end
-
-if $0 == __FILE__
-  git = SimpleGit.new
-  puts git.show
-end
\ No newline at end of file
```

Você pode também usar uma série de opções resumidas com o `git log`. Por exemplo, se você quer ver algumas estatísticas abreviadas para cada commit, você pode usar a opção `--stat`:

```cmd
$ git log --stat
commit ca82a6dff817ec66f44342007202690a93763949
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Mon Mar 17 21:52:11 2008 -0700

    changed the version number

 Rakefile | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)

commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Sat Mar 15 16:40:33 2008 -0700

    removed unnecessary test

 lib/simplegit.rb | 5 -----
 1 file changed, 5 deletions(-)

commit a11bef06a3f659402fe7563abf99ad00de2209e6
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Sat Mar 15 10:31:28 2008 -0700

    first commit

 README           |  6 ++++++
 Rakefile         | 23 +++++++++++++++++++++++
 lib/simplegit.rb | 25 +++++++++++++++++++++++++
 3 files changed, 54 insertions(+)
```

Como você pode ver, a opção --stat apresenta abaixo de cada commit uma lista dos arquivos modificados, quantos arquivos foram modificados, e quantas linhas nestes arquivos foram adicionadas e removidas. 

Uma outra opção realmente muito util é --pretty. Essa opção modifica os registros retornados para formar outro formato diferente do padrão. Algumas opções pré-definidas estão disponíveis para você usar. A opção oneline mostra cada commit em uma única linha, esta é de muita ajuda se você está olhando para muitos commits. Em adição, as opções short, full, e fuller apresentam o retorno quase no mesmo formato porem com menos ou mais informações, respectivamente:

``` cmd
$ git log --pretty=oneline
ca82a6dff817ec66f44342007202690a93763949 changed the version number
085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7 removed unnecessary test
a11bef06a3f659402fe7563abf99ad00de2209e6 first commit
```

A opção mais interessante é `format`, a qual permite a você especificar seu próprio formato de registros de retorno. Isso é especialmente útil quando você esta gerando um retorno para uma máquina analisar – pois você especifica o formato explicitamente, você sabe que isso não irá mudar com as atualizações do Git:

``` cmd
$ git log --pretty=format:"%h - %an, %ar : %s"
ca82a6d - Scott Chacon, 6 years ago : changed the version number
085bb3b - Scott Chacon, 6 years ago : removed unnecessary test
a11bef0 - Scott Chacon, 6 years ago : first commit
```

## Desfazendo coisas

Em qualquer estágio, você talvez queira desfazer algo. Aqui, vamos rever algumas ferramentas básicas para desfazer modificações que porventura tenha feito. Seja cuidadoso, porque nem sempre você pode voltar uma alteração desfeita. Essa é uma das poucas áreas do Git onde pode perder algum trabalho feito se você cometer algum engano.

Um dos motivos mais comuns para desfazer um comando, aparece quando você executa um commit muito cedo e possivelmente esquecendo de adicionar alguns arquivos ou você escreveu a mensagem do commit de forma equivocada. Se você quiser refazer este commit, execute o commit novamente usando a opção `--amend`:

```cmd
git commit --amend
```

Esse comando pega a área stage e a usa para realizar o commit. Se você não fez nenhuma alteração desde o último commit (por exemplo, se você executar o comando imediatamente depois do commit anterior), então sua imagem dos arquivos irá ser exatamente a mesma, e tudo o que você alterará será a mensagem do commit.

O mesmo editor de mensagens de commit é acionado, porém o commit anterior já possui uma mensagem. Você pode editar a mensagem como sempre, porém esta sobrescreve a mensagem do commit anterior.

Por exemplo, se você fizer um commit e então lembrar que esqueceu de colocar no stage as modificações de um arquivo que você quer adicionar no commit, você pode fazer algo semelhante a isto:

```cmd
$ git commit -m 'initial commit'
$ git add forgotten_file
$ git commit --amend
```

No final das contas você termina com um único commit – O segundo commit substitui o resultante do primeiro.

### Retirando um arquivo do Stage

Por exemplo, vamos supor que você alterou dois arquivos, e deseja realizar o commit deles separadamente, porém você acidentalmente digitou `git add *` adicionando ambos ao `stage`. Como você pode retirar um deles do stage? O comando `git status` lhe lembrará de como fazer isso:

```cmd
$ git add *
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    renamed:    README.md -> README
    modified:   CONTRIBUTING.md
```

Logo abaixo do texto “Changes to be committed”, diz git reset HEAD `<file>`... para retirar o arquivo do stage. Então, vamos usar esta sugestão para retirar o arquivo `CONTRIBUTING.md` do stage:

```cmd
$ git reset HEAD CONTRIBUTING.md
Unstaged changes after reset:
M	CONTRIBUTING.md
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    renamed:    README.md -> README

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   CONTRIBUTING.md
```

O comando é um tanto quanto estranho, mas funciona. O arquivo CONTRIBUTING.md volta ao estado modificado porem está novamente fora do stage.

## Branches (Ramificações)

Ramificação significa que você diverge da linha principal de desenvolvimento e continua a trabalhar sem alterar essa linha principal. Em muitas ferramentas versionamento, este é um processo um tanto difícil, geralmente exigindo que você crie uma nova cópia do diretório do código-fonte, o que pode demorar muito em projetos maiores.

Um branch no Git é simplesmente um ponteiro móvel para um desses commits. O nome do branch padrão no Git é master. Conforme você começa a fazer commits, você recebe um branch master que aponta para o último commit que você fez. Cada vez que você faz um novo commit, ele avança automaticamente.

### Criando um Novo Branch

O que acontece se você criar um novo branch? Bem, fazer isso cria um novo ponteiro para você mover. Digamos que você crie um novo branch chamado: `testing`. Você faz isso com o comando git branch :

```cmd
git branch testing
```

Isso cria um novo ponteiro para o mesmo commit em que você está atualmente.

![Alt text](https://git-scm.com/book/en/v2/images/two-branches.png "two-branches")

Como o Git sabe em qual branch você está atualmente? Ele mantém um ponteiro especial chamado HEAD. Note que isso é muito diferente do conceito de HEAD em outros sistemas de versionamento com os quais você pode estar acostumado, como Subversion ou CVS. No Git, isso é um ponteiro para o branch local em que você está. Neste caso, você ainda está em master. O comando git branch apenas criou um novo branch - ele não mudou para aquele branch.

![Alt text](https://git-scm.com/book/en/v2/images/head-to-master.png "head-to-master")

Você pode ver isso facilmente executando um simples comando git log que mostra para onde os ponteiros do branch estão apontando. Esta opção é chamada de `--decorate`.

```cmd
$ git log --oneline --decorate
f30ab (HEAD -> master, testing) add feature #32 - ability to add new formats to the central interface
34ac2 Fixed bug #1328 - stack overflow under certain conditions
98ca9 The initial commit of my project
```

### Alternando entre Branches

Para mudar para um branch existente, você executa o comando git checkout. Vamos mudar para o novo branch testing:

```cmd
git checkout testing
```

Isso move o HEAD e o aponta para o branch` testing`.

![Alt text](https://git-scm.com/book/en/v2/images/head-to-testing.png "head-to-testing")

O que isso significa? Bem, vamos fazer outro commit:

```cmd
$ vim test.rb
$ git commit -a -m 'made a change'
```

![Alt text](https://git-scm.com/book/en/v2/images/advance-testing.png "advance-testing")


Isso é interessante, porque agora seu branch testing avançou, mas seu branch` master` ainda aponta para o commit em que você estava quando executou `git checkout` para alternar entre os branches. Vamos voltar para o branch master:

```cmd
git checkout master
```

![Alt text](https://git-scm.com/book/en/v2/images/checkout-master.png "checkout-master")

Esse comando fez duas coisas. Ele moveu o ponteiro HEAD de volta para apontar para o branch master, e reverteu os arquivos em seu diretório de trabalho de volta para o snapshots para o qual` master` aponta. Isso também significa que as alterações feitas a partir deste ponto irão divergir de uma versão mais antiga do projeto. Essencialmente, ele retrocede o trabalho que você fez em seu branch testing para que você possa ir em uma direção diferente.

Vamos fazer algumas mudanças e confirmar novamente

```cmd
$ vim test.rb
$ git commit -a -m 'made other changes'
```

Agora o histórico do seu projeto divergiu (consulte Histórico de diferenças). Você criou e mudou para um branch, fez algum trabalho nele e, em seguida, voltou para o seu branch principal e fez outro trabalho. Ambas as mudanças são isoladas em branches separados: você pode alternar entre os branches e mesclá-los quando estiver pronto. E você fez tudo isso com comandos simples `branch`,` checkout` e `commit`.

![Alt text](https://git-scm.com/book/en/v2/images/advance-master.png "advance-master")

Você também pode ver isso facilmente com o comando `git log`. Se você executar `git log --oneline --decorate --graph --all`, ele mostrará o histórico de seus commits, exibindo onde estão seus ponteiros de branch e como seu histórico divergiu.

```cmd
$ git log --oneline --decorate --graph --all
* c2b9e (HEAD, master) made other changes
| * 87ab2 (testing) made a change
|/
* f30ab add feature #32 - ability to add new formats to the
* 34ac2 fixed bug #1328 - stack overflow under certain conditions
* 98ca9 initial commit of my project
```

Como um branch no Git é na verdade um arquivo simples que contém a verificação SHA-1 de 40 caracteres do commit para o qual ele aponta, branches são fáceis para criar e destruir. Criar um novo branch é tão rápido e simples quanto escrever 41 bytes em um arquivo (40 caracteres e uma nova linha).

##O básico de Ramificação (Branch) e Mesclagem (Merge)

### Ramificação Básica

Primeiramente, digamos que você esteja trabalhando em seu projeto e já tenha alguns commits no branch master.

![Alt text](https://git-scm.com/book/en/v2/images/basic-branching-1.png "basic-branching-1")

Você decidiu que você vai trabalhar no chamado #53 em qualquer que seja o sistema de gerenciamento de chamados que a sua empresa usa.

Para criar um novo branch e mudar para ele ao mesmo tempo, você pode executar o comando `git checkout` com o parâmetro `-b`:

```cmd
$ git checkout -b iss53
Switched to a new branch "iss53"
```

Esta é a abreviação de:

```cmd
$ git branch iss53
$ git checkout iss53
```

![Alt text](https://git-scm.com/book/en/v2/images/basic-branching-2.png "basic-branching-2")

Você trabalha no seu website e adiciona alguns commits.

Ao fazer isso, você move o branch iss53 para a frente, pois este é o branch que está selecionado, ou checked out(isto é, seu HEAD está apontando para ele):

```cmd
$ vim index.html
$ git commit -a -m 'Create new footer [issue 53]'
```

![Alt text](https://git-scm.com/book/en/v2/images/basic-branching-3.png "basic-branching-3")

Agora você recebe a ligação dizendo que há um problema com o site, e que você precisa corrigí-lo imediatamente. Com o Git, você não precisa enviar sua correção junto com as alterações do branch `iss53` que já fez. Você também não precisa se esforçar muito para desfazer essas alterações antes de poder trabalhar na correção do erro em produção. Tudo o que você precisa fazer é voltar para o seu branch master.

Entretanto, antes de fazer isso, note que se seu diretório de trabalho ou stage possui alterações ainda não commitadas que conflitam com o branch que você quer usar, o Git não deixará que você troque de branch. O melhor é que seu estado de trabalho atual esteja limpo antes de trocar de branches.

```cmd
$ git checkout master
Switched to branch 'master'
```

Neste ponto, o diretório de trabalho de seu projeto está exatamente da forma como estava antes de você começar a trabalhar no chamado #53, e você pode se concentrar na correção. 

Seu próximo passo é fazer a correção necessária; Vamos criar um branch chamado `hotfix` no qual trabalharemos até a correção estar pronta:

```cmd
$ git checkout -b hotfix
Switched to a new branch 'hotfix'
$ vim index.html
$ git commit -a -m 'Fix broken email address'
[hotfix 1fb7853] Fix broken email address
 1 file changed, 2 insertions(+)
```

![Alt text](https://git-scm.com/book/en/v2/images/basic-branching-4.png "basic-branching-4")

Você pode executar seus testes, se assegurar que a correção está do jeito que você quer, e finalmente mesclar o branch `hotfix` de volta para o branch `master` para poder enviar para produção. Para isso, você usa o comando `git merge` command:

```cmd
$ git checkout master
$ git merge hotfix
Updating f42c576..3a0874c
Fast-forward
 index.html | 2 ++
 1 file changed, 2 insertions(+)
```

Você vai notar a expressão “fast-forward” nesse merge. Já que o branch `hotfix` que você mesclou aponta para o commit `C4` que está diretamente à frente do commit `C2` no qual você está agora, o Git simplesmente move o ponteiro para a frente. Em outras palavras, quando você tenta mesclar um commit com outro commit que pode ser alcançado por meio do histórico do primeiro commit, o Git simplifica as coisas e apenas move o ponteiro para a frente porque não há nenhum alteração divergente para mesclar — isso é conhecido como um merge “fast-forward.”

Agora, a sua alteração está no snapshot do commmit para o qual o branch `master` aponta, e você você enviar a correção.

![Alt text](https://git-scm.com/book/en/v2/images/basic-branching-5.png "basic-branching-5")


Assim que a sua correção importantíssima é entregue, você já pode voltar para o trabalho que estava fazendo antes da interrupção. Porém, você irá antes excluir o branch `hotfix`, pois ele já não é mais necessário — o branch `master` aponta para o mesmo lugar. Você pode remover o branch usando a opção `-d` com o comando `git branch`:

```cmd
$ git branch -d hotfix
Deleted branch hotfix (3a0874c).
```

Agora você pode retornar ao branch com seu trabalho em progresso na issue #53 e continuar trabalhando.

```cmd
$ git checkout iss53
Switched to branch "iss53"
$ vim index.html
$ git commit -a -m 'Finish the new footer [issue 53]'
[iss53 ad82d7a] Finish the new footer [issue 53]
1 file changed, 1 insertion(+)
```

![Alt text](https://git-scm.com/book/en/v2/images/basic-branching-6.png "basic-branching-6")


É importante frisar que o trabalho que você fez no seu branch `hotfix` não está contido nos arquivos do seu branch `iss53`. Caso você precise dessas alterações, você pode fazer o merge do branch `master` no branch `iss53` executando `git merge master`, ou você pode esperar para integrar essas alterações até que você decida mesclar o branch `iss53` de volta para master mais tarde.

### Mesclagem Básica

Digamos que você decidiu que o seu trabalho no chamado `#53` está completo e pronto para ser mesclado de volta para o branch `master`. Para fazer isso, você precisa fazer o merge do branch `iss53`, da mesma forma com que você mesclou o branch `hotfix` anteriormente. Tudo o que você precisa fazer é mudar para o branch que receberá as alterações e executar o comando `git merge`:

```cmd
$ git checkout master
Switched to branch 'master'
$ git merge iss53
Merge made by the 'recursive' strategy.
index.html |    1 +
1 file changed, 1 insertion(+)
```

Isso é um pouco diferente do merge anterior que você fez com o branch `hotfix`. Neste caso, o histórico de desenvolvimento divergiu de um ponto mais antigo. O Git precisa trabalhar um pouco mais, devido ao fato de que o commit no seu branch atual não é um ancestral direto do branch cujas alterações você quer integrar. Neste caso, o Git faz uma simples mesclagem de três vias (three-way merge), usando os dois snapshots referenciados pela ponta de cada branch e o ancestral em comum dos dois.

![Alt text](https://git-scm.com/book/en/v2/images/basic-merging-1.png "basic-merging-1")

Ao invés de apenas mover o ponteiro do branch para a frente, o Git cria um novo snapshot que resulta desse merge em três vias e automaticamente cria um novo commit que aponta para este snapshot. Esse tipo de commit é chamado de commit de merge, e é especial porque tem mais de um pai.

![Alt text](https://git-scm.com/book/en/v2/images/basic-merging-2.png "basic-merging-2")

Agora que seu trabalho foi integrado, você não precisa mais do brnach `iss53`. Você pode encerrar o chamado no seu sistema e excluir o branch:

```cmd
git branch -d iss53
```
### Conflitos Básicos de Merge

De vez em quando, esse processo não acontece de maneira tão tranquila. Se você mudou a mesma parte do mesmo arquivo de maneiras diferentes nos dois branches que você está tentando mesclar, o Git não vai conseguir integrá-los de maneira limpa. Se a sua correção para o problema #53 modificou a mesma parte de um arquivo que também foi modificado em `hotfix`, você vai ter um conflito de merge que se parece com isso:

```cmd
$ git merge iss53
Auto-merging index.html
CONFLICT (content): Merge conflict in index.html
Automatic merge failed; fix conflicts and then commit the result.
```

O Git não criou automaticamente um novo commit de merge. Ele pausou o processo enquanto você soluciona o conflito. Para ver quais arquivos não foram mesclados a qualquer momento durante um conflito de merge, você pode executar `git status`:

```cmd
$ git status
On branch master
You have unmerged paths.
  (fix conflicts and run "git commit")

Unmerged paths:
  (use "git add <file>..." to mark resolution)

    both modified:      index.html

no changes added to commit (use "git add" and/or "git commit -a")
```

Qualquer arquivo que tenha conflitos que não foram solucionados é listado como unmerged("não mesclado"). O Git adiciona símbolos padrão de resolução de conflitos nos arquivos que têm conflitos, para que você possa abrí-los manualmente e solucionar os conflitos. 

Se você quiser usar uma ferramenta gráfica para resolver os conflitos, você pode executar `git mergetool`, que inicia uma ferramente de mesclagem visual apropriada e guia você atravès dos conflitos:

```cmd
$ git mergetool

This message is displayed because 'merge.tool' is not configured.
See 'git mergetool --tool-help' or 'git help config' for more details.
'git mergetool' will now attempt to use one of the following tools:
opendiff kdiff3 tkdiff xxdiff meld tortoisemerge gvimdiff diffuse diffmerge ecmerge p4merge araxis bc3 codecompare vimdiff emerge
Merging:
index.html

Normal merge conflict for 'index.html':
  {local}: modified file
  {remote}: modified file
Hit return to start merge resolution tool (opendiff):
```

Após você sair da ferramenta, o Git pergunta se a operação foi bem sucedida. Se você responder que sim, o Git adiciona o arquivo ao stage para marcá-lo como resolvido. Você pode executar `git status` novamente para verificar que todos os conflitos foram resolvidos:

```cmd
$ git status
On branch master
All conflicts fixed but you are still merging.
  (use "git commit" to conclude merge)

Changes to be committed:

    modified:   index.html
```

Se você estiver satisfeito e verificar que tudo o que havia conflitos foi testado, você pode digitar `git commit` para finalizar o commit. A mensagem de confirmação por padrão é semelhante a esta:

```cmd
Merge branch 'iss53'

Conflicts:
    index.html
#
# It looks like you may be committing a merge.
# If this is not correct, please remove the file
#	.git/MERGE_HEAD
# and try again.


# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
# On branch master
# All conflicts fixed but you are still merging.
#
# Changes to be committed:
#	modified:   index.html
#
```

### Gestão de Branches

O comando `git branch` faz mais do que apenas criar e excluir branches. Se você executá-lo sem argumentos, obterá uma lista simples de seus branches atuais:

```cmd
$ git branch
  iss53
* master
  testing
```

Observe o caractere `*` que no início do master: ele indica o branch que você fez check-out (ou seja, o branch para o qual HEAD aponta). Isso significa que se você fizer commit neste ponto, o branch master será movido para frente com seu novo trabalho. Para ver o último commit em cada branch, você pode executar `git branch -v`:

```cmd
$ git branch -v
  iss53   93b412c Fix javascript issue
* master  7a98805 Merge branch 'iss53'
  testing 782fd34 Add scott to the author list in the readme
```
As opções `--merged` e `--no-merged` podem filtrar esta lista para branches que você tem ou ainda não mesclou no branch em que está atualmente. Para ver quais branches já estão mesclados no branch em que você está, você pode executar `git branch --merged`:

```cmd
$ git branch --merged
  iss53
* master
```

Como você já mesclou o `iss53` anteriormente, você o vê na sua lista. Branches que aparecem na lista sem o `*` na frente deles geralmente podem ser deletados com `git branch -d`; você já incorporou o trabalho deles em outro branch, então não vai perder nada.

Para ver todos os branches que contêm trabalhos que você ainda não mesclou, você pode executar `git branch --no-merged`:

```cmd
$ git branch --no-merged
  testing
```
Isso mostra seu outro branch. Por conter trabalho que ainda não foi mesclado, tentar excluí-lo com git branch -d irá não irá executar:

```cmd
$ git branch -d testing
error: The branch 'testing' is not fully merged.
If you are sure you want to delete it, run 'git branch -D testing'.
```

Se você realmente deseja excluir o branch e perder esse trabalho, pode forçá-lo com `-D`, como mostra a mensagem.