+++
date = "2016-01-01T22:55:06+09:00"
draft = false
slug = ""
tags = ["tutorial", "15puzzle"]
title = "【phina.js】ゲーム作成チュートリアル（15パズル）（その２）【数字の表示】"

+++

### 前回
前回の[【phina.js】ゲーム作成チュートリアル（15パズル）（その１）【ピースの配置】](http://alkn203.github.io/blog/2015/12/31/15puzzle-tut-01/)では、15パズルのピースの配置を行いました。

### 今回の目標
以下のように、15パズルのピースに数字を表示します。

<div class='runstant'><iframe src='http://goo.gl/Nso2wV' width='100%' height='640px' style='border:0px;box-shadow:0px 0px 2px 0px #aaa'></iframe></div>

### コード説明
### 数字の表示
数字の表示には、**Label**クラスを使います。そのために、**Piece**クラスを以下のように変更します。

```js
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

### 今回はここまで
ここまでで、各ピースの中心に数字が表示されるようになりました。
次回は、[ピースにタッチイベントを追加](http://alkn203.github.io/blog/2016/01/01/15puzzle-tut-03/)させたいと思います。
