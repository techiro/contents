---
title: '[アプリ開発] Firebase Authを使ってログイン画面を実装する' # 記事のタイトル
emoji: '🔥' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['swift', 'swiftui', 'di'] # タグ。["markdown", "rust", "aws"]のように指定する
published: false # 公開設定（falseにすると下書き）
---

#はじめに
最近、私はクリーンアーキテクチャーについての学習を行なっています。
そんな中、DI(dependency injection) 日本語で、依存性の注入というもの。これを SwiftUI で実現できないかと考えました。

そこで今回は DI を SwiftUI の EnvironmentObject や ViewModel を使って、実装します。
参考にさせていただいた記事 https://mokacoding.com/blog/swiftui-dependency-injection/

# DI の概念

- クラス A がなくてもクラス B が動くこと
- インスタンス生成時にオブジェクトを渡し、クラス間の依存関係なくすこと

こう一般的に言われています。
でも、意味を理解しても、実際にコードを書いてみないとイメージがしにくいため、コードを書いてみます。

# @EnvironmentObject について

SwiftUI は、"親や祖先のビューによって供給される Observable オブジェクト "を定義するために、@EnvironmentObject プロパティラッパーを提供します。
@EnvironmentObject で定義されたプロパティは、親のオブジェクトを参照します。また、EnvironmentObject でラップされた ObservableObject が変化を発するたびに、フレームワークはビューを無効にし、結果として再描画します。

@EnvironmentObject は、SwiftUI の環境でその値を探すので、依存関係を注入できます。依存関係を注入することで、階層の深いところにあるビューが、親が依存関係を通すことなくアクセスできることを意味します。

つまり、環境変数のように、どこからでもアクセスできるようにするには、`App` 。 SwiftUI と UIKit を混ぜてマッチさせている場合は、 `UIWindowSceneDelegate` に `environmentObject(_:) ` を宣言します。

```swift SceneDelegate.swift
//略
    func scene(_ scene: UIScene, willConnectTo session: UISceneSession,
               options connectionOptions: UIScene.ConnectionOptions) {
        // EnvironmentObject のインスタンスを注入する
        let contentView = ContentView().environmentObject(FirebaseAuthenticationService())

        // Use a UIHostingController as window root view controller.
        if let windowScene = scene as? UIWindowScene {
            let window = UIWindow(windowScene: windowScene)
            window.rootViewController = UIHostingController(rootView: contentView)
            self.window = window
            window.makeKeyAndVisible()
        }
    }

```

```swift ContentView.swift

struct ContentView: View {
    // @EnvironmentObjectを使用するため注入
    @EnvironmentObject var authService: FirebaseAuthenticationService
    @State var selectedView = 1
    @State private var tabViewOffset: CGFloat = 0
    var body: some View {
        #if DEBUG

        HomeView()

        #elseif RELEASE
        // @EnvironmentObjectを使用
        if self.authService.user != nil {
            HomeView()
        } else {
            LoginView()
        }
        #endif
    }

}
```

## ObservableObject

1. ObservableObject の宣言
2. インスタンス化し、EnvironmentObject として受け取る。
3. 子 View で使用することが可能
4. 子 View に ViewModel を実装する

# View Model について

SwiftUI で MVVM パターンを使用し、各ビューにデータを表示して動作させるためのすべてのロジックを含むビューモデルを与えると、次のように依存関係を注入するために使用できます。

- 表示するビューを構築する責任を、ビューレイヤーからビューモデルに移動させる。
- init 時にビューモデル内のビューを構築するためのロジックを渡す。
- 一元化された場所でビューモデルを作成し、init 時のビュー構築ロジックの一部として依存関係を注入できます。
- ビューは、ナビゲーションリンクの宛先としてどのビューを使用するか、ボタンにどのテキストを表示するかなど、すべてのロジックをそのビューモデルに委ねるべきです。

## ViewModel の実装

ビジネスロジックをここに書いていく、その際に、EnvironmentObject を private let で受け取る。

```swift
// MARK: View
struct BookDetail: View {

    @ObservedObject var viewModel: BookDetailViewModel

    var body: some View {
        VStack {
            Text(viewModel.title)
            Text(viewModel.author)
            // The view models take care of actioning on the reading list.
            // Because of that, the view only need an instance of the view
            // model; the ReadingListController dependency is hidden inside it.
            Button(action: viewModel.addOrRemoveBook) {
                Text(viewModel.addOrRemoveButtonText)
            }
        }
    }
}

// MARK: ViewModel
class BookDetailViewModel: ObservableObject {

    private let book: Book
    //EnvironmentObject
    private let readingListController: ReadingListController

    // MARK: readingListControllerを使用した処理


```

# 依存関係の注入

このパターンでは、ViewModelFactory がすべてのビューモデルを構築し、各ビューモデルが init 時にビューを構築するロジックを受け取るため、階層の各ノードに依存関係を渡す必要がなくなります。

ルートビューのビューモデルを取得するために、App や UIWindowSceneDelegate の実装など、SwiftUI アプリケーションのトップレベルで ViewModelFactory を呼び出すことができます。

```swift

class ViewModelFactory {

    // Like when using the environment approach, ViewModelFactory is the only
    // point where we instantiate ReadingListController; no singletons or
    // static shared instances needed.
    let readingListController = ReadingListController()

    // Once again, let's gloss over how to load the books for the sake of
    // brevity.
    let books: [Book] = ...

    func makeBookListViewModel() -> BookListViewModel {
        return BookListViewModel(
            books: books,
            viewForSelectedBook: { [unowned self] in
                BookDetail(viewModel: self.makeBookDetailViewModel(for: $0))
            }
        )
    }

    func makeBookDetailViewModel(for book: Book) -> BookDetailViewModel {
        return BookDetailViewModel(book: book, readingListController: readingListController)
    }

    func makeToReadListViewModel() -> ToReadListViewModel {
        return ToReadListViewModel(readingListController: readingListController)
    }
}

```

## サードパーティーライブラリを使用する必要性がなくなる。

Swift の標準機能に近い状態を維持することで、外部ライブラリの学習曲線を取り去り、新しいリリースに依存する必要がなくなります。
