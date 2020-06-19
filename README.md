# detect4clover

Four-leaf clover detector android app.

Tensorflow Lite object detection sample android app with four-leaf clover data.

四つ葉のクローバー検出アプリ(Androidアプリ)。

[Google Cloud AutoML Vision Object Detection](https://cloud.google.com/vision/automl/object-detection/docs/)
を使って、四つ葉のクローバーを学習させたモデルを、
Tensorflow Liteのobject detectionサンプルアプリで使うようにしたもの。

![Screenshot](https://user-images.githubusercontent.com/761487/85104054-c4f1fb80-b242-11ea-8cf9-9802ed36093c.png)

![AutoML Vision Web UI](https://user-images.githubusercontent.com/761487/85104078-d6d39e80-b242-11ea-8d16-da90b0071256.png)

## どんなアプリか
* 四つ葉を探すのにはほとんど使えない。
  人の目で見つけた後に、そこにあることがわかってから使うと検出される。
  どこにあるかわからない状態で、アプリを使って見つけのは難しい。
  * 遠くからだとほとんど検出されないので、
    クローバーが生えているあたりに近付けて、画面を見ながら、
    移動させてスキャンしていくのはかなり面倒。
* 適合率が高くないので、四つ葉以外でも四つ葉として、しばしば検出する。
  * 現状、適合率はあまり高くせず、再現率を少し高めにしているため。
    でないと、何も検出されないまま、動いているかわからない状態が続くので。
  * 例:
    * 右下の青枠は、三つ葉を四つ葉と誤検出している。
    * 左下にある四つ葉を検出できていない。
    * ![Screenshot](https://user-images.githubusercontent.com/761487/85104065-cde2cd00-b242-11ea-920e-ffbece03e202.png)

## アプリの作り方
* 四つ葉のクローバーの写真を用意。自前で撮ったものを使用。
* [200KBになるようにresizer.pyでresize](https://github.com/EdjeElectronics/TensorFlow-Object-Detection-API-Tutorial-Train-Multiple-Objects-Windows-10#3a-gather-pictures)して、
* labelImgでannotation付け。
  * 使用した画像とannotation dataは、
    https://github.com/deton/detect4clover/releases/tag/v1.0.0
* [labelImg-to-csv](https://github.com/serhankilicarslan/labelImg-to-csv)でCSV fileを作って
* Google Cloud Storageに転送して、
* Google Cloud AutoML Visionでimportしてtrain
* modelをexportして、[tflite_metadata.json](https://github.com/deton/detect4clover/files/4804105/tflite_metadata.json.txt)に合わせて[Java codeを調整](https://github.com/deton/detect4clover/pull/2/files#diff-75334c1203929366530e452eaecc673a)。
* DetectorActivity.javaのMINIMUM_CONFIDENCE_TF_OD_APIの値を調整。
  Google Cloud AutoML VisionのWeb UIのevaluateタブで、
  confidence値を変えながら、PrecisionとRecall値を確認。

## 既知の問題点
* アプリをしばらく動かし続けていると、何も検出しなくなる場合あり。
  一度終了して起動し直すと検出するようになる。

## 改良案
* アプリ実行中に、confidence値を手で設定できる機能。
  適合率を高めにして、四つ葉の確率が高いもののみ検出するようにしたい等に、
  設定ができるように。
  * Cloud AutoML VisionのWeb UIでの設定と同様に、confidence値に対する
    適合率と再現率を確認できるグラフ表示付きで。
* 見つけた四つ葉を写真に撮って、四つ葉の位置を指定して、
  学習データとして使えるようにサーバに上げる機能。

## 拡張案
* 検出したら音を出すようにする?
* ドローンやロボットを使って移動させながらスキャンする?
  適合率高めにした上で。
