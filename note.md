# Git 入門コマンドライン演習80

## 第7章 基本を学ぼう
### 7.1 コマンドの基礎を学ぼう
- p.76 git push --set-upstream option がわからない。 → わかったかも
    - 通常、 git push コマンドには、リモートリポジトリ名とリモートブランチ名を引数として指定する必要がある。
        ```
        git push origin develop
        ```
    - ただし、あるローカルブランチに対して、リモートリポジトリのブランチを「上流ブランチ」として設定しておくと、上記の引数が省略できるようになる。
        ```
        git push -u origin develop # origin develop を次回以降省略できる
        ```

- SSH 接続で、git push するときに毎回 passphrase を求められるのが面倒だった。
    - [このページ](https://parashuto.com/rriver/tools/github-push-asks-passphrase-every-time) の対処を実行してみたところ、一旦解決したらしい。
    - 書いてある通りに以下のコマンドを、理解しないまま打った。
        - ssh-add -K ~/.ssh/id_rsa

### 7.2 履歴に強くなろう①
- p.81 git switch \<branch name\>
    - ブランチを、 \<branch name\> に切り替える。
    - ブランチの切り替えは、これまでずっと checkout を使っていた。

- p.82 git switch -c \<new branch name\>
    - ブランチの作成と切り替えを同時に行う。
    - これまでは git checkout -b \<branch name\> を使っていた。

- p.84 git merge \<branch name\>
    - 現在いるブランチに、指定したブランチをマージする。

### 7.3 ブランチの理解を深めよう①
- p.92 git diff --cached / git diff --staged
    - ステージングエリアとローカルリポジトリとの差分を確認することができる。
    - つけるオプションはどちらでもいいらしい（細かい違いがある？）

    - git status で表示されるもの
        - Changes to be committed: ← インデックス（ステージング）エリア
        - Changes not staged for commit: ← ワーキングツリー
        - Untracked files: ← それ以外


- p.93 git log --oneline
    - コミットログを省略して、1コミット1行で表示するオプション。
    - とても便利な一方で、適切なコミットメッセージが求められるなと思った。

## 第8章 基本を復習しよう
### 8.1 一連の流れをおさえよう
### 8.2 コンフリクトを解消しよう

# 第3部 Git を使いこなそう
## 第9章 習得しよう
### 9.1 ブランチの理解を深めよう②
- p.108
    - git branch --merged
        - 現在のブランチにマージされているブランチを出力する
    - git branch --no-merged
        - 現在のブランチにマージされていないブランチを出力する
    - git branch -d \<branch name\>
        - 指定したブランチを削除する（現在いるブランチにマージされていない場合は、警告が出て失敗する）
    - git branch -D \<branch name\>
        - 指定したブランチを削除する（現在いるブランチにマージされていなくても、削除を実行する）


- p.110
    - git branch -m \<branch before\> \<branch after\>
        - ブランチの名前を変更する。
        - \<branch before\> を省略した場合、現在いるブランチの名前を変更する。
        - すでに同名のブランチが存在する場合、変更は失敗する
    - git branch -M \<branch before\> \<branch after\>
        - ブランチの名前を変更する。
        - すでに同名のブランチが存在する場合でも、変更を実行する。
        - 前に存在した同名のブランチは、存在が消える。

- p.112
    - git fetch \<remote repository\> \<remote branch\>
        - リモートリポジトリのリモートブランチの情報をローカルにコピーする。
        - リモートリポジトリの名前には、 大抵、origin というエイリアスが張られている。

    - git fetch
        - リモートリポジトリ名とリモートブランチ名を省略した場合、リモートブランチの情報をすべてローカルにコピーする。

- p.113 (20) 特定のコミットを取り入れる
    - マージする = コミットの集合を取り入れる
    - cherry-pick = 特定のコミットのみを取り入れる

    - cherry-pick によってコンフリクトが発生した場合、選択肢は2つ
        - git cherry-pick --abort コマンドで、取り込みを中止する
        - ファイル編集でコンフリクトを解消した後、 git add コマンドでステージングし、 git cherry-pick --continue コマンドで確定する
            - （エディタが開くので、コミットメッセージを入力する）

    - git cherry-pick \<commit\>
    - git cherry-pick \<start commit\>..\<end commit\>
        - 始点として指定したコミットの、次のコミットの変更から取り入れられる。
    - git cherry-pick \<branch name\>
    - git cherry-pick --continue
    - git cherry-pick --abort

- p.117 演習21 コミットからブランチを作成する
    - git branch \<branch name\> \<commit\>
    - git switch -c \<branch name\> \<commit\>
        - ブランチ名を指定しない場合は、 detouched HEAD という状態になるらしい。（詳細は後述）

        - 自分の理解が正しければ、編集履歴は、「コミット」を頂点、「コミットとコミットの前後関係」を辺とする有向グラフとして考えられる。
        - それぞれの「コミット」は、「変更差分」もしくは「編集内容」で、コミットが辺で繋がることによって、「編集履歴」を形成している。
        - 「ブランチ」は、連なったり分岐したりしている「コミットのツリー」のいずれかの頂点を指している。
        - 通常の状態の HEAD は、いずれかのブランチを指している。

### 9.2 操作を取り消そう①
- p.120 演習22 変更を取り消す①
    - git restore \<file name\>
    - git restore .
        - ステージングされていない状態のワーキングツリーの変更を取り消す。
            - （git add する前の変更を取り消すことができる。）

- p.122 演習23 コミットを修正する
    - git commit --amend
    - git commit --amend -m \<commit message\>
        - 直前のコミットのコミットメッセージを修正する。

- p.123 演習24 変更を取り消す②
    - git reset HEAD
    - git reset
    - git reset -- \<file naem\>
        - ステージングしている変更を、ステージング前に戻す。
        - 引数を省略した場合は、デフォルトで HEAD が補われる。

- p.126 演習25 変更を取り消す③
    - git reset --soft HEAD^
        - コミットをインデックスエリアまで戻す。
        - ^ （カレット) は、 HEAD のいくつ前に戻すかを表す。上の場合は1つ前。
        - コミットを取り消す。
            - コミットとは、インデックスエリアの変更をローカルリポジトリに反映させること。
            - コミットを取り消すと、変更がインデックスエリアに戻る。
        - 「git reset コマンドの --soft オプションは HEAD 飲みを変更し、 HEAD のみを前のコミットに戻しています。」
             - → HEAD の理解が甘いために、うまく飲み込めず。

