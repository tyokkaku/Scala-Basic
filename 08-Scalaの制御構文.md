# Scalaの制御構文

## 制御構文

- 構文：プログラミング言語内でプログラムが構造を持つためのルール
  - 式：評価すると値になるもの。(1や1+2、"hoge"など)
  - 文：評価しても値にならないもの(val i = 1など)

Scalaは式が多く使われる言語である。

## {}式

{}は、英語で、braceと呼ぶ。

``{ exp1; exp2; ...expN; }``
exp1からexpNを順番に評価し、expNを評価した値を返す。最後の式が評価されて最後の値が帰る。

## if式

``if (条件式) A [else B]``

- Unit：ユニット。関数が意味のある値を返さないことを意味する型
  - ()は、Unitという型の()というオブジェクトの値

## while式

``while(条件式) A``

条件式がBoolean型のtrueである限りAを評価しつづける。

式として使うと、評価した場合に取得される値は()となる。

したがって、while文として使われる。

```scala
  def printWhileResult(): Unit = {
    var i = 0
    val result = while (i < 10) i = i + 1
    println(result)
  }
  // 以上は式であり、評価すると()を返す
```

``do A while (条件式)``で、do while式

## for式

``for (ジェネレータ1; ジェネレータ2; ... ジェネレータn) ``
``# ジェネレータ1 = a1 <- exp1``

- to ：包含する
- until：排他する

1. 複数のRange型の値を受け取れる
2. if句で条件指定できる
  - if句では、フィルタリングすることもできる(exclude(除外)できるし、select(選択)できる)
3. コレクションの要素にひとつずつ何らかの処理を行える
  - コレクションの要素を加工して新しいコレクションを作るときは、yieldをつかう

```scala
  def collectionLoop(): Unit =
    for(e <- Seq("A", "B", "C", "D", "E")) println(e)

  def generateCollection(): Seq[String] =
    for(e <- Seq("A", "B", "C", "D", "E")) yield "Pre" + e
```