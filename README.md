# git_command
Hello mina-san!
便利なコマンドやオプションを書き溜めていきましょう。

## 宮下

## 上野

## 中田

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
untracked files = stagingに追加されているファイル。<br>
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
