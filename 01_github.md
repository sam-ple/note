## 🔗 GitHub × VS Code 最初の連携方法（初回セットアップ）

### ✅ 事前準備

* GitHubアカウントを持っていること
* Git（バージョン管理ツール）がインストール済み（[Git公式サイト](https://git-scm.com/)）

---

## 🧭 手順 1：GitHubアカウントをVS Codeに認証

1. **VS Codeを開く**

2. 左側のサイドバーにある「Source Control（分岐のアイコン）」をクリック

3. 表示される「Gitが初期化されていません」「Gitリポジトリを開いてください」などのメッセージに従って、フォルダを開く（または新規作成）

4. 画面右下または上部に「GitHubにサインイン」と出る場合、それをクリック

   * 表示されない場合：

     * `F1` → 「**GitHub: Sign in**」を実行（コマンドパレット）

5. ブラウザが開き、GitHubのログインページが表示される → 認証を完了

---

## 🧭 手順 2：VS Code から GitHub にリポジトリを作成 or クローン

### 方法①：**新しいリポジトリを作る（ローカル → GitHub）**

1. 任意のプロジェクトフォルダを開く

2. ターミナルで初期化：

   ```bash
   git init
   git add .
   git commit -m "Initial commit"
   ```

3. GitHubに空のリポジトリを作成（ブラウザ上またはVS Code拡張）

4. VS Code ターミナルで：

   ```bash
   git remote add origin https://github.com/ユーザー名/リポジトリ名.git
   git branch -M main
   git push -u origin main
   ```

---

### 方法②：**既存のGitHubリポジトリをクローン（GitHub → ローカル）**

1. GitHub上のリポジトリにアクセスし、「**Code** → HTTPSのURL」をコピー
2. VS Codeで：

   * `F1` → `Git: Clone` を選択
   * コピーしたURLを貼り付けてEnter
   * 保存先フォルダを選ぶ
3. 自動的にリポジトリがクローンされ、開くか確認される → 「開く」を選ぶ

---

## 🧩 おすすめ拡張機能（便利な補助ツール）

* **GitHub Pull Requests and Issues**（公式）
* **GitLens**（Gitの履歴・比較など強化）
* **Git Graph**（視覚的にブランチ管理）

---

## 📝 備考

* **認証方式**：2021年以降、パスワードの代わりに「Personal Access Token (PAT)」が必要。VS Codeがブラウザで自動処理してくれるので、基本は手動でトークンを入力する必要なし。
* **SSH接続したい場合**：鍵ペアの生成＆GitHubへの登録が必要（希望あれば詳しく解説可）


