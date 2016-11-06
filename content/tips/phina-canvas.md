+++
date = "2016-01-31T23:59:06+09:00"
draft = false
slug = ""
tags = ["tips"]
title = "ShapeとCanvas描画を組み合わせたサンプル"

+++
### はじめに
phina.jsでゲームを作る際には、通常は**Sprite**や**Shape**を使うことになるでしょう。
しかし、phina.jsには**html5**の**canvas**を機能的にラップした**phina.graphics.Canvas**という便利なクラスがあります。
今回は、この**Canvas**クラスの機能と**Shape**を組み合わせたサンプルを紹介します。

### サンプル
* 矩形をドラッグして離すと画面中央にバネのような動きをして戻ります。
* 線描画用として、画面と同じ広さを持つ空の**Shape**を作ってシーンに重ね合わせています。
* **Shape**の**canvas**に線を描画して、矩形から線が出ているように位置を合わせています。
* **Shape**の**canvas**の描画内容を毎フレーム更新して、線が矩形に追従するようにしています。

<div class='runstant'><iframe src='http://goo.gl/aaEsAL' width='100%' height='640px' style='border:0px;box-shadow:0px 0px 2px 0px #aaa'></iframe></div>

[[runstantで開く](http://goo.gl/BKLSjh)]

### ソースコード

```js
phina.globalize();
// 定数
var SCREEN_WIDTH = 640;
var SCREEN_HEIGHT = 960;
var CENTER_X = SCREEN_WIDTH / 2;
var CENTER_Y = SCREEN_HEIGHT / 2;
// メインシーン
phina.define('MainScene', {
  superClass: 'CanvasScene',
  // コンストラクタ
  init: function() {
    // 親クラス初期化
    this.superInit();
    // 背景色
    this.backgroundColor = 'black';

    Label({
      text: 'Drag and Release',
      fontSize: 48,
      fill: 'yellow',
    }).addChildTo(this).setPosition(CENTER_X, this.gridY.span(2));
    // ロープを作成
    var rope = Rope({
      width: SCREEN_WIDTH, // 画面の幅
      height: SCREEN_HEIGHT, // 画面の高さ
      stroke: 'red',
      strokeWidth: 8,
    }).addChildTo(this);
    // シーンに重ねるために原点を左上にする
    rope.origin.set(0, 0);

    var rect = RectangleShape().addChildTo(this)
                               .setPosition(CENTER_X, CENTER_Y);
    // ドラッグ可能にする
    Draggable().attachTo(rect);
    // リリースされたら
    rect.onpointend = function() {
      // 中心へ移動するアニメーション
      rect.tweener.clear()
                  .to({x: CENTER_X, y: CENTER_Y}, 1000, 'easeOutElastic');
    };
    // 毎フレーム更新
    this.update = function() {
      // 矩形の位置を引き渡す
      rope.rect = rect;
      // canvasを再描画するようにフラグを立てる
      rope._dirtyDraw = true;
    };
  },
});
// ロープクラス
phina.define('Rope', {
  // Shapeクラスを継承
  superClass: 'Shape',
  // コンストラクタ
  init: function(options) {
    this.superInit(options);
    // 矩形情報を内部で保持(最初はダミー値)
    this.rect = Rect(100, 100, 100, 100);
  },
  // 自身のcanvasの描画内容
  render: function(canvas) {
    // スタイル指定
    canvas.strokeStyle = this.stroke;
    canvas.lineWidth = this.strokeWidth;
    // ラインを引く
    canvas.drawLine(CENTER_X, CENTER_Y, this.rect.x, this.rect.y);
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
