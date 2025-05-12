## 🔄 既存GitHubリポジトリにローカルプロジェクトを後から連携する手順

### ✅ 前提

* GitHubに空のリポジトリ（または初期コミットありのリポジトリ）が**すでに作成済み**
* ローカル（VS Code）にプロジェクトのフォルダが存在している

---

## 🧭 手順（ローカルフォルダをGitHubリポジトリと連携）

1. **VS Codeでプロジェクトフォルダを開く**

2. **Gitを初期化（まだしていない場合）**

   ターミナルを開いて：

   ```bash
   git init
   ```

3. **ファイルをステージング＆コミット**

   ```bash
   git add .
   git commit -m "First local commit"
   ```

4. **GitHubリポジトリを「origin」として登録**

   GitHubで対象リポジトリを開き、「Code」ボタンからURLをコピー（例：[https://github.com/ユーザー名/リポジトリ名.git）](https://github.com/ユーザー名/リポジトリ名.git）)

   ```bash
   git remote add origin https://github.com/ユーザー名/リポジトリ名.git
   ```

5. **mainブランチへ変更（必要であれば）**

   ```bash
   git branch -M main
   ```

6. **GitHubにプッシュ（初回）**

   ```bash
   git push -u origin main
   ```

---

## 📝 よくある注意点

* GitHub側にすでに「README」などのファイルがあってローカルと食い違うと、プッシュ時にエラーになる場合があります。その場合は次のように対応：

  ```bash
  git pull origin main --allow-unrelated-histories
  ```

  → その後コンフリクトを解決してから `git push`。

---

## 🎯 まとめ

| 状況             | コマンド                        |
| -------------- | --------------------------- |
| Git未初期化        | `git init`                  |
| GitHubリポジトリと連携 | `git remote add origin ...` |
| 最初のPush        | `git push -u origin main`   |

