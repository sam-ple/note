## ⚠️ 前提注意

* **GitHub上のリポジトリは残す**
* **コードは残すが、履歴（コミットログ）は消す**
* **他の人と共有している場合は注意（履歴が強制的に書き換えられる）**

## 🔄 コミット履歴を全削除して初期化する手順

### 🔧 手順（ローカルでの操作）

1. **今のコードを保持したまま、履歴を削除**

   ターミナルでプロジェクトフォルダに移動して、以下のコマンドを実行：

   ```bash
   rm -rf .git
   git init
   git add .
   git commit -m "Initial clean commit"
   ```

   これで、**新しいGit履歴**が作成されました。

2. **GitHubリポジトリと再接続**

   ```bash
   git remote add origin https://github.com/ユーザー名/リポジトリ名.git
   ```

3. **リモートに強制プッシュ（履歴を完全に上書き）**

   ```bash
   git branch -M main
   git push -f origin main
   ```

   `-f`（または `--force`）が強制上書きのオプションです。

## ✅ 結果

* GitHub上のリポジトリはそのまま残ります。
* ファイル構成もそのまま。
* **コミット履歴は完全に1つだけにリセットされます（初期コミットのみ）。**

## 🛡️ バックアップ推奨

この操作は**履歴を完全に破棄**するので、念のために現在のプロジェクトフォルダをコピーしておくことをおすすめします。

* * *

## 🔄 目的：GitHubの「Deployments」履歴をリセットしたい

### ① **GitHub Actions のデプロイ履歴をリセット**

GitHub Actions を使っていた場合：

#### ✅ 方法

1. **リポジトリ設定**へ（Settings → Actions → General）
2. 下へスクロールして「**Artifacts and Logs**」の中の
   　`Delete all logs and artifacts for this repository` をクリック
3. これによりすべての **Workflowログや成果物が削除される**

> ⚠️ 注意：GitHubの「Deployments」タブには残る場合があります。

### ② **Pagesのデプロイ履歴をリセット（GitHub Pages）**

#### ✅ 方法1：`gh-pages` ブランチを削除＆再作成

```bash
git push origin --delete gh-pages
git checkout --orphan gh-pages
git rm -rf .
echo "reset" > index.html
git add .
git commit -m "reset pages"
git push origin gh-pages
```

これにより GitHub Pages の履歴は **新しい1件の履歴に置き換わる**。

### ③ **Vercel / Netlify など外部サービス連携の場合**

#### ✅ Vercel の場合

* Vercelのダッシュボードから該当プロジェクトを選択
* 「Deployments」→ 手動で各デプロイを削除
* 必要なら「プロジェクト自体を削除」して再作成

#### ✅ Netlify の場合

* Netlifyのダッシュボードで「Deploys」→ 各デプロイを削除

## 🚫 GitHubの「Deployments」タブ自体の履歴は消せる？

GitHubの「Deployments」タブに出る履歴（特に **`environment` に紐づいたもの**）は、**GitHub API** で削除が可能です。

### ✅ GitHub API を使った削除例（環境ごと）

```bash
curl \
  -X DELETE \
  -H "Authorization: token YOUR_GITHUB_TOKEN" \
  https://api.github.com/repos/USERNAME/REPO/deployments/DEPLOYMENT_ID
```

> ただし、「`active`なデプロイがある環境」は削除できないため、まず `status` を `inactive` に変更してから削除します。

## 🔁 最終手段：リポジトリを新しく作り直す

どうしても完全にクリーンにしたい場合は：

1. 新しいリポジトリを作成
2. 必要なファイルだけをコピー
3. `git init` → 新しい履歴で再スタート

* * *

## ✅ 方法：新しいブランチでデプロイ履歴をリセットする手順

### 🎯 目的

* 今のリポジトリは残す
* 現在のブランチの履歴やデプロイ履歴は触らず
* **新しいクリーンなブランチからデプロイ開始**

