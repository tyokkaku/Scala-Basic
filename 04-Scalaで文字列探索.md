# Scalaで文字列探索

## 文字と文字列

- String
- Char
  - 2バイトのデータで、Unicodeというエンコードで表現された文字列のこと。(エンコードとは、2進数の数値を文字に対応させる方法のこと)

```scala
// 文字は1文字をシングルクォーテーションで囲むこともできる
'a'
Char = a
// ダブルクォーテーションで囲んだ場合は文字列になる
"a"
String = a
```

## 文字列検索

- 索引型検索
  - 文章からあらかじめ単語を抜き出して、抜き出した単語ごとに索引を作っておく検索方法のこと
  - GoogleなどのWeb検索で用いられる
- 非索引型検索
  - 与えられた文字の情報のみを用いて検索する方法のこと
  - エディタ内やWebページ内の文字検索などで用いられる

## シンプルな文字列検索のアルゴリズム

文字列探索のアルゴリズムはいくつかある

- 力まかせの探索
- BM法

## for と while

for

``for(i <- 0 to 10) { println(i) }``

while

```scala
var i = 0
while (i <= 10) { println(i); i = i + 1}
```

do-while

```scala
var i = 0
do { println(i); i = i + 1} while (i <= 10)
```

## 「パターン」と一致した添字を取得

```scala
object SimpleSearch extends App {
  val text = "カワカドカドカドドワンゴカドカドンゴドワドワンゴドワカワカドンゴドワ".toSeq
  val pattern = "ドワンゴ".toSeq
  val matchIndexes = search(text, pattern)

  def search(text: Seq[Char], pattern: Seq[Char]): Seq[Int] = {
    // 空のシークエンスを生成
    var matchIndexes = Seq[Int]()
    // 添字の最初から最後まで、isMatchで、マッチしたかどうかの真偽値を得る
    for (i <- 0 to text.length - 1) {
      val partText = text.slice(i, i + pattern.length)
      // 空のシークエンスに値を追加
      if (isMatch(partText, pattern)) matchIndexes = matchIndexes :+ i
    }
    matchIndexes
  }

  def isMatch(textPart: Seq[Char], pattern: Seq[Char]): Boolean = {
    var isMatch = true
    var i = 0
    while (i <= pattern.length - 1) {
      if (textPart.length < pattern.length || textPart(i) != pattern(i)) isMatch = false
      i = i + 1
    }
    isMatch
  }

  println(s"出現場所： ${matchIndexes}")
}
```

## 正規表現を使った文字列検索

```scala
object RegexSearch extends App {
  val text = "カワカドカドカドドワンゴカドカドンゴドワドワンゴドワカワカドンゴドワ"
  val pattern = "ドワンゴ"
  val matchIndexes = pattern.r.findAllIn(text).matchData.map(_.start).toList
  if(matchIndexes == List()) println("wwww")
  println(s"出現場所： ${matchIndexes}")
}
```

- pattern.r : Regex型に変換
- pattern.r.findAllln(text). : 文字列を検索
- pattern.r.findAllln(text).matchData : マッチ情報を取得
- pattern.r.findAllln(text).matchData.map(_.start) : すべてのマッチ情報から出現箇所のインデックスを取得
- pattern.r.findAllln(text).matchData.map(_.start).toList : iterator[Int]型を、List[int]型に変換