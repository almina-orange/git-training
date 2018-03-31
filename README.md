# README: git-training

## Introduction
- GitHubのトレーニング用repository
- GitHubの基本的な使い方もメモ（詳しくは逐次調べる）
- GitLabでも応用できるはず（多分）


## What is GitHub?
- ソースコードのバージョン管理システム
- 基本的にソースコード保存用のドライブみたいに考えればよい（多分）
- 端末上のディレクトリ（local repository）で編集したものをpushすることで，GitHub上のディレクトリ（remote repository）に反映する
- pushやcommitなどの履歴（バージョン）管理，branch操作によって複数人で同時作業ができる


## Training
### Step0: 基本の設定
GitHubにログインできることを前提とする．

1. GitHub上でremote repositoryを作成．
    - アカウントページを開く（`https://github.com/[user]`）
    - `New -> New repository`をクリック
    - `Repository name`に任意の名前を入力
    - `Description`にrepositoryの用途を入力
    - 他のユーザから見えないようにする場合は`Private`にチェック
    - `Initialize this repository with a README`をチェックすると，自動で`README.md`を作ってくれる
    - `Create repository`をクリックし，repositoryを作成

2. gitコマンドが使えることを確認．（入っていない場合はインストールする）

    ```bash
    $ git --version
    ```
3. remote repositoryとディレクトリ（local repository）を連携．
    - ディレクトリにlocal repositoryを作成

        ```bash
        $ git init
        ```
    - remote repositoryを登録（`origin`という名前をつけている）

        ```bash
        $ git remote origin https://github.com/[user]/[repository]
        ```
    - configを設定（`.git/config`で直接編集も可能）

        ```bash
        $ git config --global user.name [user]
        $ git config --global user.email [address]
        $ git config --list
        ```

### Step1: GitHubにファイルをpushする
編集したファイルをGitHubへ反映する手順（GitHubへの保存方法）．

```bash
... GitHubで新しいrepositoryを作成 ...

# 作成したrepositoryをlocalに落とす（./[repository]というディレクトリができる）
# cloneした場合は`git init`する必要がない
$ git clone https://github.com/[user]/[repository].git
$ cd [repository]

# README.mdを作成し，remoteに反映
$ touch README.md
$ echo "# README" >> README.md
$ git status
$ git add README.md
$ git commit -m "[message]"
$ git push origin [branch(or master)]

... GitHubを開き，反映されていることを確認 ...
```

### Step2: branchを切って編集
- ファイルを編集するときの基本的な手順．
- 編集する場合は必ずbranchを切る．
- branch名は，Git-flowを参考にする．

```bash
# 現在のbranchを確認（基本は"master"）
$ git branch
> * master

# 新しいbranchを作成，branchの切り替え
$ git branch [branch]
$ git checkout [branch]

# 現在のbranchを再確認
$ git branch
>   master
> * [branch]

... ファイルを編集 ...

# branchに編集を反映
$ git status
$ git add .
$ git commit -m "[message]"
$ git push origin [branch]

... GitHubを開き，反映されていることを確認
`master`には編集が反映されず，`[branch]`のみに編集が反映される ...
```

### Step3: branchをmasterにmerge
編集を`master`に統合させる．
基本的には，`[branch]`での編集が完了した後に行う．
branch同士の衝突（conflict）に注意すること．

- CLI上で行う場合

```bash
# masterで操作
$ git checkout master
$ git merge [branch]

# mergeされたことを確認
git log
git status

# 再びmasterをpush
$ git push origin master

# 必要がなければ[branch]を削除
$ git branch -d [branch]
$ git push origin :[branch]
```

- GUI（GitHub）上で行う場合
    1. `Pull Request`をクリック
    2. コメントを書いてリクエストを送信
    3. リクエストを受けた側は，リクエスト内容を確認した上でmergeする

### Step4: 異なる端末からbranchで編集
- 異なる端末間の同期の手順．
- 必ず同期を行ってから編集を行うこと（衝突の原因になる）．
- 基本的には「作業前に`pull [branch]`」「作業後に`push [branch]`」でいい．
- 以下の手順は，違うディレクトリを作って別々に編集し，同期を試してみると良い．

```bash
# 作業用branchに入る
$ git checkout [branch]

# remote repositoryをlocalに反映
$ git pull origin [branch]

... ファイルを編集 ...

# 編集した分をGitHubに反映
$ git status
$ git add .
$ git commit -m "[message]"
$ git push origin [branch]
```

- 補足: 異なる端末からの作業の始め方のススメ
    - 既にGitHub上にrepositoryがあることを想定する
    - また作業用branchがある場合を想定する

```bash
# remoteをlocalに落とす
$ git clone https://github.com/[user]/[repository].git
$ cd [repository]

# 作業用branchがあれば引っ張ってくる
# （remote上のbranch（origin/[branch]）をlocalの[branch]に反映させる）
$ git checkout -b [branch] origin/[branch]
```


## Snippets
役に立ちそうなコマンド集．

- ログをグラフ状に20行，かつ簡略表示

    ```bash
    $ git log --graph --oneline --decorate -20
    ```
- commit情報の表示

    ```bash
    $ git show HEAD    # 最新
    $ git show HEAD~2  # 最新から2つ前
    ```
- commitの取り消し

    ```bash
    $ git reset HEAD
    $ git reset --soft HEAD
    $ git reset --hard HEAD
    ```
- やり直し手順

    ```bash
    # 1. コミットを完全に取り消す
    $ git reset --hard [commit-ID]

    # 2. 無理やりremoteに反映させる
    $ git push -f origin [branch]
    ```

- やり直しのやり直し（消してしまったcommitの復活）

    ```bash
    # 1. 消してしまった全てのログを確認
    $ git reflog

    # 2. 戻したいcommitのIDを探してreset
    $ git reset --hard HEAD@{...}
    ```

- 特定のbranchのみクローンする

    ```bash
    $ git clone -b [branch] [URL]
    ```


## Notes
- `fetch`と`pull`の違いは知っておいた方がいいらしい．
- ワーキングツリー，インデックス，ローカルブランチ，リモートブランチはイメージできるようにすると良い．
- HEAD，FETCH_HEAD，MERGE_HEADも知っておくこと．
- **`.gitignore`を利用すれば，誤ってファイルを追加する心配がなくなるので使用することを推奨する．**
- branch同士の衝突を防ぐためにも，できる限り各branchは独立性を持たせること．
- branch名にはある程度ルールを持たせること．代表的な名前空間の例はGit-flowなどを参照．
- 衝突した場合は，基本的に以下の手順で解消することが多い．
    1. 先に[branch]へmasterをmergeする．
    2. 各所の衝突部分を手作業で修正．
    3. 改めてmasterへ[branch]をmergeする．


## Reference
- [Qiita - GitHub入門](https://qiita.com/ay3/items/8d758ebde41d256a32dc)
- [Qiita - 基本的なGitコマンドまとめ](https://qiita.com/2m1tsu3/items/6d49374230afab251337)
- [KRAY Inc - Git初心者に捧ぐ！Gitの「これなんで？」を解説します](http://kray.jp/blog/git-why-explanation/)
- [サルでもわかるGit入門](https://backlog.com/ja/git-tutorial/)
- [git初心者への道 - お仕事で困らないレベルまでググっとします。 - GitHub](https://gist.github.com/yatemmma/6486028)