### 💡 ステップ 1：空の履歴で新ブランチ作成（`orphan`）

```bash
git checkout --orphan reset-branch
git rm -rf .
echo "reset" > README.md
git add .
git commit -m "reset deployment"
git push origin reset-branch
```

> ✅ `--orphan` により、履歴がまったく無い新しいスタートになるブランチが作られます。

### 💡 ステップ 2：デプロイ設定を新ブランチに変更（GitHub Pagesの場合）

#### 🔧 GitHub Pagesの場合：

1. GitHubリポジトリの **Settings** → **Pages**
2. 「Source」を `reset-branch` に変更
3. `main` など旧ブランチのデプロイ履歴が表示されなくなる

### 💡 ステップ 3：GitHub Actionsなどでデプロイを管理している場合

#### 📄 `.github/workflows/deploy.yml` がある場合：

* 新しいブランチにも workflow ファイルをコピー
* `on: push` のブランチ名を `reset-branch` に指定

```yaml
on:
  push:
    branches:
      - reset-branch
```

### 💬 こうすることで…

* **元の履歴・デプロイ履歴は保持**
* **新しい履歴で clean deploy を始められる**
* 元に戻したくなったら、`main` に切り替えるだけ

### 🚫 注意：GitHub Deploymentsタブ自体の履歴は完全には消せない

ブランチを切り替えても、「Settings > Environments」内の履歴は残ることがあります。完全に消したい場合は、GitHub APIや Environments の削除が必要です。

* * *

## ✅ 方法1：GitHubリポジトリ設定から**ログとアーティファクトを一括削除**

これは「履歴の中身（ログ）」だけを消します。

### 🔧 手順：

1. GitHubの該当リポジトリへ
2. 上部の「Settings」へ
3. 左側メニューから「Actions」 > 「General」へ
4. 下のほうにある
   　→ `Artifacts and logs`
   　→ `Delete all logs and artifacts for this repository` をクリック

🧹 これで **すべてのworkflow実行ログや成果物（artifact）が削除**されます。
ただし、**実行履歴自体はActionsタブに残ります**（空の履歴として表示）。

## ✅ 方法2：workflow実行履歴自体を**GitHub API**で削除（1件ずつ）

GitHubは公式に「workflow runsの削除」を**API経由でサポート**しています。

### 🔨 手順（GitHub CLIまたはcurl）

#### 1. すべてのworkflow run IDを取得

```bash
curl -H "Authorization: token YOUR_GITHUB_TOKEN" \
     https://api.github.com/repos/USERNAME/REPO/actions/runs
```

#### 2. 1つずつ削除

```bash
curl -X DELETE -H "Authorization: token YOUR_GITHUB_TOKEN" \
     https://api.github.com/repos/USERNAME/REPO/actions/runs/RUN_ID
```

> ⚠️ `YOUR_GITHUB_TOKEN` は、**repoスコープ付きのPersonal Access Token** が必要です。

## ✅ 方法3：リポジトリの「Actions設定を一時無効化」して履歴リセット

### 手順：

1. Settings → Actions → General
2. 「Disable Actions for this repository」にチェック
3. 少し時間を置いて再度有効にすると、履歴がリセットされたように見えることがあります（非推奨なテクニック）

## ❌ 不可能なこと

| やりたいこと              | できるか？ | 方法                               |
| ------------------- | ----- | -------------------------------- |
| 実行ログの一括削除           | ✅     | Settings → Actions → Delete logs |
| 実行履歴自体の一括削除         | ❌     | APIで1件ずつ削除が必要                    |
| Actionsタブからすべて履歴を消す | ❌     | リポジトリを削除・再作成しないと不完全              |

### ✳️ 補足：完全に履歴を消すなら

* **リポジトリを削除して作り直す**
* または、`git clone` → `git remote set-url` → `git push --mirror` で新しい空リポジトリを作成


