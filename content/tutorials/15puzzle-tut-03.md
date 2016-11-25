+++
date = "2016-01-01T23:55:06+09:00"
draft = false
slug = ""
tags = ["tutorial", "15puzzle"]
title = "【phina.js】ゲーム作成チュートリアル（15パズル）（その３）【ピースのタッチ】"
eyecatch = "/images/15puzzle-tut-2.png"
+++

## 前回
前回の[【phina.js】ゲーム作成チュートリアル（15パズル）（その２）【数字の表示】](http://alkn203.github.io/blog/2015/12/31/15puzzle-tut-02/)では、15パズルのピースに数字を表示させました。

## 今回の目標
見た目の変化はありませんが、ピースにタッチイベントを追加して、結果が分かるようにコンソールに出力させます。
<center>![15puzzle-tut-2](/images/15puzzle-tut-2.png)</center>
コードは以下のとおりです。

```js
phina.globalize();
// 定数
var SCREEN_WIDTH = 640;            // 画面横サイズ
var SCREEN_HEIGHT = 960;           // 画面縦サイズ
var GRID_SIZE = SCREEN_WIDTH / 4;  // グリッドのサイズ
var PIECE_SIZE = GRID_SIZE * 0.95; // ピースの大きさ
var PIECE_NUM_XY = 4;              // 縦横のピース数
var PIECE_OFFSET = GRID_SIZE / 2;  // オフセット値
// メインシーン
phina.define('MainScene', {
  superClass: 'DisplayScene',
  // コンストラクタ
  init: function() {
    // 親クラス初期化
    this.superInit();
    // 背景色
    this.backgroundColor = 'gray';
    // グリッド
    var grid = Grid(SCREEN_WIDTH, PIECE_NUM_XY);
    // ピースグループ
    var pieceGroup = DisplayElement().addChildTo(this);
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
  },
});
// ピースクラス
phina.define('Piece', {
  // RectangleShapeを継承
  superClass: 'phina.display.RectangleShape',
    // コンストラクタ
    init: function(num) {
      // 親クラス初期化
      this.superInit({
        width: PIECE_SIZE,
        height: PIECE_SIZE,
        cornerRadius: 10,
        fill: 'silver',
        stroke: 'white',
      });
      // 数字
      this.num = num;
      // 数字表示用ラベル
      this.label = Label({
        text: this.num + '',
        fontSize: PIECE_SIZE * 0.8,
        fill: 'white',
      }).addChildTo(this);
    },
});
// メイン
phina.main(function() {
  var app = GameApp({
    startLabel: 'main',
  });
  app.run();
});
```
<a href="http://runstant.com/alkn203/projects/dea18292" target="_blank">[runstantで確認]</a>

## コード説明
### タッチイベントの追加
ピース作成時の処理を以下のように変更します。u

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
