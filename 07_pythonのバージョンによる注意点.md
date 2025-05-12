## ✅ Python 3.xで気をつけるべき主な変更点・進化点（3.10以降中心）

### 🆕 Python 3.10 から追加：`match-case` 構文（構造的パターンマッチ）

```python
match value:
    case 1:
        print("One")
    case 2 | 3:
        print("Two or Three")
    case _:
        print("Other")
```

* ✅ `match-case` は **3.10以降でしか使えません**
* ❌ それ以前のPython（3.9以下）では**SyntaxError**になります

### 🔀 型ヒントの進化（3.10～3.12）

#### ▶ `|` を使ったユニオン型（3.10～）

```python
def handle(value: int | str):
    ...
```

* 3.9以前では `Union[int, str]` を使う必要がある（`from typing import Union`）

### 💡 `except*`（並列例外対応）→ Python 3.11以降

```python
try:
    ...
except* SomeException:
    ...
```

* `except*` は **複数例外（並列処理）** に対応する構文（特に `asyncio` との相性◎）
* Python 3.10以前では使えません

### 📦 標準ライブラリの変更・削除

* `distutils`（インストール関連ライブラリ）→ **Python 3.12 で削除**

  * 今後は `setuptools` や `pip` に完全移行
* `asyncio.get_event_loop()` の挙動が変わった（3.10～）


### 🔐 セキュリティ関連の強化（3.10～）

* 一部の関数（`ssl`, `hashlib` など）で**暗号プロトコルのサポート終了**（古いTLSなど）
* `open()` などで明示的に `encoding="utf-8"` を指定すべきケースが増加（ロケール依存の問題を避けるため）


## 📌 バージョン間で注意が必要な具体例

| 特徴・機能                               | 使えるバージョン        | 注意点                           |                    |
| ----------------------------------- | --------------- | ----------------------------- | ------------------ |
| `match-case`（構造的パターンマッチ）            | 3.10～           | 3.9以前は構文エラー                   |                    |
| \`int                               | str\`（型のUnion）  | 3.10～                         | 3.9以前は `Union` を使う |
| `except*`                           | 3.11～           | 通常の `except` と使い分け注意          |                    |
| `distutils` 削除                      | 3.12～           | setuptools に移行必要              |                    |
| `typing.Annotated`, `LiteralString` | 3.9～3.11で段階的に追加 | 型チェックに使う場合注意                  |                    |
| `self` の型ヒント `Self`                 | 3.11～           | `from typing import Self` が必要 |                    |


## 🔍 実用的な注意点

* VS Code などで仮想環境を使っていると、**古いPythonで `match-case` を書いてエラーになる**ことがある
* プロジェクト全体で `python_requires=">=3.10"` を `setup.cfg` や `pyproject.toml` に明示しておくと安全


## ✅ まとめ：Python 3.x系の主要注意点

| バージョン | 主な追加・変更機能                            |       |
| ----- | ------------------------------------ | ----- |
| 3.8   | `:=`（ワルラス演算子）、`positional-only args` |       |
| 3.9   | 標準コレクションの型ヒントが `list[int]` 形式に対応     |       |
| 3.10  | `match-case`、ユニオン型の \`               | \` 記法 |
| 3.11  | `except*`、パフォーマンス強化、`Self`           |       |
| 3.12  | `distutils` 削除、PEP 695（型ジェネリクスの簡略化）  |       |

* * *

## ✅ ① **構文（文法）の変更・新機能**

特に **3.10〜3.12** では構文追加・変更が目立ちます。

| バージョン | 注意点・例                 | 備考                 |          |                         |
| ----- | --------------------- | ------------------ | -------- | ----------------------- |
| 3.10〜 | `match-case` 構文が新登場   | 以前のバージョンではエラー      |          |                         |
| 3.10〜 | 型ヒントで \`int           | str`のような`          | \` 記法が可能 | 3.9以下では `Union` を使う必要あり |
| 3.11〜 | `except*` 文で並列例外に対応   | 非同期例外処理で使う         |          |                         |
| 3.12〜 | PEP695による型ジェネリクス構文の刷新 | `type[T]` などがより簡潔に |          |                         |

## ✅ ② **標準ライブラリ・APIの削除や挙動変更**

| ライブラリ/API                           | 影響                                    | 詳細                                    |
| ----------------------------------- | ------------------------------------- | ------------------------------------- |
| `distutils`                         | 削除（3.12）                              | `setuptools` に完全移行が必要                 |
| `asyncio.get_event_loop()`          | 挙動変更（3.10〜）                           | 明示的に `new_event_loop()` を使うのが安全       |
| `collections.OrderedDict` などのインポート元 | `collections` → `collections.abc` に変更 | 警告またはエラーになる場合あり（3.10以降）               |
| `open()` のデフォルトエンコーディング             | ロケール依存 → UTF-8 に統一（今後）                | 明示推奨：`open("file", encoding="utf-8")` |

## ✅ ③ **非推奨（deprecated）機能の削除・警告化**

Pythonでは\*\*段階的に非推奨（警告）→ 削除（エラー）\*\*となるので、**DeprecationWarning**が出るコードは今のうちに直すのが安全です。

| 例                                   | 状況                  | 修正方法                        |
| ----------------------------------- | ------------------- | --------------------------- |
| `typing.Sequence` などの `typing` 古い構文 | 非推奨 → モダン構文へ移行      | `list[str]` 等を使う            |
| `inspect.getargspec()`              | 削除                  | `inspect.signature()` を使用する |
| `isAlive()`（thread）                 | `is_alive()` に置換が必要 | Python 3.9 以降               |
| `imp` モジュール                         | 完全削除                | `importlib` に移行（3.12〜）      |

## ✅ ④ 仮想環境や依存パッケージへの影響

* `venv` を再作成すべき（バージョンごとに）
* `requirements.txt` でバージョン指定が厳しい場合、インストールエラーが出やすい
* C拡張（NumPyなど）が**最新版Pythonでまだ対応していない**こともある

```bash
# 新しいPythonで仮想環境を作り直す
python3.12 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

## ✅ ⑤ 既存コードが動かない例（代表的）

```python
# Python 3.10以降でないと使えない
def handle(x: int | str): ...
match x:
    case 1: ...
```

```python
# Python 3.12以降で削除されたdistutils
from distutils.core import setup  # ❌ → ImportError
```

## ✅ バージョンアップ時のチェックリスト

| チェック項目                  | 対応内容                          |             |
| ----------------------- | ----------------------------- | ----------- |
| `requirements.txt` は最新？ | パッケージバージョン指定が古くないか確認          |             |
| 非推奨な関数を使っていないか？         | `DeprecationWarning` を無視しない   |             |
| 標準ライブラリに依存している箇所は？      | import元や挙動の変化を確認              |             |
| 仮想環境は再作成済み？             | `python -m venv` を新バージョンで作り直す |             |
| 型ヒントは古くないか？             | `Union[int, str]` → \`int     | str\` など移行可 |

## 🧩 おすすめツール

* **`pyupgrade`**：古い構文を新構文に自動変換
* **`mypy` / `pyright`**：型チェックとバージョン互換確認
* **`tox`** や **`nox`**：複数バージョンでの自動テスト


