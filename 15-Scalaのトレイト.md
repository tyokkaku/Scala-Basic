# Scalaのトレイト

## トレイトについて

ミックスイン：トレイトを継承すること

- トレイトの特徴
  - 複数のトレイトを1つのクラスやトレイトにミックスインできる
  - 直接インスタンス化できない
  - クラスパラメーター(コンストラクタ引数)を取ることができない

抽象クラスの継承がis-aの関係を持つのに対して、トレイトは、「振る舞いの追加」という意味合いで使われる

traitで定義されるメソッドは、必ずしも実装される必要はない

## ミックスインの行い方

ひとつのクラスに対して複数の振る舞いを与える

``class [クラス名] extends [クラス/トレイト] with [クラス/トレイト] with [クラス/トレイト]``

```scala
trait Fillable {

  def fill(): Unit = println("Fill!")

}

trait Disposable {

  def dispose(): Unit = println("Dispose!")

}

class Paper

// PaperCupクラスは、FillableとDisposableというトレイトをミックスインしている
class PaperCup extends Paper with Fillable with Disposable

```

## トレイト、ミックスインの注意点

トレイトは、

1.クラスの複数継承はできない
2.直接インスタンス化できない
  - 匿名内部クラスを使えば、トレイトをミックスインしたクラスのインスタンス化はできる
3. コンストラクタ引数は宣言できない

匿名内部クラス：評価時に作成するクラスのこと

``val 変数 = new トレイト名(){ override def メソッド名(): Unit = println("Hello") }``

## 多重継承で起こる菱型継承問題

菱形継承問題：多重継承によってメソッドの実装が衝突すること

```scala
class Class1 extends TraitB with TraitC
<console>:15: error: class Class1 inherits conflicting members:
  method greet in trait TraitB of type ()Unit  and
  method greet in trait TraitC of type ()Unit
(Note: this can be resolved by declaring an override in class Class1.)
       class Class1 extends TraitB with TraitC
```

解決方法

1. 定義時にメソッドをoverrideする
2. superに肩を指定してメソッドを呼び出す
  - `` override def メソッド名(): Unit = super[指定するトレイト名].greet()``

両方のメソッドを呼び出したい場合

1. superキーワードを使って両方とも明示的に呼ぶ
2. 線形化

## 線形化の仕組み

線形化：トレイトがミックス院された順番をトレイトの継承順番とみなす。(継承するトレイトのメソッドにoverride修飾子をつける)

線形化をすると、あとからミックスインされたトレイトが優先される

## 自分型アノテーション

自分型アノテーション：クラスやトレイトの中で自分自身の型にアノテーションを記述すること

アノテーション：あるデータに対して関連する情報（メタデータ）を注釈として付与すること

```scala
trait Greeter {
  def greet(): Unit
}

trait Robot {
  // Robotトレイト自身がミックスインされるインスタンスは、Greeterというトレイトがなくてはならない、ことの指定
  self: Greeter =>

  def start(): Unit = greet()
  override final def toString = "Robot"
}
```

Greeterトレイトを継承したHelloGreeterトレイトを作成

Robotトレイトだが、HelloGreeterにはGreeterトレイトがミックスインされているので、インスタンス化ができる

```scala
trait HelloGreeter extends Greeter {
  def greet(): Unit = println("Hello!")
}
val r: Robot = new Robot with HelloGreeter
r.start()
```

依存性の注入(Dependency Injection)：利用するモジュールの実装を外側から与えること

ただし、アノテーションをつけずに呼び出しても、greetメソッドを呼ぶことができてしまう。(is-aの縛りを壊してしまう)。外から呼び出すことができるのは完結的ではなくなり、あまりよいことではない。

自分型アノテーション利用の特徴
  - is-aの縛りを影響させないで、振る舞いだけインスタンスに与えることができる
  - 循環参照を許す

- 依存性の注入を行うさまざまな方法
  - 継承を使う
  - 自分型を使う
  - コンストラクタを使って機能を持つオブジェクトを渡す
  - 依存性注入を行うライブラリを利用してXMLなどの設定やアノテーションという機能を利用して行う