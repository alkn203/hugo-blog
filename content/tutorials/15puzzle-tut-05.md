+++
date = "2016-01-01T23:59:06+09:00"
draft = false
slug = ""
tags = ["tutorial", "15puzzle"]
title = "【phina.js】ゲーム作成チュートリアル（15パズル）（その５）【tweenerで移動処理】"
eyecatch = "/images/15puzzle-tut-4-1.png"
+++

## 前回
前回の[【phina.js】ゲーム作成チュートリアル（15パズル）（その４）【ピースの移動】](http://alkn203.github.io/blog/2016/01/01/15puzzle-tut-04/)では、ピースをタッチで移動できるようにしました。

## 今回の目標
[phina.js](http://phinajs.com/)の目玉機能の1つである**tweener**を使ってピースの移動を滑らかにしてみます。
<center>![15puzzle-tut-2](/images/15puzzle-tut-4-1.png)</center>

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
    var self = this;
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
          // ピース移動処理
          self.movePiece(this);
        };
        // 16番のピースは非表示
        if (num === 16) piece.hide();
      });
    });
    // 参照用
    this.pieceGroup = pieceGroup;
  },
  // 16番ピース（空白）を取得
  getBlankPiece: function() {
    var result = null;
    this.pieceGroup.children.some(function(piece) {
      // 16番ピースを結果に格納
      if (piece.num === 16) {
        result = piece;
        return true;
      }
    });
    return result;
  },
  // ピースの移動処理
  movePiece: function(piece) {
    // 空白ピースを得る
    var blank = this.getBlankPiece();
    // x, yの座標差の絶対値
    var dx = Math.abs(piece.x - blank.x);
    var dy = Math.abs(piece.y - blank.y);
    // 隣り合わせの判定
    if ((piece.x === blank.x && dy === GRID_SIZE) || 
        (piece.y === blank.y && dx === GRID_SIZE)) {
      // タッチされたピース位置を記憶
      var touchX = piece.x;
      var touchY = piece.y;
      // tweenerで移動処理
      piece.tweener.clear()
                   .to({x: blank.x, y: blank.y}, 200, "easeOutCubic")
                   .call(function() {
                     // 空白ピースをタッチされたピースの位置へ
                     blank.setPosition(touchX, touchY);
                   });
    }
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

<a href="http://runstant.com/alkn203/projects/a12dc827" target="_blank">[runstantで確認]</a>

前回までに比べて移動が滑らかになったかと思います。
[phina.js](http://phinajs.com/)では、このような動きを**tweener**という機能で実現できます。

## コード説明
**movePiece**関数を以下のように変更します。


```js
// ピースの移動処理
movePiece: function(piece) {
  // 空白ピースを得る
  var blank = this.getBlankPiece();
  // x, yの座標差の絶対値
  var dx = Math.abs(piece.x - blank.x);
  var dy = Math.abs(piece.y - blank.y);
  // 隣り合わせの判定
  if ((piece.x === blank.x && dy === GRID_SIZE) ||
      (piece.y === blank.y && dx === GRID_SIZE)) {
    // タッチされたピース位置を記憶
    var touchX = piece.x;
    var touchY = piece.y;
    // tweenerで移動処理
    piece.tweener.clear()
                 .to({x: blank.x, y: blank.y}, 200, "easeOutCubic")
                 .call(function() {
                   // 空白ピースをタッチされたピースの位置へ
                   blank.setPosition(touchX, touchY);
                 });
  }
},
```

tweenerの設定部分を説明します。
最初に**clear**で設定をリセットします。

```js
    piece.tweener.clear()

```

見やすいように改行していますが、次にさせたい処理をチェインメソッドで繋げます。

```js

                 .to({x: blank.x, y: blank.y}, 200, "easeOutCubic")

```

* **to**の{}内にパラメータを設定します。今回は指定の位置（空白ピース）へ移動と設定しています。その他、拡大縮小、回転といったパラメータが同時に設定可能です。

* 次の**200**という数字は、{}内の処理をどれくらいの時間をかけて行うかの設定です。単位は**ミリ秒**（1000分の1秒）です。

* 最後の**easeOutCubic**は、イージングといって、動きにエフェクトをかけることができます。**tweener**の使い方については、[simiraaaa](https://twitter.com/simiraaaa)さんの記事[[phina.js] Tweenerを使いこなそう! [Tweener 基本編]](http://qiita.com/simiraaaa/items/b80fec588fa2db272f67)がとても参考になります。

**call**を使うことで、以下のように通常の処理を繋げて**非同期で**実行させることができます。

```js
.call(function() {
  // 空白ピースをタッチされたピースの位置へ
  blank.setPosition(touchX, touchY);
});

```

以上をまとめると、**タッチされたピースを空白ピースの場所に移動させた後、空白ピースをタッチされたピースの元位置に移動（入れ替え）**という一連の処理になります。書き方に多少慣れが必要ですが、より複雑な動きも**tweener**なら手軽に設定することができます。

## 今回はここまで
ここまでで、**tweener**を使ってピースが滑らかに移動できるようになりました。
次回は**ゲーム的要素の追加**で、よりゲームらしくしていきます。
