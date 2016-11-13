+++
date = "2016-01-01T22:55:06+09:00"
draft = false
slug = ""
tags = ["tutorial", "15puzzle"]
title = "【phina.js】ゲーム作成チュートリアル（15パズル）（その２）【数字の表示】"
eyecatch = "/images/15puzzle-tut-2.png"
+++

## 前回
前回の[【phina.js】ゲーム作成チュートリアル（15パズル）（その１）【ピースの配置】](http://alkn203.github.io/blog/2015/12/31/15puzzle-tut-01/)では、15パズルのピースの配置を行いました。

## 今回の目標
以下のように、15パズルのピースに数字を表示します。
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
<a href="http://runstant.com/alkn203/projects/7c4b18ba" target="_blank">[runstantで確認]</a>

## コード説明
### 数字の表示
数字の表示には、**Label**クラスを使います。そのために、**Piece**クラスを以下のように変更します。

```js
// ピースクラス
phina.define('Piece', {
  // RectangleShapeを継承
  superClass: 'RectangleShape',
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
```

* **init**関数に**num**という引数を作成し、初期化時に数字を与えるようにします。
* **this.num**に与えらえた値を保持させます。
* **Label**の**text**プロパティに与えられた数字を設定して、**addChildTo**で**this**(Piece)に追加します。併せて、フォントサイズと色を設定しています。ピースの子要素としてラベルを追加しておけば、ピースが動いた時もラベルが追従するようになります。

### 数字の計算

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
    // 16番のピースは非表示
    if (num === 16) piece.hide();
  });
});
```

* 数字は、**MainScene**のピースを配置するループ処理で正しい値になるように計算して、**Piece**に引数として与えます。
* ループでは16番までの数字が作成されますが、15パズルでは16番は不要ですので、条件で16番のピースは空白とみなして**hide**を使って非表示にしています。

## 今回はここまで
ここまでで、各ピースの中心に数字が表示されるようになりました。
次回は、[ピースにタッチイベントを追加](http://alkn203.github.io/blog/2016/01/01/15puzzle-tut-03/)させたいと思います。
