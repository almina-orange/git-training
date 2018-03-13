# README: git-training
---
## Introduction
- GitHubのトレーニング用リポジトリ
- GitHubの基本的な使い方もメモ
- GitLubでも応用できるはず（多分）

## Training
#### Step1: GitHubにファイルをpushする
* GitHub Accountを持っていることを前提とする．

```
git clone https://github.com/[username]/[repository-name].git
cd [repository-name]
touch README.md
echo "# README" >> README.md
git add README.md
git commit -m "[commit-message]"
git push origin [branch-name(or master)]
```

#### Step2: branchを切って編集
```
git brance [brance-name]
git checkout [branch-name]

... edit files ...

git add .
git commit -m "[commit-message]"
git push origin [branch-name]
```

---
## Reference
- Qiita - GitHub 入門
  https://qiita.com/ay3/items/8d758ebde41d256a32dc
