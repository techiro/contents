---
title: 'Zennの記事をtextlintで文章校正する方法' # 記事のタイトル
emoji: '🤖' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['markdown', textlint] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

# はじめに

今回の記事では，Zenn と Github を連携したレポジトリに，文章校正ツール textlint を導入して，より良い記事を書くための環境を構築する記事です。

## 誰向けに書いたか

- マークダウンで備忘録やメモ，記事を git で管理している人
- Zenn の記事を Github で管理したい人
- 文章校正ツールを自分の環境にインストールしたい人

# 開発環境

- node v15.11.0
- vscode

# Github 連携

Github 連携については，公式のドキュメントがうまくまとまっているため，ここでは詳しく説明しません。以下の記事を参考にしてください。
https://zenn.dev/zenn/articles/connect-to-github

私は，`content` というレポジトリを作成しました。レポジトリの名前はわかりやすい名前にしておけば良いです。

# Zenn CLI をインストールする

任意のレポジトリに zenn CLI をインストールします。

```
cd contents
npm install zenn-cli
npx zenn init
npx zenn preview
```

👀 Preview: http://localhost:8000
が表示されたら成功です。

公式のドキュメント: https://zenn.dev/zenn/articles/install-zenn-cli

# textlint の環境構築

## textlint とは？

textlint とは，自然言語のための校正ツールのことです。
また，npm を使用することで，様々なルールを自分の環境にインストールすることが可能となります。

例えば，公開されている日本語の技術文書向けの textlint ルールをインストールすれば，コマンド 1 つでそのルールを自分の環境にインストールできます。

以下の画像は文章校正の一例です。

![](https://storage.googleapis.com/zenn-user-upload/90e155493ec146a5e3ac67f1.png)

この記事内でインストールしているものは以下のようなものです。

## textlint をインストール

### ターミナル上の操作

```
npm install -g textlint
```

:::message
グローバルにインストールしないと，コマンドがうまく機能しないため， `-g`　でインストールする。また，それでもコマンドが機能しないならば，vscode を再起動するとうまく起動します。
:::

```
command not found: textlint # textlint コマンドが使えない問題が発生するのでグローバルにインストール
```

### VSCode 上の操作

VSCode に拡張機能を導入する。
![](https://storage.googleapis.com/zenn-user-upload/f31e31d608229ffd42d92361.png)

設定は何も変更しない。

## 設定ファイルの構築

`textlint --init `　を行うと `.textlintrc`　ファイルが生成される。
このファイルの中にルールを書いていく。

### ルールの導入

ルールは自分で書くこともできるが，ここは textlint の巨人の肩の上に乗ることをおすすめする。
かなり洗練されたルールがネット上に転がっているためそれを使わせていただく。

まずルールのプリセットをインストールする。

```
npm install --save-dev
textlint-rule-preset-ja-technical-writing textlint-rule-preset-ja-spacing
```

> GitHub: textlint-rule-preset-ja-technical-writing
> かなりメジャーな技術文書向けの textlint ルールプリセット
>
> GitHub: textlint-rule-preset-ja-spacing
> 日本語周りにおけるスペースの有無を決定する textlint ルールプリセット
>
> https://zenn.dev/serima/articles/4dac7baf0b9377b0b58b

次に，`.textlintrc` 上に以下のコードを書く。

```
{
"plugins": {
"@textlint/markdown": {
"extensions":[".md"]
}
},
"rules": {
"preset-ja-technical-writing": {
"no-exclamation-question-mark": {
"allowFullWidthExclamation": true,
"allowFullWidthQuestion": true,
},
"no-doubled-joshi": {
"strict": false,
"allow":["か"], // 助詞のうち「か」は複数回の出現を許す(e.g.: するかどうか)
},
"no-mix-dearu-desumasu": true
},
"preset-ja-spacing": {
"ja-space-between-half-and-full-width": {
space: "always",
exceptPunctuation: true,
},
"ja-space-around-code": {
"before": true,
"after": true
}
},
"ja-technical-writing/ja-no-mixed-period": {
"allowPeriodMarks":[":","。"],
},
"ja-technical-writing/sentence-length": false, //100 文字数制限の無効化
"no-mix-dearu-desumasu": true
}
}
```

以上で textlint の設定は完了。

# おわりに

textlint を導入することで，自分の文章が曖昧であることを客観的に分析したり，スペースを自動的に保管してくれたりと，表現方法を意識できるようになった。

もっと使いやすい設定方法があれば，記事にして共有します！

おわり！

# 参考にした記事

https://github.com/textlint/textlint
https://zenn.dev/serima/articles/4dac7baf0b9377b0b58b

マークダウン記法について
https://zenn.dev/zenn/articles/markdown-guide

よりよい文書を書くための校正ツール「textlint」のSmartHR用ルールプリセットを公開しました！｜SmartHRオープン社内報
https://shanaiho.smarthr.co.jp/n/n881866630eda#4m9j3
