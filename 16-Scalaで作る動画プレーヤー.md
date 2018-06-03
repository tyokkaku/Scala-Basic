# Scalaで作る動画プレーヤー

## 動画プレイヤーの要件
## プロジェクトの作成
## JavaFX の javafx.scene.media

javafx.scene.media：JavaFxのメディアファイルを扱うためのパッケージ

- エンコーディング
  - オーディオ
    - AAC(Advanced Audio Coding)
    - MP3(MPEG-1 Audio Layer-3)
    - PCM(Pulse Code Modulation)
  - ビデオ
    - H.264.AVC
    - VP6

現状の主流ファイル形式
  - ビデオ：H.264/AVC
  - オーディオ：AACで扱っているmp4

パッケージを扱うときのJavaとの差異

- ジェネリクスは<>で表記される
- アクセス修飾子の記述方法が異なる
  - なし    ： 同一パッケージ内のアクセス
  - public ： どこからでもアクセス可能
  - protected: 子クラスの他、同一パッケージ内からもアクセス可能 
- static修飾子がついているメンバーは、インスタンス化しなくても利用できる
- コンストラクタが複数存在する
- traitに似たインスタンス化できない型であるinterface型がある


## mp4 形式の動画の用意

## ウィンドウだけのアプリケーション

可変長引数：メソッドの最後の引数において、いくらでもその型の値を引き受けることができるという型の指定

```scala
package jp.ed.nnn.nightcoreplayer

// JavaFXで利用するクラス
import javafx.application.Application
import javafx.scene.Scene
import javafx.scene.layout.BorderPane
import javafx.scene.paint.Color
import javafx.stage.Stage

// Scalaのアプリケーションとして実行する
object Main extends App{
  // Applicationクラスのlaunchメソッドを、static修飾子のついたメソッドを呼ぶ
  // 引数：Mainクラスをクラスオブジェクトを得るための書き方で渡す
  Application.launch(classOf[Main], args: _*)
}

class Main extends Application {
  
  override def start(primaryStage: Stage): Unit = {
    // borderPaneクラス：レイアウトを行う。JavaFXにおけるUI部品は、Nodeというクラスの子クラスになる
    val baseBorderPane = new BorderPane()
    // Sceneクラス：JavaFXのすべてのUIコンポーネントの入れ物。コンテナと呼ぶ。
    val scene = new Scene(baseBorderPane, 800, 500)
    // 背景色の設定
    scene.setFill(Color.BLACK)
    // primaryStage：JavaFXにおいてSceneを設定することができる。Stageクラスのインスタンス。
    // Sceneをセットして、可視化するためにshowメソッドを呼ぶ
    primaryStage.setScene(scene)
    primaryStage.show()
  }
}
```

## とりあえずの動画プレイヤー

```scala
// media：再生するメディアファイルのクラス
// MediaPlayer：メディアを再生するためのクラス


class Main extends Application {

  override def start(primaryStage: Stage): Unit = {
    // ファイルのパスからMediaのインスタンスを作成
    val path = "/Users/mock/workspace/video.mp4"
    val media = new Media(new File(path).toURI.toString)
    // MediaPlayerのインスタンスを作成
    val mediaPlayer = new MediaPlayer(media)
    // 再生速度を1.25倍に設定し、再生
    mediaPlayer.setRate(1.25)
    mediaPlayer.play()
    // MediaView：JavaFXのNodeの子クラスのインスタンス。実際に映像を表示する。
    // プレーヤーを引数にしてMediaViewのインスタンスを作成
    val mediaView = new MediaView(mediaPlayer)
    val baseBorderPane = new BorderPane()
    // 背景色を黒
    baseBorderPane.setStyle("-fx-background-color: Black")
    baseBorderPane.setCenter(mediaView)
    // 画面の大きさの設定。表示
    val scene = new Scene(baseBorderPane, 800, 500)
    scene.setFill(Color.BLACK)
    primaryStage.setScene(scene)
    primaryStage.show()
  }
}

package jp.ed.nnn.nightcoreplayer

import java.io.File

import javafx.application.Application
import javafx.beans.value.{ChangeListener, ObservableValue}
import javafx.geometry.Pos
import javafx.scene.Scene
import javafx.scene.control.Label
import javafx.scene.layout.{BorderPane, HBox}
import javafx.scene.media.{Media, MediaPlayer, MediaView}
import javafx.scene.paint.Color
import javafx.stage.Stage
import javafx.util.Duration


object Main extends App{
  Application.launch(classOf[Main], args: _*)
}

  // 起動時
  // currentTimeProperty：MediaPlayerクラスのインスタンスの属性。現在の時刻を与える
  // addListener：変化を監視するためのオブジェクトを追加するメソッド。Listnerのインスタンスを追加する。
  mediaPlayer.currentTimeProperty().addListener(new ChangeListener[Duration] {
    override def changed(observable: ObservableValue[_ <: Duration], oldValue: Duration, newValue: Duration): Unit = 
    timeLabel.setText(formatTime(mediaPlayer.getCurrentTime, mediaPlayer.getTotalDuration))
    })
  // 再生時
  mediaPlayer.setOnReady(new Runnable {
    override def run(): Unit =
    timeLabel.setText(formatTime(mediaPlayer.getCurrentTime, mediaPlayer.getTotalDuration))
    })

  private[this] def formatTime(elapsed: Duration, duration: Duration): String =
  
}

```

## 独立したアプリケーションの作成

## 動画形式について

- 動画形式(コンテナ)
  - 映像ファイル
  - 音声ファイル

- コンテナ
  - AVI、WMV、ASF、FLV、MP4、MOV、MPEG、MKV
- コーデック
  - 映像
    - H.264
    - MPEG-4
    - MPEG-2
    - Svid
    - DivX
    - VP9
    - VP8
  - 音声
    - MP3
    - AAC
    - Vorbis
    - WMA
    - MP2
    - FLAC
    - WAV

映像の圧縮

30fps：1秒間に30枚のパラパラ漫画

コーデックの二種類のアルゴリズム
  - 1枚1枚を圧縮する
  - 前後の画像で似た部分を圧縮する

コンテナによって利用可能なコーデックは異なる

コンテナによって機能は異なる(字幕対応、ストリーミング再生、互換性など)

## 動画コーデックの一覧

- 非可逆
  - AOMedia Video1
  - H.264
  - Xvid
  - VP9
- 可逆
  - HuffYUV
  - Ut Video

## 種類と特徴

### AOMedia Video 1

AV1。今後はすべての企業が採用する...？

もっとも圧縮率の高いコーデック。現在もっとも将来性がある。

### H.264/MPEG-4 AVC

主流だったコーデック。低ビットレートでも高画質をキープ

### Xvid/Divx

Divx：一昔前にもっとも主流だったコーデック
Xvid：Divxの同人版

### VP9

Googleのコーデック。Youtubeの動画はすべてVP9で再エンコードされている

### MPEG-1

カラオケ。大昔のコーデック

### MPEG-2

DVD/Blu-ray、地上デジタル放送などで有名なコーデック

### H.264/HEVC

H.264の後継。ニコニコ動画、amazon、appleなどが採用。Av1に移行？

## ビットレートあたりの画質の良さ

``AV1 > H.265/HEVC = VP9 > H.264/MPEG-4 AVC >= WMV9 > MPEG-4 > Xvid = Divx > MPEG-2 > MPEG-1 > DV >= Motion JPEG``