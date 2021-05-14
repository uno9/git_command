# git_command
Hello mina-san!
便利なコマンドやオプションを書き溜めていきましょう。

## 宮下

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
