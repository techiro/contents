# my contents

This repository is for sharing and storing what I have learned.

# 各種設定方法

## Zenn CLI をインストールする

https://zenn.dev/zenn/articles/install-zenn-cli

## zenn-cli + reviewdog + textlint + GitHub Actions

https://zenn.dev/serima/articles/4dac7baf0b9377b0b58b

# よく使うコマンド

https://zenn.dev/zenn/articles/markdown-guide

## プレビュー

```
npx zenn preview # プレビュー開始
```

## 記事作成

```
npx zenn new:article --slug my-awesome-article

```

## コードブロック

\```swift:sample.swift

\```

## Zenn 独自

```
:::message alert

:::
```

# よく使うプラグイン

## [Markdown All in One](https://marketplace.visualstudio.com/items?itemName=yzhang.markdown-all-in-one#keyboard-shortcuts-1)

### コマンド
- ⌘ + shift + p


# VSCodeのコマンド

## 複数行を選択するコマンド

Mac: `⌥ Opt+⌘ Cmd+↑/↓`
Windows: `Ctrl+Alt+↑/↓`
Linux: `Shift+Alt+↑/↓`

### 引用文を挿入するフロー

```text
引用したい文章
引用したい文章
引用したい文章
```
カーソルを行の先頭に持っていき、引用したい文章に到達するまで複数回コマンドを叩く。(`⌥ Opt+⌘ Cmd+↑/↓`)

その状態で引用記号の `>` を入力すれば、完了します。


