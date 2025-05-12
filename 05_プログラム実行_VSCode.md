## 🟥 Ruby を VS Code で実行する方法

### ✅ 必要なもの

1. **Ruby 本体のインストール**

   * Mac: ほぼ標準で入っている
   * Windows: [RubyInstaller](https://rubyinstaller.org/) を使う
   * Linux: `sudo apt install ruby` など

2. **VS Code 拡張機能のインストール**

   * 拡張機能（Extensions）で次を検索してインストール：

     ```
     Ruby
     ```

     * 作者：Peng Lv（拡張IDは `rebornix.Ruby`）
   * 補足として以下も推奨：

     * `Ruby Solargraph`（コード補完・ドキュメント）
     * `Endwise`（`end` を自動補完）

3. **Ruby スクリプトを実行する方法（ターミナル使用）**

   1. `.rb` ファイルを作成（例：`hello.rb`）

   2. 内容を書く：

      ```ruby
      puts "Hello from Ruby!"
      ```

   3. ターミナルを開いて次のコマンドを実行：

      ```bash
      ruby hello.rb
      ```

---

## 🛠 実行ボタン（Ctrl+Alt+N）を使いたい場合

1. 拡張機能「**Code Runner**」をインストール
   拡張ID: `formulahendry.code-runner`

2. インストール後、Rubyファイルを開いて右上の「▶ Run Code」ボタンを押す
   またはショートカットキー `Ctrl + Alt + N`（Macでは `Control + Option + N`）

---

## 🔄 他の言語の対応プラグイン＆実行方法

| 言語     | 拡張機能名                   | 実行方法例                     |
| ------ | ----------------------- | ------------------------- |
| Python | Python (Microsoft)      | `python ファイル名.py`、または ▶実行 |
| Java   | Extension Pack for Java | `Run`ボタン、ターミナルで`java`     |
| C/C++  | C/C++ (Microsoft)       | `g++`, `gcc`などでビルド＆実行     |
| Go     | Go (Goチーム)              | `go run main.go`          |
| Rust   | rust-analyzer           | `cargo run`               |
| PHP    | PHP Intelephense        | `php ファイル名.php`           |

---

## 🚨 注意点

* 拡張機能は**実行環境を用意するものではなく、エディタの支援をするもの**です。各言語の実行環境（Ruby, Python, Java など）は**別途インストール**が必要です。
* Code Runner で `gets` を含むコードは正しく動作しないことがあるため、ターミナル実行がおすすめです。

