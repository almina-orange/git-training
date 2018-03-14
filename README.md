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
git push origin [branch-name(or master)]

... GitHubで反映されていることを確認 ...
```

#### Step2: branchを切って編集
- ファイルを編集するときの基本的な手順
- branchが何なのかは調べること

```
# branchを作成，branchの切り替え
git brance [brance-name]
git checkout [branch-name]

... edit files ...

# branchに編集を反映
git status
git add .
git commit -m "[commit-message]"
git push origin [branch-name]
```

#### Step3: branchをmerge

#### Step4: 異なる端末からbranchで編集


## Snippets
```
git log --oneline --graph
```

---
## Reference
- Qiita - GitHub 入門
  https://qiita.com/ay3/items/8d758ebde41d256a32dc
- Qiita - 基本的なGitコマンドまとめ
  https://qiita.com/2m1tsu3/items/6d49374230afab251337