- p.127 演習26 変更を取り消す④
    - git reset --hard HEAD^
        - 変更前まで戻す
    - git reset --mixed HEAD  # デフォルト
        - ワーキングツリー まで戻す
    - git reset --soft HEAD^
        - インデックスエリア まで戻す

### 9.3 履歴に強くなろう②
- p.131 演習27 コミットを確認する②
    - git log のオプション
            --oneline   各コミットを1行で出力
            --graph  グラフで表示
            --all   すべてのブランチ
            --pretty フォーマット（プレースホルダーは色々ある）

- p.133 演習28 ファイルの差分を確認する②
    - git diff \<branch name1\> \<branch name2\> \<file name\>
        - 指定したファイルの、ブランチ間の差分を出力
        - 2つのブランチは .. で繋ぐことでも指定できる
        - ファイルを省略した場合は、ブランチ間の差分が単純に出力される（すべてのファイル以上の含意がある？）

- p.135 演習29 コミットの中身を確認する
    - git show
    - git show \<commit\>
    - git show \<commit\>:\<file name\>
        - コミットを指定しない場合は、直前のコミットの変更を出力する
            --oneline で簡略化できる

- p.138 演習30 作業履歴を確認する
    - git reflog
    - git reflog -\<作業履歴数\>
    - git reflog --pretty
        - コミットだけでなく、マージ、リセットなどの作業も追跡できるらしい。
        - HEAD を移動するような操作を出力している。
        - つまり、 HEAD に関する作業を追跡できる。

### 9.4 コマンドを組み合わせよう
- p.141 演習31 ファイルとディレクトリを削除する
    - git rm \<file name\>
        - ファイルを削除した上で、「削除」という変更をステージングまで行う。
        - 単純な rm コマンドを使うと、別途 git add が必要になる。
    - git rm -r \<directory name\>
        - ディレクトリを削除・ステージングするには、 -r オプションをつける。

- p.145 演習32 ファイル名とディレウトリ名を変更する
    - git mv \<before file\> \<after file\>
        - ファイル名を変更した上で、「変更(renamed)」という変更をステージングまで行う。
        - 単純な mv コマンドを使うと、削除したファイルと新規追加したファイルを、別途 git add する必要がある。
        - deleted: と added: に分かれるのではなく、 renamed というステータス(?)になる。
    - git mv \<before dir\> \<after dir\>
        - 同上。

- p.148 演習33 リモートブランチをまーじする
    - リモートには、 main ブランチが存在している。
    - ローカルには、 main ブランチと origin/main ブランチが存在している（とする）。
    - git fetch すると、リモートの main ブランチの情報を取ってきて、それを origin/main とする。
    - マージを行うことで、ローカルの main ブランチに、ローカルの origin/main ブランチの内容を反映する。

    - git fetch と git merge origin/main とを同時に行うコマンドが、 git pull origin main ということになる。

### 9.5 便利なコマンドを学ぼう
- (34)
    - 空のディレクトリを Git で管理する場合、 .keep, .gitkeep などのダミーファイルを作成する場合が多い。
    - Git は、ファイルを追跡するものなので、ディレクトリだけでは管理の対象とならない。
- (35)
    - git rebase \<branch name\>
        - merge コマンド以外でマージする方法。
        - 多分、ブランチを切った元の commit (base) を、マージするブランチの commit に付け替えるイメージ。
- (36) コミットを整理する
    - コミットをまとめる、という作業ができる。
    - git rebase -i \<commit\>
    - git log --online -\<出力数\>
    - git rebase --abort
        - 競合した際に作業を取りやめる
    - git rebase --continue
        - 競合を解消する
- (37) ファイル編集履歴を確認する
    - git blame \<file name\>
            -s : 編集者名、タイムスタンプを省略
            -L \<start\>,\<end\> : 特定の行の変更を確認


