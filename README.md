# git_command
Hello mina-san!
便利なコマンドやオプションを書き溜めていきましょう。

## 宮下

## 上野

## 中田

## ハイン

## 千田 
### rebaseでできるあれこれ
#### ①rebaseでコミットをやり直す
* `$ git rebase -i <コミットIDかHEADの指定>`でコミットを表示する。（`git log` とは逆に表示される）
```
（例）

$ git rebase -i HEAD~4
pick 3ba3a2e commit comment 1
pick 6ef356e commit comment 2
pick d84a911 commit comment 3
pick 9a34966 commit comment 4

# Rebase f492a4e..9a34966 onto f492a4e (4 commands)
#
# Commands:
# Note that empty commits are commented out
#
# However, if you remove everything, the rebase will be aborted.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# d, drop = remove commit
# x, exec = run command (the rest of the line) using shell
# f, fixup = like "squash", but discard this commit's log message
# s, squash = use commit, but meld into previous commit
# e, edit = use commit, but stop for amending
# r, reword = use commit, but edit the commit message
# p, pick = use commit
```
* 修正したいコミットを`pick`から`edit`にする。（この時、editのところでコミットの適用が止まる）
* エディタでファイルの内容をやり直す
* `$ git commit --amend`でコミットメッセージを変更
* `$ git rebase --continue`で`edit`で止めていたコミットを適用させる。


### マージとリベースの使い分け（簡略版）
マージ：
* コンフリクトの解消が楽
* 履歴にマージコミットが残る

リベース：
*  履歴がスッキリ
* コンフリクトの解消がコミット毎だから少しめんどくさい

## 瀧野


