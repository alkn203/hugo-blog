+++
date = "2016-01-31T23:59:06+09:00"
draft = false
slug = ""
tags = ["tips","pathshape","shape"]
title = "ShapeとPathShapeを組み合わせたサンプル"
eyecatch = "/images/phina-canvas.png"
+++
## はじめに
phina.jsでゲームを作る際には、通常は**Sprite**や**Shape**を使うことになるでしょう。
しかし、phina.jsには**html5**の**canvas**を機能的にラップした**phina.graphics.Canvas**という便利なクラスがあります。
今回は、この**Canvas**クラスの機能を利用して線の描画を行う**PathShape**と組み合わせたサンプルを紹介します。

## サンプル
* 矩形をドラッグして離すと画面中央にバネのような動きをして戻ります。
* 線描画用として、画面と同じ広さを持つ**PathShape**を作ってシーンに重ね合わせています。
* **PathShape**で線を描画して、矩形から線が出ているように位置を合わせています。
* **PathShape**のパス情報を毎フレーム更新して、線が矩形に追従するようにしています。
* パス情報を与えれば、**PathShape**が自動で線を描画してくれます。

<center>![phina-canvas](/images/phina-canvas.png)</center>

## ソースコード

```js
phina.globalize();
/*
 * メインシーン
 */
phina.define("MainScene", {
  superClass: 'DisplayScene',
  // コンストラクタ
  init: function() {
    // 親クラス初期化
    this.superInit();
    // 背景色
    this.backgroundColor = 'black';
    // 画面のサイズ
    var w = this.gridX.width;
    var h = this.gridY.width;
    var cx = w /2;
    var cy = h / 2;
    
    Label({
      text: 'Drag a rect and release',
      fontSize: 48,
      fill: 'yellow',
    }).addChildTo(this).setPosition(cx, this.gridY.span(2));
    // 矩形
    var rect = RectangleShape().setPosition(cx, cy);
    // PathShapeを利用してロープを作成
    var rope = PathShape({
      width: w, 
      height: h, 
      stroke: 'red',
      strokeWidth: 8,
      paths: [Vector2(cx, cy), Vector2(rect.x, rect.y)],
    }).addChildTo(this);
    // 描画順を考えてこのタイミングで追加
    rect.addChildTo(this);
    // ドラッグ可能にする
    Draggable().attachTo(rect);
    // リリースされたら
    rect.onpointend = function() {
      // 中心へ移動するアニメーション
      rect.tweener.to({x: cx, y: cy}, 1000, 'easeOutElastic').play();
    };
    // 毎フレーム更新
    this.update = function() {
      // pathを更新
      rope.changePath(1, rect.x, rect.y);
    };
  },
});
/*
 * メイン処理
 */
phina.main(function() {
  // アプリケーションを生成
  var app = GameApp({
    // MainScene から開始
    startLabel: 'main',
  });
  // fps表示
  //app.enableStats();
  // 実行
  app.run();
});
```

<a href="http://runstant.com/alkn203/projects/6e0e6f07" target="_blank">[runstantで確認]</a>
