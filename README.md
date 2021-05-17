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

## 上野

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
