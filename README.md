# gitとgithub関連のCheatSheet

頻繁に忘れるのでメモとしてこのファイルに書いておく。

結構マジでgitの理解が浅かったと反省しているので[このチュートリアル](http://k.swd.cc/learnGitBranching-ja/)で学習し直す。

## 基本知識

- gitは差分を記録している。
- **コードの修正は常にトピックブランチを作成して行う**
  - トピックブランチをマージするようにプルリクエストを作成することテーマが明確になり、レビューしやすくなる。
- **早めに、かつ頻繁にブランチを切りなさい**
- Git にコミットした内容のすべては、ほぼ常に取り消しが可能であることを覚えておきましょう。 削除したブランチへのコミットや --amend コミットで上書きされた元のコミットでさえも復旧することができます (データの復元方法については データリカバリ を参照ください)。 しかし、まだコミットしていない内容を失ってしまうと、それは二度と取り戻せません。

## 用語集

Gitに関連する用語
### リポジトリ

ファイルやディレクトリの状態を記録する場所。
変更履歴を管理したいディレクトリをリポジトリの管理下に置くことで、そのディレクトリ内のファイルやディレクトリの変更履歴を記録することができます。

### コミット

- コミットを実行すると、リポジトリの内では、前回コミットした時の状態から現在の状態までの差分を記録したコミット(またはリビジョン)と呼ばれるものが作成されます。

- コミットには、コミットの情報から計算された重複のない英数字40桁の名前が付けられています。この名前を指定することで、リポジトリの中からコミットを指定することができます。

- バグ修正や機能追加などの異なる意味を持つ変更は、できるだけ分けてコミットするようにしましょう。後から履歴を見て特定の変更内容を探す時に探しやすくなります。


### チェックアウト

バージョン管理システムから、作業ディレクトリにファイルやディレクトリをコピーすること

### トピックブランチ

一つのトピックに集中して他の作業は一切行わないブランチのこと

### 統合ブランチ

トピックブランチの分岐元となり、併合する先となるブランチのこと。
通常はmainブランチである。(古いプロジェクトや文献だとmasterブランチになっていることが多い。)
いつでも他人に見せられるように中途半端な変更を含まない。
複数のバージョンリリースを扱う場合は統合ブランチは複数あることもある。

### 三つの状態

Gitは、ファイルが帰属する、コミット済、修正済、ステージ済の、三つの主要な状態を持ちます。 コミット済は、ローカル・データベースにデータが安全に格納されていることを意味します。 修正済は、ファイルに変更を加えていますが、データベースにそれがまだコミットされていないことを意味します。 ステージ済は、次のスナップショットのコミットに加えるために、現在のバージョンの修正されたファイルに印をつけている状態を意味します。

### 作業ディレクトリ(ワークツリー)とステージング・エリア(インデックス)

Gitでは、Gitの管理下に置かれた、実際に作業をしているディレクトリのことをワークツリーと呼びます

Gitではリポジトリとワークツリーの間にはインデックスというものが存在しています。インデックスとは、リポジトリにコミットする準備をするための場所のことです。

Gitでは、コミットを実行した時にワークツリーから直接リポジトリ内に状態を記録するのでなく、その間に設けられているインデックスの設定された状態を記録するようになっています。
そのため、コミットでファイルの状態を記録するためには、まずインデックスにファイルを登録する必要があります。

このようにインデックスを間に挟むことで、ワークツリー内の必要ないファイルを含めずにコミットを行ったり、ファイルの一部の変更だけをインデックスに登録してコミットすることができます。

### HEAD

HEADは今チェックアウトされているコミットのこと。

- `git checkout HEAD^`で直前のコミットに移動することができる。
  - `(特定のコミット)^`は"特定のコミット"の直前を表す。
- `git checkout HEAD~(移動したい数)`で現在のコミットから"移動したい数"だけ前のコミットに移動することができる。

### fast-forward merge

Git のファストフォワードマージとは, マージされるブランチの HEAD をマージするブランチの先端にそのまま移動させるマージを指します。

# コマンド集

## init(ブランチを初期化する)

- `git init`でブランチを初期化する。

## commit(コミットする)

- `git commit -am "(コミットメッセージ)"`で`git add .`,`git commit -m "(コミットメッセージ)"`と同じ挙動になる。
- - コミットメッセージは、他の人がコミットの変更内容を調べる場合や、自分で後から履歴を見直す際に大切な情報となるので、変更内容のわかりやすいコメントを書くように心がけましょう。 Gitでは標準的に

```
1行目 : コミットでの変更内容の要約
2行目 : 空行
3行目以降 : 変更した理由
```

という形式でコメントを書きます。

## status(ステータスを表示する)

- `git status` を使用すると、作業ツリー (およびステージング領域、ステージング領域については後で詳しく説明します) の状態が表示されます。 Git によって現在追跡されている変更がわかるので、別のスナップショットの取得を Git に指示するかどうかを判断できます。

## log(履歴を確認する)

- `git log`でcommitの履歴を確認できる。
- `git log --pretty=short`でコミットメッセージの一行目だけを確認できる。
- `git log (ファイル名)`で指定したファイルだけのログを表示できる。
- `git log -p (ファイル名)`でファイルの差分も確認できる。
- `git log --graph`で視覚的にわかりやすくグラフ形式でログが表示される。

## reflog(強力にログを確認する)

- `gir reflog`で`git log`では確認できないような、チェックアウトの履歴や`git reset --hard`で取り消したコミットなども確認することができる。

## branch(ブランチを切る)

- `git branch`でブランチの一覧を表示できる。
- `git branch (新規ブランチ名)`でブランチを切れる
- `git branch -f master HEAD~3`でmasterブランチをHEADから数えて3個前の親コミットへと強制的に移動することができる。
- `git branch -D (ブランチ名)`でブランチを削除することができる。



## checkout(ブランチ間を移動する)

- `git checkout (移動したいブランチ名)`でブランチ間を移動できる。
  - `git checkout -`で直前にいたブランチに移動することができる。
- `git checkout -b (新規ブランチ名)`でブランチの作成と移動を一括で行える。
  - `git branch (新規ブランチ名)`,`git checkout (新規ブランチ名)`を順に実行した場合と同じ。
- `git checkout -b (新規ブランチ名) (古いブランチ名)`で古いブランチからブランチの作成と移動を一括で行える。
- `git checkout (移動したいコミットのハッシュ値)`でコミットを指定して移動することができる。

## checkout -- (ファイル名) (ステージ前のファイルの変更を取り消す)

- `git checkout -- (ファイル名)`でファイルに加えた変更を取り消す。

ここで理解しておくべきなのが、`git checkout — [file]`は危険なコマンドだ、ということです。 あなたがファイルに加えた変更はすべて消えてしまいます。変更した内容を、別のファイルで上書きしたのと同じことになります。そのファイルが不要であることが確実にわかっているとき以外は、このコマンドを使わないようにしましょう。
## merge(安全に二つのブランチを統合する)

- 統合されるブランチに移動して`git merge (統合したいブランチ)`で統合されるブランチに統合したいブランチの変更を取り込むことができる。
- マージしたことを明確に記録したい場合は`git merge --no-ff (統合したいブランチ)`で例えfast-forwardマージであったとしてもマージコミットを作る。

## merge --squash(ブランチ上のコミットを一つにまとめてマージする)

少し特殊なmergeとして、squashオプションを紹介します。

このオプションを指定してブランチをマージすると、そのブランチのコミット全てをまとめたコミットが追加されます。

主な利用シーン

- トピックブランチ中のコミットを一つにまとめて統合ブランチに統合する。

## rebase(スッキリと二つのブランチを統合する)

- `merge`と違って統合されるブランチの履歴を書き換えるので明確な目的がない場合は`merge`を使用した方が安全

## rebase -i(コミットの履歴を書き換える)

rebaseにiオプションを指定すると、コミットの書き換え、入れ替え、削除、統合を行うことができます。

例えば`$ git rebase -i HEAD~~`を実行するとテキストエディタが開いて、HEADからHEAD~ ~までのコミットを修正することができます。

- pickをeditに変更すると修正するコミットにチェックアウトすることができます。
- その後、`git commit --amend`、`git rebase --continue`を行なう。
- この時、ほかのコミットで競合が発生することがあります。その時は、競合箇所を修正してからaddとrebase --continueを実行してください。このとき、コミットは必要ないので実行しないでください。
もし、途中でrebaseの作業を中止したくなった場合はrebaseに--abortオプションを指定して実行すると、これまでのrebaseでの作業をなかった事にして中止できます。
- pickをfixupに変更するとそのコミットは一つ上のコミットを修正するためのコミットと見做され一つ上のコミットとひとまとめにされて消滅します。
  - このコミットも`git reflog`を使えば確認できます。

主な利用シーン

- pushする前にコミットコメントをきれいに書きなおす
- 意味的に同じ内容のコミットをわかりやすいように一つにまとめる
- コミット漏れしたファイルを後から追加する

## reset(localのcommitを取り消す)

resetでは、要らなくなったコミットを捨てることができます。実行時に影響範囲によって異なるモードを指定することで、インデックスやワークツリーの内容も戻すかどうか指定できます。

- local上のcommitを取り消してなかったことにする。
- `git reset HEAD~3`で直近の3回分のコミットを取り消すことができる。

オプション

- 変更したインデックスの状態を元に戻す(mixed)
- 最近のコミットを完全に無かったことにする(hard)
- コミットだけを無かったことにする(soft)

## reset HEAD (ファイル名)　(ステージされたファイルのステージを解除する)

- `git reset HEAD (ファイル名)`でステージを解除できる
- `git restore --staged (ファイル名)`でも同様のことができる。(むしろ、公式はこちらを推奨してる嫌いがある。`git status`ではこちらが表示される。)

## revert(既に公開したコミットを安全に打ち消す)

- `git revert (取り消したいコミット)`で取り消したいコミットを打ち消すコミットが作成される。
- 特に直前のコミットを打ち消したい場合は`git revert HEAD`

## cherry-pick

- `git cherry-pick (取り込みたいコミットのハッシュ値)`で取り込みたいコミットの差分だけを取り込むことができる。

主な利用シーン

- ブランチを間違えて追加したコミットを正しい場所に移す
- 別ブランチのコミットを現在のブランチにも追加する

## stash

まだコミットしていない変更内容や新しく追加したファイルが、インデックスやワークツリーに残ったままで、他のブランチへのチェックアウトを行うと、その変更内容は元のブランチから、移動先のブランチに対して移動します。

ただし、移動先のブランチで、同じファイルが既に何らかの変更が行われている場合はチェックアウトに失敗します。このような場合は、変更内容を一度コミットするか、またはstashを使って一時的に変更内容を退避させてからチェックアウトしなければなりません。

stashとは、ファイルの変更内容を一時的に記録しておく領域です。stashを使うことで、ワークツリーとインデックス中でまだコミットされていない変更を一時的に退避させることができます。退避させた変更は後から取り出して、元のブランチや別のブランチに反映させることができます。

- `git stash`で現在の作業を一時的に退避
- `git stash list`で退避した作用の一覧を表示
- `git stash pop`で退避した作業の最新のものを復元
  - `git stash pop stash@{0}`などのように引数を指定することで特定の作業を復元できる。
  - 特定の作業を見つけるためには上記の`git stash list`を用いる。
- `git stash drop`で退避した作業の最新のものを削除
- `git stash clear`で退避した作業を全て削除

## add(ファイルを追加する)

- `git add (ファイル名)`でファイルもしくはフォルダをgit管理に追加することができる。`git add .`で全ての変更をstageすることができる。

## tag(コミットにタグをつける)

- `git tag -a (タグ名)`でコミットにタグをつけることができる。

## diff(差分を確認する)

- `git diff`でワークツリーとステージ領域の差分を確認できます。
- `git diff (コミット名)`で指定したコミットとワークツリーの差分を確認することができる。
  - `git diff HEAD`で最新コミットとワークツリーの差分を表示できる。
  - 特に`git diff origin/main`はpushする前に確認した方が良い。

## commit --amend(直前のコミットを上書き)

`git add`で必要な変更をstagingした後、`git commit --amend`で先ほど`git add`した変更を新しいコミットで上書きして取り込み一つのコミットに変える。

主な利用シーン

- 直前のコミット漏れしたファイルを後から追加する
- 直前のコミットコメントを修正する

## config(設定を行う)

設定した内容は`~/.gitconfig`に反映される。

- `git config --global user.name "(名前)"`でGitで利用する名前を登録する。
- `git config --global user.email "(Emailアドレス))"`でGitで利用するEmailアドレスを登録する。
## config --list(設定の一覧を確認する)

- `git config --list`で設定の一覧を確認することができる。



# Git Hub,リモートリポジトリ関連

## Tips

- 機能の実装途中であっても他の開発者のフィードバックを受けるためにもプルリクエストを活用すると良い。
- リポジトリのメンテナとしては動かないコードをリポジトリに入れるのはプログラマとしての信頼を失う行為だと思った方が良い。

## 用語集

### Pull Request

ソースコード付きのIssue。

- 開発中のプルリクエストは\[WIP\]とつけるのが慣例。WIP とは Work In Progress の略。

## コマンド集

### push(リモートリポジトリに変更を反映する)

- `git push origin (ブランチ名)`でリモートリポジトリに(ブランチ名)に変更を反映できる。
- ローカルリポジトリからリモートリポジトリにpushするときは、pushしたブランチがfast-forwardマージされるようにしておく必要があります。もし、競合が発生するような場合は、pushが拒否されます。
- リモートリポジトリで共有しているコミットは、基本的に書き換えてはいけません。もし書き換えてしまうと、そのリモートリポジトリと同期している他のリポジトリの履歴がおかしな状態になることがあります。

## pull(リモートリポジトリのファイルを取得してローカルにマージする)

- `git pull`でリモートリポジトリの変更をローカルに反映することができる。
- `git pull origin main`で現在作業しているブランチにmainブランチの変更を取り込むことができる。

## fetch(リモートブランチからマージせずに変更を取得する)

pullを実行すると、リモートリポジトリの内容のマージが自動的に行われてしまいます。しかし、単にリモートリポジトリの内容を確認したいだけの時はマージをしたくない場合もあります。そのような時はfetchを使用します。

fetchを実行すると、リモートリポジトリの最新の履歴の取得だけを行うことができます。
取得したコミットは、名前の無いブランチとして取り込まれます。このブランチはFETCH_HEADという名前でチェックアウトすることができます。

```
git fetch
git merge FETCH_HEAD
```

で`git pull`と同じ挙動となる。

### remote add(リモートリポジトリを追加する。)

- forkしたリポジトリをリモートリポジトリとして追加する場合は`git remote add (付けたい名前) git@github.com:(フォークしたリモートリポジトリの名前)`のようにして追加する。
  - 追加したリモートを最新の状態にするためには`git fetch (付けた名前)`のようにして行い。反映するためには`git merge (付けた名前)/(取り込みたいブランチ名)`のようにして行う。



