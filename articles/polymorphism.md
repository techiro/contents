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
