
# クリーンアーキテクチャの用語まとめ

![代表的な図](../images/20210802_07.54.jpg)

- UseCase
- Repository
- Interactor
- Controller


- ビジネスロジック
  - システムにおける実際のロジック部分
  - 入力に対して処理を行い、View に反映させる部分やデータベースを更新する部分


- Entities
  - Enterprise Business Rules
  - ビジネスロジックを表現するレイヤー

- UseCase
  - Application Business Rules
  - ビジネスロジックの API
  - interface や抽象クラス


- Controllers
  - Interface Adapters
  - 入力、永続化、出力が所属
  - 単体テストクラスもここに所属

- UI,DB,ExternalInterface など
  - Frameworks & Drivers
  - UI もここに含まれる。つまり、このレイヤーは、誰からも依存されないため、どんな変更を行なってもその上のレイヤーはその変更を知らない。


## UseCaseの役割
>ユースケースとは、
>利用者があるシステムを用いて特定の目的を達するまでの、双方の間のやり取りを明確に定義したもの
>(https://e-words.jp/w/%E3%83%A6%E3%83%BC%E3%82%B9%E3%82%B1%E3%83%BC%E3%82%B9.htmlから引用)

UML でいうユースケース図で、アクター(人やシステム)が必要とする要件のこと。



## DIコンテナ
抽象化されたクラスに実際の具象化されたクラスを注入するためのものです。