## 第10章 Git を上手に利用しよう
### 10.1 Git 開発を効率的に進めよう
38. 変更を無視する
- **.gitignore** というファイルに、追跡しないファイルを指定することができる。
- ワイルドカード ( * ) を利用することができる。
    - ワイルドカードは、0文字以上の任意の文字を表す。
- 四角かっこ ( [, ] ) で、括弧内のいずれか1文字とマッチする。
- ディレクトリ名を指定すると、ディレクトリごと追跡から外れる。
- エクスクラメーション ( ! ) をつけると、無視しないファイルとして指定できる。
- .gitignore ファイルの変更も、コミットしておく必要がある。

39. ローカルでのみ変更を無視する
    ```
    git update-index --skip-worktree <file name>
    git update-index --no-skip-worktree <file name>
    ```
- .gitignore は、すでに追跡されているファイルを除外するのには使えない。
    - 「追跡しているファイルを一時的にローカルでだけ変更したい」場合に使えない。
- git update-index コマンドに、 --skip-worktree オプションをつけると、一時的にファイルが追跡されなくなる。
- --no-skip-worktree オプションで、再度追跡できる。

40. エイリアスを作成する
    ```
    git config --global alias.<短縮コマンド> <コマンド>
    ```
- 「エイリアス」は「別名」「通称」などといった意味を持つ言葉。
- 例えば、 git branch を git br として登録する場合、以下
    ```
    git config --global alias.br branch
    ```
- 設定した内容は、 ~/.gitconfig に保存されている。 cat コマンドで確認できる。
    ```
    cat ~/.gitconfig
    ```

### 10.2 Git の理解を深めよう
41. リモートリポジトリを登録する
- 既存のローカルリポジトリに対して、github 上で作成したリポジトリをリモートリポジトリとして登録するコマンド（復習）
    ```
    git remote add <リモートリポジトリ名> <リポジトリのURL>
    ```
- 上の <リモートリポジトリ名> は、慣習的に origin とされることが多い。
- origin 以外にも、同様のコマンドでリモートリポジトリのエイリアスを追加することができる。
- 確認する場合は以下のコマンド。
    ```
    git remote -v
    ```
    - -v オプションで、URL つきで確認できる。

42. リポジトリ名を修正する
    ```
    git remote set-url <リモート名> <新しいリポジトリのURL>
    git remote -v
    ```
- github 上でリポジトリ名を変更した場合でも、ローカルでは以前のURLが残っている。
    - git push を叩いた場合、警告が出る。
- 警告を解消するために、上記のコマンドで、登録しているリポジトリ名を設定し直す。

43. ヘッドを理解する
- HEAD とは、今いるブランチを参照するもの（指差しているもの、というイメージかな？）
- HEAD には、いくつかの種類がある。
    - HEAD
    - FETCH_HEAD
    - ORIG_HEAD
    - MERGE_HEAD
- HEAD の情報は .git ディレクトリに保存されている。
    ```
    $ cat .git/HEAD
    ref: refs/heads/main
    ```
    - main ブランチを指していることが確認できる。
    ```
    $ ls .git/ | grep HEAD
    FETCH_HEAD
    HEAD
    ORIG_HEAD
    REBASE_HEAD
    ```
    - いくつかの種類があることも確認できる。
    ```
    $ cat .git/refs/heads/main
    cd048c4ec42ad4fccd760d73cd30e2ae9c3f89fd
    ```
    - HEAD の参照先として出力されたファイルの中身を確認すると、コミットIDが確認できる。
    ```
    $ git log -1
    commit cd048c4ec42ad4fccd760d73cd30e2ae9c3f89fd (HEAD -> main)
    Author: Ryohey <ryohey.trumpet+tech@gmail.com>
    Date:   Sun Aug 7 18:06:51 2022 +0900
        [update] note markdownize
    ```
    - 確かに一致している。
    - ブランチは、最新のコミットを参照している。

44. Git オブジェクトを理解する
    ```
    git hash-object --stdin
    git cat-file -p <commit>
    git cat-file -t <commit>
    ```
- Git は、 Git オブジェクトというオブジェクトで、ファイルやディレクトリを管理している。
- Git オブジェクトは、SHA-1 ハッシュ値で表され、 .git/objects/ 配下に保存される。
- オブジェクトは以下の種類に分けられる
    - Blob オブジェクト : ファイル情報を指したもの
    - Tree オブジェクト : Blob オブジェクトや Tree オブジェクトを指したもの
    - Commit オブジェクト : Tree オブジェクトを指したもので、加えて前の Commit オブジェクトを指しているもの
    - Tag オブジェクト : Commit オブジェクトを指したもの
- git hash-object コマンドに --stdin オプションをつけて、このコマンドにパイプでファイルの中身を渡してやると、そのファイルのオブジェクトのハッシュ値がわかる。
    - ハッシュ値の先頭2文字は、 .git/objects/ 以下のディレクトリ名を指し、残りがファイル名を指す。
    - ファイルの中身は、cat コマンドで確認できなかった（おそらくバイナリファイル）
