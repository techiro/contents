---
title: 'ffmpeg エラー解決方法' # 記事のタイトル
emoji: '📷' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['ffmpeg'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---


## 開発環境
- macOS 11.3.1
- (2021/08) ffmpeg を半年ぶりに使ったらエラー

## エラー解決策

Command Line Tools for Xcode をアップデートすると解決しました。

```
xcode-select --install
```

インストールが完了したら、ffmpeg を brew でインストールします。

```
brew install ffmpeg
```

無事インストールできました！

