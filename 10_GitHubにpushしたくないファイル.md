## ✅ `.gitignore` を使う（基本かつ推奨）

`.gitignore` に除外したいファイル名やフォルダを記載することで、Gitが追跡しなくなります。

### 例：

```plaintext
# キャッシュ・ログ
__pycache__/
*.log

# VSCode設定（個人の環境だけにしたい場合）
.vscode/

# シークレット情報
.env
apikey.txt

# Python仮想環境
venv/
```

* **すでにGitに追加されているファイルは、`.gitignore`に書いても無効**です → その場合は `git rm --cached ファイル名` を使います。

---

## 🛠 手順（VSCodeでの操作）

1. プロジェクトルートに `.gitignore` ファイルがなければ作成

2. 除外したいファイル・フォルダを1行ずつ書く

3. すでにGitに追加されていたら：

   ```bash
   git rm --cached ファイル名
   ```

4. 変更をコミットしてPush：

   ```bash
   git add .gitignore
   git commit -m "Add .gitignore"
   git push
   ```

---

## 💡 VSCode側の補助機能

* `.gitignore` に記載されたファイルは、**Source Controlビューにも表示されません**
* 拡張機能「**gitignore**」や「**.gitignore Generator**」を使えば、自動でテンプレを生成できます（Node、Python、Javaなど用）

---

## 🧹 忘れがちなポイント

* `.env` や `apikey.txt` のような **秘密情報は絶対にpushしない**ように注意！
* **`git rm --cached` はファイル自体は残しつつ、Gitの管理から外す**安全な方法です


