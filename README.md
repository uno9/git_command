# git_command
Hello mina-san!
便利なコマンドやオプションを書き溜めていきましょう。

## 宮下

### 1. pushについての小技

「git push origin <ブランチ名>」するときにブランチ名が必要で毎回、「git branch --contains」や「git branch」でブランチ名を探してpushしたことありませんか？

そんなときに「git push origin HEAD」を使えば自動的に自分の今いるブランチを指定してプッシュしてくれます。

https://reasonable-code.com/git-push-origin-head/
※HEADでもheadでも大文字、小文字関係なくいけます。

### 2. ailasについて

ailasは便利なので絶対設定してください！！
checkout, branchやcommitなどの文字を省略できる機能です。

```
設定前: git checkout master
設定後: git co master

設定前: git branch -D test
設定後: git br -D test
```

設定の仕方は色々ありますが、下記のサイトではワンラインで済むものがまとめられています。
https://dev.classmethod.jp/articles/git-config-alias-19/

### 3.　gitのTAB補完機能

gitのコマンド叩いてて、linuxみたいにコマンド補完できたらなあ〜って時に使えます。
全員やってください！笑

gitコマンドだけでなくブランチ名や、オプションもtabで入力補完できます。

```
$ cp /Library/Developer/CommandLineTools/usr/share/git-core/git-completion.bash /usr/local/etc/bash_completion.d/  (cpでgit-completion.bashを移動)
$ source /usr/local/etc/bash_completion.d/git-completion.bash  (sourceで適用)

(注意)/Library/Developer/CommandLineTools/usr/share/git-core/git-completion.bashに、git-completion.bashがない人はfindコマンドでどこにあるか探してくださいね。
```

## 上野

### cloneをして、作業する

※ clone後の特殊な状況で作業をするときに使うコマンド

