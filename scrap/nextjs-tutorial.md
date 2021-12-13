## Navigate Between Pages
- exprt defaultは、ページが読み込まれたときに暗黙的に実行される。
```
export default function FirstPost() {
    return <h1>First Post</h1>
}
```

- ページ作成時
`pages`ディレクトリがそのままURLpathになるのが特徴
これはある意味で、これはHTMLまたはPHPファイルを使用してWebサイトを構築することに似ている。 HTMLを書く代わりに、JSXを書き、Reactコンポーネントを使用する。

- Link Component
リンクを使いたい時はHTMLの場合、`a`タグを使う。
→Next.jsは`Link`コンポーネントを利用する。

```
{''}は空のスペースを追加します。これは、テキストを複数の行に分割するために使用されます。
```
以下のような書き方ができる
<Link href="">
<a>
hogehoge
</a>
</Link>

- コードの最適化
Next.jsは必要なページのみ読み込むため最適化が図れる。
あるページがエラーを出してもその他のページには影響がない

Next以外の外部ページにリンクする必要がある場合。 jsアプリ、`Link`なしの<a>のみを利用する。

- Assets, Metadata, and CSS
`public`ディレクトリを利用する


- Imageタグ
Next.jsは、デフォルトで画像最適化もサポートしています

- CSSコンポーネント
`container`ディレクトリに格納する
重要：CSSモジュールを使用するには、CSSファイル名が.`module.css`で終わる必要があります。


global CSSファイルは`pages/_app.js`のみにインポート可能

- カスタム PostCSS設定

classnamesライブラリ
classnameで読み込むcssを変化させる。
```yarn add classnames```

Next.jsのデフォルトの動作と一致するように、`postcss-preset-env`と`postcss-flexbugs-fixes`を使用することをお勧めします。まず、パッケージをインストールします。

```
npm install tailwindcss postcss-preset-env postcss-flexbugs-fixes
```

https://nextjs.org/learn/basics/assets-metadata-css/styling-tips

## Pre-rendering and Data Fetching

Next.jsの最も重要なコンセプトにはPre-renderingというものがある。
https://nextjs.org/docs/basic-features/pages#pre-rendering
>、各ページのHTMLを事前に生成します。事前レンダリングにより、パフォーマンスとSEOが向上する可能性があります。

Next.jsは静的HTMLをpreレンダリングするため、JavaScript無しでレンダリングできる。

1. ⌥ + ⌘ + i→デバッカーツールの起動
2. ⌘　+ shilf + p →設定画面
3. java　scriptを無効
4. https://next-learn-starter.vercel.app/にアクセス

```注：上記の手順をlocalhostで試すこともできますが、JavaScriptを無効にするとCSSが読み込まれません。```

- Static Generation with Data using `getStaticProps`

getStaticPropsは外部のデータを取得するときに利用される。

- Fetch External API or Query Database

```
export async function getSortedPostsData() {
  // Instead of the file system,
  // fetch post data from an external API endpoint
  const res = await fetch('..')
  return res.json()
}
```

```
import someDatabaseSDK from 'someDatabaseSDK'

const databaseClient = someDatabaseSDK.createClient(...)

export async function getSortedPostsData() {
  // Instead of the file system,
  // fetch post data from a database
  return databaseClient.query('SELECT posts...')
}
```

`getStaticProps`はクライアント側で実行されることはありません。ブラウザのJSバンドルには含まれていません。つまり、ブラウザに送信せずに、データベースクエリなどのコードを記述できます。
fallback keyをfalseにすると、アクセスできなくなる。
