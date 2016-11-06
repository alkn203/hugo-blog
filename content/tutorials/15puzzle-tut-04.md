+++
date = "2016-01-01T23:58:06+09:00"
draft = false
slug = ""
tags = ["tutorial", "15puzzle"]
title = "【phina.js】ゲーム作成チュートリアル（15パズル）（その４）【ピースの移動】"

+++

### 前回
前回の[【phina.js】ゲーム作成チュートリアル（15パズル）（その３）【ピースのタッチ】](http://alkn203.github.io/blog/2016/01/01/15puzzle-tut-03/)では、15パズルのピースをタッチできるようにしました。

### 今回の目標
以下のように、ピースをタッチで移動できるようにします。まずは、動作確認してみて下さい。

<div class='runstant'><iframe src='http://goo.gl/63h9GZ' width='100%' height='640px' style='border:0px;box-shadow:0px 0px 2px 0px #aaa'></iframe></div>

### ピース移動の考え方
* 15パズルにおいて移動できるピースは、空白と隣り合わせのピースだけです。よって、**タッチされたピースと空白の位置を入れ替える**ことが移動とみなせます。これまで、空白をあえて**見えない16番ピース**にしたのは、このためです。

* **隣り合わせ**については、**x座標が同じでy座標の差の絶対値がグリッド1個分のサイズ*、またはその逆パターンの時**で判定しています。

以下のバージョンでは、判定の様子がコンソールに出力されるようにしています。試しにピースをタッチしてみてください。

<div class='runstant'><iframe src='http://goo.gl/fg9O2l' width='100%' height='640px' style='border:0px;box-shadow:0px 0px 2px 0px #aaa'></iframe></div>

### コード説明
### ピースの移動処理

まず、見えない16番ピースを返すだけの**getBlankPiece**関数を**MainScene**に追加します。コードは以下のとおりです。

```js
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
```

**pieceGroup**の子要素配列を**some**関数でループして、16番ピースがあったら**return true**としてループを抜けるようにしています。

次に、ピースの移動処理を行う**movePiece**関数を**MainScene**に追加します。コードは以下のとおりです。

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
    // 一時変数に待避
    var tmpX = blank.x;
    var tmpY = blank.y;
    // 位置入れ換え
    blank.setPosition(piece.x, piece.y);
    piece.setPosition(tmpX, tmpY);
  }
},
```

* タッチされたピースを引数で受け取り、空白ピースと位置を入れ替えます。先に述べたとおり、**隣り合わせ**については、**x座標が同じでy座標の差の絶対値がピース1個分のサイズ*、またはその逆パターンの時**で判定しています。

* 位置の入れ替えは、一方の座標を一時変数に待避させて、どちらかが上書きされないようにします。

最後に、**MainScene**の一部を以下のように変更します。

```js
// ピースグループ
var pieceGroup = CanvasElement().addChildTo(this);
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
```

* **this**の参照が正しくなるように、**MainScene**を指す**this**を**self**という変数に代入しています。

* **piece.onpointend**の中身を**self.movePiece(this)**として、タッチされたピースが引数として渡されるようにしています。ここでの**self**は**MainScene**で**this**は**piece**を指しています。

* **pieceGroup**を**MainScene**全体から参照できるように**this.pieceGroup**という変数に代入しています。

### 今回はここまで
ここまでで、ピースが移動できるようになりました。書き方にもよりますが、[phina.js](http://phinajs.com/)だと**100行程度**でここまで作ることができるのがお分かり頂けたかと思います。
次回は、[phina.js](http://phinajs.com/)の目玉機能の一つである**tweener**で[ピースの動きを滑らかに](http://alkn203.github.io/blog/2016/01/01/15puzzle-tut-05/)してみます。
