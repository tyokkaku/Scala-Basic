# Scalaで解く部分和問題

## 全探索

コンピュータは繰り返し処理や単純な判定が得意である。

## 部分和問題

```js
a = (3, 6, -5, 7)
n = 4
k = 8
```

## 深さ優先探索

深さ優先探索

最初のパターンを最初に判定する。とにかくすべてのパターンを試していく探索のこと。

部分和問題を解くときは、一回の呼び出しの中で、数列の一番目の要素を「足す場合」と「足さない場合」の2つを再帰呼び出しする。

- タプル
  - 分割代入というScalaの昨日
  - {}で囲って、,で区切った値

## ビット演算子

- ビット単位AND : &
- ビット単位OR  : |
- ビット単位XOR : ^
- ビット単位補数 : ~
- 右ビットシフト(最上位ビットで埋める) : >>
- 右ビットシフト(0で埋める) : >>>
- 左ビットシフト(0で埋める) : <<

### ビット単位AND : &

条件付き論理積``&&``と同様の動きをする

```scala
1 & 1
// res: Int = 1

00000000 00000000 00000000 00000001
00000000 00000000 00000000 00000001

00000000 00000000 00000000 00000001

-1 & 2
// res: Int = 2
11111111 11111111 11111111 11111111
00000000 00000000 00000000 00000010

00000000 00000000 00000000 00000010
```

### ビット単位OR  : |

論理演算子の``||``と同様の動きをする

```scala
1 & 0
// res: Int = 1

00000000 00000000 00000000 00000001
00000000 00000000 00000000 00000000

00000000 00000000 00000000 00000001

-1 & 2
// res: Int = 2
11111111 11111111 11111111 11111111
00000000 00000000 00000000 00000010

11111111 11111111 11111111 11111111
```

### ビット単位XOR  : ^

排他的論理和の``^``と同様の動きをする

```scala
1 ^ 0
// res: Int = 1

00000000 00000000 00000000 00000001
00000000 00000000 00000000 00000000

00000000 00000000 00000000 00000001

-1 ^ 2
// res: Int = 2
11111111 11111111 11111111 11111111
00000000 00000000 00000000 00000010

11111111 11111111 11111111 11111101
```

### ビット単位補数 : ~

全ビットを反転させる。

```scala
~0
// res: Int = -1

00000000 00000000 00000000 00000000

11111111 11111111 11111111 11111111
```

### ビットシフト

>>

右側に最上位ビットで埋めながらシフトする

```scala
-1 >> 31

11111111 11111111 11111111 11111111

11111111 11111111 11111111 11111111
```

``>>>``

右側に最上位ビットを0で埋めながらシフトする

```scala
-1 >> 31

11111111 11111111 11111111 11111111

00000000 00000000 00000000 00000001
```

<<

左側に1ビット０を埋めながらシフトする

```scala
00000000 00000000 00000000 00000001

00000000 00000000 00000000 00000010
```

## ビット演算を用いた部分和問題の実装

```scala
object PartialSumBitOp extends App {
  val a = Seq(1, 10, 49, 3, 8, 13, 7, 23, 60, -500, 42, 599, 45, -23, 1, 10, 49, 3, 8, 13)
  val n = a.length
  val k = 444

  // マッチのフラグ
  var isMatch = false
  // カウンターとなるビット列を表すIntの変数
  var bitsCounter = 0
  // -1をnビット分シフトし、反転する。
  // その結果、n分まで1になり、カウンターの最大値として機能する
  val max = ~(-1 << n)

  // マッチするか、カウンターが最大になるまで、ビットカウンターを1ずつ進めながらループする
  while (!isMatch && bitsCounter <= max) {
    // 合計値sum
    var sum = 0
    // iを0からn-1までループ
    for (i <- 0 to (n - 1)) {
      // マスク： 論理積を使って特定のビット領域の値を取得すること。（ここでは最大値までの値を取得できる）
      val mask = 1 << i
      val masked = bitsCounter & mask
      // マスクされた値が0ではないか確認する。
      // もし対象のビットが1であれば、数列aの添字iの値をsumに追加する
      if (masked != 0) sum = sum + a(i)
    }
    // sumがkにマッチしていたら、マッチを判定とする
    if (sum == k) {
      isMatch = true
    } else {
      // マッチしていなければ、bitsCounterを1加算する
      bitsCounter = bitsCounter + 1
    }
  }
  // マッチした場合に、マッチした組み合わせをresultとして作成してコンソールに出力する
  if (isMatch) {
    var result: Seq[Int] = Seq()
    for (i <- 0 to (n - 1)) {
      val mask = 1 << i
      val masked = bitsCounter & mask
      if (masked != 0) result = result :+ a(i)
    }
    println(s"Yes ${result}")
  } else {
    println("No")
  }
}
```