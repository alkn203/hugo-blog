+++
date = "2016-01-01T23:55:06+09:00"
draft = false
slug = ""
tags = ["tutorial", "15puzzle"]
title = "【phina.js】ゲーム作成チュートリアル（15パズル）（その３）【ピースのタッチ】"

+++

### 前回
前回の[【phina.js】ゲーム作成チュートリアル（15パズル）（その２）【数字の表示】](http://alkn203.github.io/blog/2015/12/31/15puzzle-tut-02/)では、15パズルのピースに数字を表示させました。

### 今回の目標
見た目の変化はありませんが、ピースにタッチイベントを追加して、結果が分かるようにコンソールに出力させます。

<div class='runstant'><iframe src='http://goo.gl/YnrdZj' width='100%' height='640px' style='border:0px;box-shadow:0px 0px 2px 0px #aaa'></iframe></div>

### コード説明
### タッチイベントの追加
ピース作成時の処理を以下のように変更します。

```js
// ピース配置
PIECE_NUM_XY.times(function(spanX) {
  PIECE_NUM_XY.times(function(spanY) {
    // 番号
    var num = spanY * PIECE_NUM_XY + spanX + 1;
    // ピース作成
    var piece = Piece(num).addChildTo(pieceGroup);
    // Gridを利用して配置
    piece.x = grid.span(spanX) + PIECE_OFFSET;
    piece.y = grid.span(spanY) + PIECE_OFFSET;
    // タッチを有効にする
    piece.setInteractive(true);
    // タッチされた時の処理
    piece.onpointend = function() {
      console.log(this.num);
    };
    // 16番のピースは非表示
    if (num === 16) piece.hide();
  });
});
```

[phina.js](http://phinajs.com/)でオブジェクトのタッチを有効にするためには、**obj.setInteractive(true)**と設定します。今回は、ピース配置時に有効にしますので、**piece.setInteractive(true)**を追加します。

タッチを有効にしましたが、通常プログラムで使うためにはタッチイベントを拾う必要があります。そこで、ピースに対して**onpointend**関数を追加し、タッチ時の処理を記述します。今回は動作確認用にタッチされたピースの数字をコンソールに出力するようにしています。

良く使うタッチイベントは、以下の3種類があります。それぞれ書き換えてみて動作確認すると、違いが分かるかと思います。
* **onpointstart**　--- タッチ開始時
* **onpointmove**　 --- タッチ中（移動含む）
* **onpointgend**　 --- タッチが離れた時

### 今回はここまで
ここまでで、ピースにタッチイベントを追加することができました。
次回は、メインともいえる[ピースの移動](http://alkn203.github.io/blog/2016/01/01/15puzzle-tut-04/)を実装したいと思います。
