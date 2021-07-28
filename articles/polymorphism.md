# オブジェクト指向プログラミングで重要な性質

- カプセル化
- 継承
- ポリモーフィズム

## カプセル化

カプセル化は、クラスの宣言と定義を分離することです。

## 継承

```swift

func abstructPolymorphism() {

    class Animal {
        func greeting() -> String {
            return "";
        }
    }


    class Cat: Animal {
        override func greeting() -> String {
            return "にゃあー"
        }
    }

    class Dog: Animal {
        override func greeting() -> String {
            return "ワン！"
        }
    }

    let catAndDog = [
        Cat(),
        Dog()
    ]

    catAndDog.forEach { animal in
        print(animal.greeting())
    }


}

abstructPolymorphism()

```

## OO とは何か？

ソフトウェアアーキテクトの世界では答えは明らかで、OO とは「ポリモーフィズム」を使用することで、システムのソースコードの依存関係を絶対的に制御する能力のことです。

上位レベルと下位レベルのモジュールを独立させて開発することができる。

## 依存関係逆転

ソースコードの依存関係と制御の流れは必ずしも同じでなくても良い。
依存の関係は UML の矢印で表すことができる。
ソースコードの依存関係はどこにあっても逆転できる。
システムにあるソースコードの依存関係の方向を絶対的に制御できる。
依存関係を制御の流れに合わせる必要がなくなる。

![依存関係逆転](../images/20210728_11.20.png)

ビジネスルールを含むコンポーネントは UI やデータベースを含むコンポーネントに依存しない。
逆に UI と DB はビジネスルールに依存している。

![UIとDBはビジネスルールに依存](../images/20210728_11.28.png)
