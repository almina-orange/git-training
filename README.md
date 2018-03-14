# README: git-training
---

## Introduction
- GitHubのトレーニング用repository
- GitHubの基本的な使い方もメモ（詳しくは逐次調べる）
- GitLubでも応用できるはず（多分）


## What is GitHub?
- ソースコードのバージョン管理システム
- 基本的にソースコード保存用のドライブみたいに考えればよい（多分）
- 端末上のディレクトリ（local repository）で編集したものをpushすることで，GitHub上のディレクトリ（remote repository）に反映する
- pushやcommitなどの履歴（バージョン）管理，branch操作によって複数人で同時作業ができる


## Training
#### Step0: GitHubとの連携
* GitHub Accountを持っていることを前提とする．

```
# gitのバージョンを確認（入っていないならインストール）
git --version

# ディレクトリにlocal repositoryを作成
git init

# remote repositoryを登録（"origin"という名前をつけている）
git remote origin https://github.com/[username]/[repository-name]

# configを設定（.git/configでも編集可能）
git config --global user.name [username]
git config --global user.email [mail-address]
git config --list
```

#### Step1: GitHubにファイルをpushする
- localからremoteへの基本的な反映の手順
- （保存の仕方と捉えて構わない）

```
... GitHubで新しいrepositoryを作成する ...

# 作成したrepositoryをlocalに落とす（./[repository-name]というディレクトリができる）
git clone https://github.com/[username]/[repository-name].git
cd [repository-name]

# README.mdを作成し，remoteに反映
touch README.md
echo "# README" >> README.md
git status
git add README.md
git commit -m "[commit-message]"
git push origin [branch(or master)]

... GitHubで反映されていることを確認 ...
```

#### Step2: branchを切って編集
- ファイルを編集するときの基本的な手順
- branchが何なのかは調べること

```
# 現在のbranchを確認（基本は"master"）
git branch

# branchを作成，branchの切り替え
git brance [brance]
git checkout [branch]

... edit files ...

# branchに編集を反映
git status
git add .
git commit -m "[commit-message]"
git push origin [branch]
```

#### Step3: branchをmerge
- 編集をmaster branchに反映させる
- 編集が完了したら行う

```
# master branchで操作
git checkout master
git merge [branch]

# mergeされたことを確認
git log
git status

# 再びmasterをpush
git push origin master

# 必要がなければbranchを削除
git branch -d [branch]
git push origin :[branch]
```

#### Step4: 異なる端末からbranchで編集
- 必ず同期を行うように（`git pull origin [branch]`）

```
# 編集した分をbranchに反映する
git pull origin [branch]

... edit files ...

git status
git add .
git commit -m "[commit-message]"
git push origin [branch]
```


## Snippets
```
git log --oneline --graph    # ログをグラフ状かつ簡略表示
```

## Memo
- `pull`と`fetch + merge`がよく分かってない
- 基本的な扱い方がこれでいいのかも微妙
  - 基本はmasterを編集すべきなのか？
  - 異なる端末で同じbranchを扱うのは不自然？
  - branchを切るタイミングとかがよく理解できてない気がする

---
## Reference
- Qiita - GitHub 入門
  https://qiita.com/ay3/items/8d758ebde41d256a32dc
- Qiita - 基本的なGitコマンドまとめ
  https://qiita.com/2m1tsu3/items/6d49374230afab251337