1.前作業していたブランチでまた作業したい！
* 状況｜開発中にリポジトリがおかしくなって、再cloneをした

  ```
  git fetch
  git branch -a
  git checkout -b <作成したいブランチ名> origin/<作業したいブランチ名>
  ```

  sample:
  ```
    git fetch
    git branch -a
    * main
    remotes/origin/HEAD -> origin/main
    remotes/origin/add-introduction
    remotes/origin/alias
    remotes/origin/chida
    remotes/origin/chida_branch
    remotes/origin/feature/push
    remotes/origin/git_miss_command_tips
    (抜粋)
    git checkout git_miss_command_tips
  ```

  * コマンド説明
    * [git fetch](https://www-creators.com/archives/1272)|cloneしたリモートブランチの履歴を取得
    * git branch -a |リモートブランチに存在している全てのブランチ情報を表示
    * git checkout -b |ブランチを新規作成し、ブランチの移動をおこなう

2.cloneしたリポジトリのdevelopブランチから新しいブランチを作りたい
* 上記の別パターン

  ```
  git fetch
  git checkout -b <作成したいブランチ名> origin/develop
  ```

3.特定のブランチのみcloneする
* 状況｜cloneしたいブランチが決まっている場合に使用する
  * 特定のブランチのみのcloneになるため、取得するコミットの履歴数を抑えることができる
  * `git clone リポジトリ名 -b ブランチorタグ名`
* 参考｜[Gitで特定のブランチorタグをcloneする](https://qiita.com/iaoiui/items/fc318fa75cce3227b638)

### gitでミスをしたときの対処法

1.push先を間違えたけど、変更を残したまま、戻したい
```
git log
git reset --soft <commit id>
```

2.commitを間違えた！もどしたい！
* `git log -1`
  * logの内、最新の1件を表示する
* 変更したデータを**残した**まま**直前**のcommitを戻したい！
  * `git reset --soft HEAD^`
* [
git commit を取り消して元に戻す方法、徹底まとめ](http://www-creators.com/archives/1116#1_git_commit)

3.stashを間違えて削除した…戻したい…
* 削除するstashを間違えたときに、過去の変更データを捜索して戻す
  * stashなどのデータを読み込み
   `git fsck --no-reflog | awk '/dangling commit/ {print $3}' > dangling_commit`
  * 変更歴などを表示
    * グラフver
      `git log --graph --oneline --decorate --all $(cat dangling_commit)`
    * 列挙ver
      `git log --oneline $(cat dangling_commit) | grep 'WIP on'`
  * メッセージなどを手掛かりに目当てのstashデータを探す
  * 見つけたら、下記のコマンドで中身を確認
    * `git show <ハッシュ>`
  * データを発見したらハッシュをコピーして下記のコマンドにペースト
    * `git stash apply <ハッシュ>`
  * いらないファイルは削除
    * `rm dangling_commit`

* 実行すると、**×stashが生成される**のではなく**変更として復活**する
* 変更を選択して新たなstashを生成することで、元に戻せる
* [git stashを復元する](https://zenn.dev/snowcait/articles/7ba0720db50aea28c652)

* [Gitでやらかした時に使える19個の奥義](https://qiita.com/muran001/items/dea2bbbaea1260098051)

## 中田

### git resetの使い方一覧
- git reset HEAD <ファイル名>・・・ステージング登録取り消し
- git reset --hard HEAD^・・・直前のコミットを取り消す(ワークディレクトリの内容を書き換える)
- git reset --soft HEAD^・・・直前のコミットを取り消す(ワークディレクトリの内容をそのまま)

### HEADオプションについて
- HEAD^・・・複数ある親コミットのなかからコミットを指定できる
- HEAD~{n}・・・n個前のコミットを指定できる

### hardオプションの例を紹介
- hard HEAD@{n}・・・特定の時点までファイルを巻き戻す（nには戻りたい地点の数字を入力）

### git revertについて

- git revert <commit>・・・コミットIDを指定することで、そのコミットを打ち消すようなコミットが新しく追加させる。
- git revert <commit> -e・・・revertコミットを行なうときに、コミットメッセージの編集を行なうかどうかをオプションで指定できる。
- git revert <commit> -n・・・indexに戻すだけでコミットを行なわないようにすることもできる。
- git revert -m 1 <commit>・・・マージコミットを取り消そうとした場合、-mオプションで戻したい親を数字で指定できる。git showやgit logで親の数字がわかる。



## ハイン
<br>
git stash
------
どんな場で使うか？<br>
例えとして、作業Aの最中ですが急な作業Bに手をかけないといけない場合であれば、やりかけている作業Aが消えないようにどっかで一時的に保存してもらい、作業Bにすぐ入れるようになります。作業Bが終わり次第、できたところまでの作業Aを戻せるということを実現するためによく使用されている`git stash`コマンドです。<br>

1. 一時的に保存する
```
$ git stash
```
わかりやすくするため、以下のようにメッセージなども追加できる。
```
$ git stash save "add index.html file"
```
untracked files を強制的に一時的保存するために、`git stash -u`　を使う。　<br>
untracked files = stagingに追加されていないファイル。<br>
.gitignoreのファイルを保存するために `git stash -a`を使う。<br>

2. 一時的に保存さているものを表示する
```
$ git stash list
```
このようアウトプットが出てくる
```
stash@{0}: WIP on main: 5022a25 new homepage
stash@{1}: WIP on main: 5002d47 cancel button
stash@{2}: WIP on main: 5125f26 order cart
```
3. 一時的に保存されているファイルを戻す
```
$ git stash apply stash@{0}
```
{0} = 保存されているものの番号。 <br>

4. stashされたコードと最新のコードの違いを表示する
```
$ git stash show
```
5. 単一のファイル変更、またはファイル内からの個々の変更をstashしたい場合、`git stash -p` or `git stash -patch`を使う。作業コピーの各変更を繰り返し、それを隠したいかどうかを尋ねます。

## 千田

## 瀧野
### git resetについて

git resetには3種類のオプションがある

- git reset --soft
- git reset --mixed
- git reset --hard

#### 1. git reset --soft
直前の**コミットだけ**を消すコマンド  
「間違ってコミットしてしまった」「直前のコミット内容を修正したい」という時に使うと、コミット直前の状態にまで戻すことができる

#### 2. git reset --mixed
直前の**コミットとステージ**を巻き戻すコマンド  
「addした内容も取り消したい」という時に使うと、git addする前の状態まで戻ることができる  
作業ディレクトリの内容は残る

#### 3. git reset --hard
直前の**コミット・ステージ・作業ディレクトリ**を丸ごと消すコマンド  
「このコミット要らない」という時に使うと、コミットを丸ごと消すことができる


||作業ディレクトリ|ステージング(インデックス)|ローカルリポジトリ|
|--|--|--|--|
|soft|残る|残る|消える|
|mixed|残る|消える|消える|
|hard|消える|消える|消える|

#### 参考にしたもの
- [第6話 git reset 3種類をどこよりもわかりやすい図解で解説！【連載】マンガでわかるGit ～コマンド編～
マンガ・Git 連載・コラム スキルアップ
](https://www.r-staffing.co.jp/engineer/entry/20191129_1)

### 綺麗なPRを書こう
※このPRの書き方はRubyサブシステムチームで学んだことが主になってます。どのチームでも通用する内容ではないと思うのですが、個人的に分かりやすかったので共有します。

#### 1. 綺麗なPRとは？
誰が見ても（社内、社外問わず）、このPRでどんな変更を加えるのかが一目で分かるPR

#### 2. PRを綺麗に書くメリット
- レビュアーなど、PRを確認する人にとって必要な情報が詰まってるので分かりやすい
- 開発の背景、目的、ゴールなどを自分が再確認できる
- 実装やテストの抜け漏れを防ぐ
- 事前に開発の内容を細かく書くことで、予定工数が書きやすくなる

#### 3. 書く内容（**太文字**は必須）
- **issueの背景と目的**
- **関連するチケットのURL**
- **PRがマージされたときの状態（ゴール）**
- **開発方針**とその方針を選んだ理由
- PR内容を理解するために必要な情報（前提知識、修正に該当するページのスクショなど）
- **開発の具体的な内容**（チェックボックスを用いる）
- **テストの具体的な内容**（チェックボックスを用いる）
- レビューして欲しい箇所、不安な箇所
- 今回保留した内容

#### 4. 注意点
- 社外の方やチームメンバー以外の方にPRを見てもらう時は、前提知識などを分かりやすく書く
- 開発とテストの具体的な内容について
  - あらゆるケースを全て書き出し、ケースごとにとる対応を明記する
  - 開発の進捗を分かりやすくするため、開発終了したものにチェックを入れる
  - レビュー依頼する時には全てチェックが入っている状態にする
