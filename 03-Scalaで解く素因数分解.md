# 03 Scalaで解く素因数分解

## 素因数分解のアルゴリズム

素因数分解： 与えられた数がどのような素数の掛け算によって表されるのか分かるまで、素数を積の形に分解する方法のこと

試し割り法
  - ある数が与えられたら、最小の素数である2から割れるかどうかをチェックする方法のこと
    - 割る数は最大で、与えられた数の平方根の数まで

## Scala でよく利用されるコレクション

- Seq
  - シーケンス
  - 一つの種類の型から構成される要素のコレクション。配列と同じようなもの。

```scala
// シーケンスを生成する
  Seq(1, 2, 3)
  // Seq[Int] = List(1, 2, 3)

// シーケンスの要素に添字でアクセスする
  resX(0)
  // Int = 1

// シーケンスに要素を追加する
  resX :+4
  // Seq[Int] = List(1, 2, 3, 4)
```

- Map
  - 連想配列
```scala
// Mapを生成する
  Map("key1" -> 1, "key2" -> 2)
  // scala.collection.immutable.Map[String,Int] = Map(key1 -> 1, key2 -> 2)

// Mapの要素にキーでアクセスする
  resX("key2")
  // Int = 2

// キーと値の組み合わせを追加する
  resX + ("key3" -> 3)
  // scala.collection.immutable.Map[String,Int] = Map(key1 -> 1, key2 -> 2, key3 -> 3)
```

なお、SeqとMapは基本的には不変なコレクションであり、それ自身を変更することはできない

## 再帰関数の中身の解説

```scala
import scala.math.sqrt

object Factorization extends App {
  var target = 24
  val maxDivisor = sqrt(target).toInt

  def factorizationRec(num: Int, divisor: Int, acc: Map[Int, Int]): Map[Int, Int] = {
    if (divisor > maxDivisor) {
      // 割る数が最大値よりも大きい場合の処理
      if (num == 1) acc else acc + (num -> 1)
    } else if (num % divisor == 0) {
      // 割り切れた場合の処理
        // Scalaでは Map() + (key -> value) として、新しいMapを取得できる
        // キー：divisor 値：acc.getOrElseで得られる現在の割った回数に1を足した値
      val nextAcc = acc + (divisor -> (acc.getOrElse(divisor, 0) + 1))
      factorizationRec(num / divisor, divisor, nextAcc)
    } else {
      // 割り切れなかった場合の処理
      factorizationRec(num, divisor + 1, acc)
    }
  }

  println(factorizationRec(target, 2, Map()))
}
```

- getOrElseメソッド
  - Mapのメソッド
  - 第一引数にそのMapのキー、第二引数にキーの値がなかった場合の値を設定する

