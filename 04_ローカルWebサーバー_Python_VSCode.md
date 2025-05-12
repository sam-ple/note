## 🧭 手順：VS Code で CGI 対応のローカルWebサーバーを起動する方法

### ✅ 前提条件

* Pythonがインストール済み（バージョン3.x）
* VS Codeがインストール済み
* `python` コマンドが使える（VS Codeのターミナルで `python --version` が動作する）

---

### 🧱 ステップ 1：適切なディレクトリ構造を用意

`http.server` の `--cgi` オプションは、**`cgi-bin/` フォルダ内の `.py` や `.cgi` ファイル**のみをCGIスクリプトとして実行します。

例：

```
project_folder/
│
├── index.html
└── cgi-bin/
    └── test.py
```

> 📌 `cgi-bin/` という名前のディレクトリは必須です。

---

### 🛠 ステップ 2：CGIスクリプトの準備（例）

`cgi-bin/test.py`

```python
#!/usr/bin/env python3

print("Content-Type: text/html")
print()
print("<html><body><h1>Hello from CGI!</h1></body></html>")
```

> ✅ 上記のように、**必ず`Content-Type`ヘッダーと空行**を出力してください。

---

### 🏁 ステップ 3：サーバー起動（VS Codeターミナルで）

ターミナル（\`Ctrl + \`\`）を開き、プロジェクトフォルダに移動してから：

```bash
python -m http.server 8000 --cgi
```

---

### 🌐 ステップ 4：ブラウザでアクセス

以下のURLにアクセス：

* 通常のHTML → `http://localhost:8000/index.html`
* CGIスクリプト → `http://localhost:8000/cgi-bin/test.py`

---

## ⚠️ 注意点

1. `cgi-bin` 外ではCGIスクリプトは動作しません（セキュリティ上の仕様）。
2. Windows で `.py` ファイルを実行するには、スクリプトに `#!/usr/bin/env python3` のような**shebang行**が必要です。
3. CGIは非常に古い仕組みです。必要な理由がなければ、代わりに Flask や FastAPI を使うことをおすすめします。

