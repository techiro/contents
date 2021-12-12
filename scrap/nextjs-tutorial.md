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