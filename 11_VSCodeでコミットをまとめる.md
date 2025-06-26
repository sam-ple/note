# VSCodeでコミットをまとめる

## 事前準備

1. **変更内容を整理**
   作業中の変更があれば、まずコミットするか退避（stash）しておく。

   * ターミナルで `git status` を実行し、未コミットの変更がないか確認
   * 変更があれば

     ```bash
     git add .
     git commit -m "途中の作業を一時保存"
     ```

     または

     ```bash
     git stash
     ```

## 1. ターミナルを開く（VSCode内）

* メニューの「ターミナル」→「新しいターミナル」
* 画面下部にターミナルが表示される

## 2. インタラクティブリベースコマンドを実行

```bash
git rebase -i HEAD~N
```

* `N` はまとめたい直近のコミット数
* 例：直近7個をまとめるなら

  ```bash
  git rebase -i HEAD~7
  ```

## 3. エディタでコミットリストが表示される

* VSCodeのエディタが開く（通常は「COMMIT\_EDITMSG」などのファイル）
* こんな感じで7個のコミットがリストアップされている

```
pick a1b2c3d Fix typo
pick b2c3d4e Adjust layout
pick c3d4e5f Add tests
pick d4e5f6a Refactor code
pick e5f6a7b Fix bug
pick f6a7b8c Update README
pick a7b8c9d Minor tweaks
```

## 4. コミットのまとめ設定を変更

* 最初の1個は `pick` のまま残す
* 2個目以降の `pick` を `squash` または `s` に書き換える

例：

```
pick a1b2c3d Fix typo
squash b2c3d4e Adjust layout
squash c3d4e5f Add tests
squash d4e5f6a Refactor code
squash e5f6a7b Fix bug
squash f6a7b8c Update README
squash a7b8c9d Minor tweaks
```

* `squash` はコミットを1つにまとめてメッセージを結合する意味

## 5. 保存してエディタを閉じる

* 編集後、`Ctrl+S` (Windows) / `Cmd+S` (Mac) で保存
* エディタを閉じる（タブを閉じる）

## 6. コミットメッセージ編集画面が開く

* まとめたコミットメッセージが表示されるので、好きなメッセージに編集
* 例：

```
Fix typos and improve layout, tests, and docs

- Fixed various bugs
- Refactored code for clarity
- Updated README and minor tweaks
```

* 編集後、保存＆閉じる

## 7. リベース完了

* コミットがまとめられて履歴がスッキリする
* ターミナルに戻る

## 8. 変更をリモートに反映

* まだリモートにプッシュしていなければ普通に

```bash
git push origin ブランチ名
```

* すでにプッシュ済みで履歴を変えた場合は

```bash
git push --force origin ブランチ名
```

を使う（強制プッシュなので注意）

# ポイントまとめ

* **リベースは履歴を書き換える操作なので慎重に**
* **未コミットの変更はコミットかstashしてから実行**
* **チームで共有しているブランチの場合は、強制プッシュで履歴が変わることに注意**
* **コミットメッセージはわかりやすく書き換えるのがおすすめ**
