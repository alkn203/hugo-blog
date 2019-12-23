+++
date = "2019-12-2318T20:55:06+09:00"
draft = false
slug = ""
tags = ["array"]
title = "【phina.js】Arrayクラスのeach関数を拡張する"
eyecatch = "break.gif"
+++

### はじめに
**phina.js**には、通常のfor文の代替で使える便利なメソッドがあります。
**Number**の拡張クラスのメソッド**times**が代表的な例です。

```javascript
  (10).times(function(i) {
    console.log('hello');
  });
```

- コールバック関数内に書かれた処理を10回繰り返します。
- 結果は hello が10回表示されます。

#### スプライトを4つのかけらに分ける
この表現に対しては、元のスプライトを部分的に描画した4つの新たなスプライトを作ることにしました。

```javascript
_marioLike: function(sprite) {
  // 縦横の分割数
  var divX = 2;
  var divY = 2;

  var image = sprite.image.domElement;
  // 分割サイズ
  var sizeX = sprite.width / divX;
  var sizeY = sprite.height / divY;
  // グリッド
  var grid = phina.util.Grid(sprite.width, divX);
  var srcRect = sprite.srcRect;
  // 分割スプライト作成
  (divX * divY).times(function(i) {
    // インデックス位置設定
    var xIndex = i % divX;
    var yIndex = (i / divX).floor();
    // 新規canvas作成
    var canvas = phina.graphics.Canvas().setSize(sizeX, sizeY);
    var x = srcRect.x + xIndex * sizeX;
    var y = srcRect.y + yIndex * sizeY;
    // canvasに描画
    canvas.context.drawImage(image, x, y, sizeX, sizeY, 0, 0, sizeX, sizeY);
    // 破片作成
    var piece = phina.display.Sprite(canvas).addChildTo(this);
    // 位置指定
    var px = grid.span(xIndex) + sprite.x - piece.width / 2;
    var py = grid.span(yIndex) + sprite.y - piece.height / 2;
    
    piece.setPosition(px, py);
    piece.rotation = 45;
  }, this);
```

#### かけらのグラフィックは斜めにする
単純にかけらの**rotation**を**45**に設定しただけです。

#### 上のかけらと下のかけらは違う角度で飛ばす

この辺りは、試行錯誤で調整していくほかありません。

```javascript
// 破片を散らす
[[-120, 35], [-60, 35], [-150, 20], [-30, 20]].each(function(elem, i) {
  var piece = this.children[i];
  piece.physical.gravity.y = 4.6;
  piece.physical.velocity = phina.geom.Vector2().fromDegree(elem[0], elem[1]);
}, this);
```

### 拡張ファイル化
汎用性を高めるために**phina.effect.Break**というクラスにしてみました。

https://github.com/alkn203/phina-extensions/blob/master/effect/break.js
