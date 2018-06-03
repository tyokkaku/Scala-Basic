# Scala でアプリケーションをビルドする

## JavaFX で作成する GUI アプリケーション

- JavaFX：GUIを作成するためのライブラリ
  - もともとはリッチクライアントを開発するために設計された
    - リッチクライアントとは、サーバーとの通信をする、表現力や操作性の高いクライアント・アプリケーションのこと
    - JavaScriptなどで実装する場合もあるが、JavaFXのほうが軽快に動く
    - MinecraftなどはJavaで開発されている

- src/main/scala/Main.scala
  - scalaのソースコードを置くためのフォルダ
  - Javaやテストのソースコードと共存させる場合は、src/main/scalaというようにフォルダを利用する
- build.sbt
  - sbtがビルドを行う際の設定ファイル
  - コンパイル時のオプションなどを設定する
- project/assembly.sbt
  - アプリケーションを単一のファイルとして配布できる形にビルドするためのファイル
  - jarファイルが生成される
    - ``java -jar PATH/jarファイル``でアプリケーションを実行できる

## 起動引数で動きを変更する

```scala
  val circleNum = getParameters.getNamed.getOrDefault("num", "30").toInt
  for (i <- 1 to circleNum) {
```

``java -jar target/scala-2.11/root-assembly-0.1-SNAPSHOT.jar --num=100``