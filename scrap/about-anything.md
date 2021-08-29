# わからなかった単語・調べた単語をすぐに検索できるページ

## SwiftUI
SwiftUI は、宣言型の状態駆動型フレームワークです。

## KVO(Key Value Observe)
Key Value Observe の略のことです。
>「キー値監視」と訳されている (Swiftのプロパティ監視とは別）
>オブジェクトの属性値の監視をプロパティを通してではなく、キー(文字列、#keyPath式)を使って行うことができる
>通知を受け取る方も通知する方もNSObjectを継承する
>NSKeyValueObserving プロトコルが自動的に適合される(引用:https://qiita.com/ysn/items/9ca0248362f47d563f38)


## SoC Separation of concerns 関心の分離
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


## @StateObjectと@ObservedObjectの違い

@StateObject は親の画面更新によって状態が変化せずに、値が保持される。


## VStackとLazyVStackの違い
「Lazy」が付いていない HStack・VStack との違いは「必要な分だけ読み込む」ところです。
LazyHStack や LazyVStack は画面に表示される分だけ順次メモリにロードしてくれます。


## Conpletionの処理を関数にして切り分ける

```swift

//completionの時
private func onReceive(_ completion: Subscribers.Completion<Error>) {
    switch completion {
    case .finished:
        break
    case let .failure(error):
        print(error.localizedDescription)
    }

}
private func onReceive(_ batch: [RecruitData]) {
    state.responses += batch
    state.page += 1
    state.canLoadNext = batch.count > 50
}


```


## スタブ
スタブ = 事前に設定した振る舞いをする偽物のオブジェクト。特に、取得用のオブジェクトとして利用する。

テスト時のみ、返ってくる値を固定値にする(= 返ってくる値が明確)
クラス外とのやりとりは Protocol を通すことで、実際の実装との関係を切り離す
大域変数と直接やりとりを行わないようにし、大域変数への変更は最小限にとどめること。


https://qiita.com/yokoyas000/items/327fb2b481738fac4c5a


## XCTest
XCTest は、ユニットテストや UI テストの実行環境を提供します。
テストという行為は品質保証を支援することにある。
テストの自動化を行うことで、人の手作業の誤り・失敗を減らすなどを実現することが容易になる。

- iOS Unit Testing Bundle
  - ユニットテスト用
- iOS UI Testing Bundle
  - UI テスト用

ターゲット作成時のテンプレートとして用意されている。


## GEMS（Generic Error-Modeling System）
失敗には２つの種類が存在していて、1 つは実行。 2 つは、計画が不適切という分け方。
人は、滅多に発生しないことを効率的に発見することが難しい生き物です。
自動化されたテストによる恩恵は大きいものの、テストコードやその実行環境の整備に対して向き合う必要があります。

## どんなところをテストするのが効果的か？
- 開発中にあまり触れない、設定画面や規約などの変化の少ない箇所
- 障害発生時に十分に考慮できている必要のある機能（たとえば障害時のみに表示されるダイアログなど）
- 自動化でないと操作が面倒な範囲
- 端末設定や言語などの環境の切り替えの組み合わせ

##　テストをする上でのポイント
「1. 自動化」され「2. 繰り返し実行可能」で「3. 統一的な仕組み」に支えられたテストが重要です。


## AAA パターン(Arrange Act and Assert)


1. 状態の準備 (Arrange)
2. 操作を行う (Act)
3. 結果の検証 (Assert)

このようにテストを３つのフェーズにわけて考える方法は、AAA パターンと呼ばれます。

## Four-Phase Test
「setup」「exercise」「verify」「teardown」
このテストはグローバルなデータベースに変化を与える時に、後片付けを行うことを考慮したテストのことです。


## テストダブルについて
オブジェクト指向(OO)プログラミングで書かれたコードに対してユニットテストをする際の必須のテクニックのことです。
ユニットテストを行う時にオブジェクト同士の依存関係が問題になることがある。そのため、テスト対象の依存するオブジェクトをテスト用にして都合の良い振る舞いをするものに置き換えることで、実際にテストをしたいオブジェクトをテストできるようになります。
依存関係を外部から注入するテクニックを DI（Dependency Injection）と呼ぶ。

## テストダブルの分類　Test Double

- スタブ
  - 都合の良い値を返す
- モック
  - 依存コンポーネントに対するメソッド呼び出しやプロパティのアクセスについて、その呼び出し回数や引数の値を記録するもの
  テスト対象と正しくコミュニケーションを取ったかについて検証できます。
- スパイ
  - モックと同じだが、実際の依存コンポーネントを使用して、アクセスの記録のみを行います。
- フェイク
  - 実際の依存コンポーネントと同等か極めてそれに近い挙動を持つものを、代わりに利用するものです。
- ダミー
  - テスト対象のコンポーネントとは関係がなく、ただコンパイルを通すためだけに作るものです。



## Cuckoo 
自動生成型のモックライブラリ
https://github.com/Brightify/Cuckoo

## HTTPスタブ
外部との HTTP 通信するクラスについて、レスポンスが正しくパースできることを確認するユニットテストを書いたり、
テスト用のダミーレスポンスを使って UI テストをするのに役立ちます。


## アサーションメソッド
expression を exp と略す。


## XCTestのライフサイクル

Singleton やデータベースなどの永続的なデータを扱うコンポーネントには tearDown()後処理を行うことが重要です。

## テストのランダマイズ化
Edit Scheme -> Test -> Options -> Randomize execution order からテスト実行順序のランダム化を行うことができる。

## 非同期のテスト
### 非同期のテストを行うために必要なもの
- `XCTestExpectation(description: String)`
- `wait(for: [XCTestExpectation], timeout: second)`

クロージャーの中に `XCTestExpectation.fulfill()` を入れる。


## コンストラクタインジェクション
コンストラクタインジェクションは、コンポーネントの初期化時に依存するコンポーネントを代入する DI です。

## セッターインジェクション
コンポーネントのイニシャライズ以降にセッターメソッド、あるいは外部に公開されたプロパティに対して依存するコンポーネントを代入する DI です。
セッターインジェクションは代入忘れを起こす可能性があるため、可能な限りコンストラクタインジェクションを使っていくのがベターです。

## スタブ
1. プロトコルの定義
2. プロトコルを継承したスタブクラスを定義
3. Manager や ViewModel などでプロトコルをプロパティーに宣言

実際に使用します。

## 振舞駆動開発(BDD)
- Given　あるコンテキストでテストを与えて
- When　テスト対象に何かが起こる時
- Then  ある結果が期待される
という 3 つの要素に分けて仕様を記述する。

## Quickの基本
describe,context,it を使った Quick の基本です。

## Nimbleの基本
入力は「期待値」と「結果」の 2 つです。

```
expect(＜検証する式＞).to(＜期待する結果＞)
```


## リアクティブプログラミング
アプリの非同期処理を行うために必要なライブラリは Foundation や UIkit などでは、Delegate,Notification Center,GCD Grand Central Dispatch, Callback などが使われている。
これらとは異なる手法で非同期処理を実現しているリアクティブプログラミングという方法がある。

## Dispatch Queue（直訳：送信待ち行列？）
処理待ちタスクを追加するためのキュー。追加された順にタスクを処理側へ渡す役割を担う。タスクの処理は担当しない。
## Main DispatchQueue
OS 側で作成済みなので呼び出すだけ。
1 つだけ存在。
直列処理タイプ。
UI 表示系タスクはここで行わないと動かない。

## Grobal DispatchQueue
OS 側で作成済みなので呼び出すだけ。
5 つ存在。（ただし、実質使えるのは 4 つ）
並列処理タイプ。
用途を指定して呼出し。

その他のディスパッチキューについて詳しく書かれた記事
https://qiita.com/ShoichiKuraoka/items/bb2a280688d29de3ff18

## Operator
ある Publisher を別の Publisher に変換するメソッドを、Combine の用語で「Operator」と呼びます。
- map
- filter
- compactMap
- combineLatest などがある

## Combineについて
SwiftUI は内部的に Combine を活用しています。



## Swift のエラー4分類

### 関数の呼び元で回復可能だと判断される場合
- Simple Domain Error:原因が明白なので、 nil を返す
- Recoverable error:原因によって回復手段が異なると考えられるため、Error を throw
### 関数の呼び元で回復不能だと判断される場合
- Universal error:プログラムを停止させるべきであり、 fatalError() する
- Logic failure:呼ぶ側のバグだとして、 precondition を使って正しい事前条件を明示


## 2つのデータの同期方法

### フロー同期
となりあった画面の状態を次の画面に渡す同期方法

### オブザーバー同期
参照元のオブジェクトの状態をオブザーブして参照先にその変化を同期する方法。オブザーバー同期はデータが変更されるたびに同期処理が実行されるため、いつデータが同期されるかが追いづらいデメリットがある。
しかし、共通した監視先を持つ複数の箇所で、データが同期できるメリットなどもあるため、使い方を分けることが重要です。


## MVP
MVP には 2 種類の考え方がある。PassiveView と Supervisiong Controller です。

### PassiveView
View に描画処理の実装を持たせるのみにしておき、描画指示を Presenter に任せることでプレゼンテーションロジックのテストを行いやすくなる点がメリットです。
各コンポーネントのデータのやりとりはフロー同期によって実現される。
Presenter は 1 つの View に対して、1 つ作成する。iOS においては、一般的に VC の存在が不可欠であるため、View が Presenter のことを知っており、Presenter は View を weak3 章で持つようにする。

