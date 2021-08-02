# わからなかった単語・調べた単語をすぐに検索できるページ

## SwiftUI
SwiftUI は、宣言型の状態駆動型フレームワークです。

## KVO(Key Value Observe)
Key Value Observe の略のことです。
>「キー値監視」と訳されている (Swiftのプロパティ監視とは別）
>オブジェクトの属性値の監視をプロパティを通してではなく、キー(文字列、#keyPath式)を使って行うことができる
>通知を受け取る方も通知する方もNSObjectを継承する
>NSKeyValueObserving プロトコルが自動的に適合される(引用:https://qiita.com/ysn/items/9ca0248362f47d563f38)


## SoC Separation of concerns
コンピュータプログラムを異なるセクションに分割し、各セクションが別々の問題に対処する設計原理のことです。
5 つの SOLID 原則（単一責任とインターフェイス分離）のうち 2 つが、この概念から直接派生していることが非常に重要です。

## @Binding
SwiftUI では、変数を参照型として扱うための Binding という型があります。
@Binding は、Binding 型を扱うための PropertyWrapper です。
つまり、@Binding と Binding は明確には異なるものです。

## VIPER

- View: View と ViewController
- Interactor: API 通信担当
- Presenter: 自分以外の中継役
- Entity: データそのもの
- Router: 画面遷移担当

## Presenter
View と Router、Interactor の３つと関係しています。
依存しているのは、Router と Interactor の 2 つで、View からは依存されています。そのため Presenter は View のことは何も知らずただ View(Button やタップなど)に命令されたことに従うウェイターさんのような仕事をこなします。
- View から受け取ったイベントを別クラスに依頼
- View に対して画面更新を依頼
- Interactor に対してデータの取得を依頼
- Router に対して画面遷移を依頼

```swift
final class PostsListPresenter: ObservableObject {
    struct Parameter {
        let frogs: [Post]
    }
    enum Inputs {
        case didTapAboutButton
    }
    private let router = PostsListListRouter()
    let params: Parameter
    @Published var isShowAbout = false
    init(params: Parameter) {
        self.params = params
    }
}
```