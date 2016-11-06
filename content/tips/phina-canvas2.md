+++
date = "2016-02-16T23:59:06+09:00"
draft = false
slug = ""
tags = ["tips"]
title = "ShapeとCanvas描画を組み合わせたサンプル（時計Shape）"

+++

### はじめに
以前に**tmlib.js**で作った物を**phina.js**にリファクタリングしてみました。
**Shape**を通常の**canvas**と見なせますので、今回の時計に限らず、**canvas**で描けるものは全て**Shape**として取り扱うことができます。

<div class='runstant'><iframe src='http://goo.gl/y3bhHM' width='100%' height='640px' style='border:0px;box-shadow:0px 0px 2px 0px #aaa'></iframe></div>

[[runstantで開く](http://goo.gl/hY3BcQ)]

### コード
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
    // 時計Shapeを作成
    this.clock = ClockShape({
      width: 320, // 幅
      height: 320, // 高さ
      fill: "silver",
      stroke: "gray",
    }).addChildTo(this).setPosition(320, 480);

    this.clock.physical.gravity.y = 0.5;
  },
  // 毎フレーム更新
  update: function() {
    // canvasを再描画するようにフラグを立てる
    this.clock._dirtyDraw = true;

    if (this.clock.bottom > 960) {
      this.clock.physical.force(0, -10);
    }
  },
});
// 時計クラス
phina.define('ClockShape', {
  // Shapeクラスを継承
  superClass: 'Shape',
  // コンストラクタ
  init: function(options) {
    this.superInit(options);
    this.backgroundColor = 'transparent';
  },
  // 自身のcanvasの描画内容
  render: function(canvas) {
    // 描画領域クリア
    canvas.clear();
    // スタイル指定
    canvas.fillStyle = this.fill;
    canvas.strokeStyle = this.stroke;
    // 半径
    var radius = this.radius;
    // canvasの中心
    var cx = 0;
    var cy = 0;
    var baseW = radius / 50;
    // 時計盤描画
    canvas.fillCircle(cx, cy, radius);
    canvas.lineWidth = baseW;
    canvas.strokeCircle(cx, cy, radius - 2);
    canvas.lineWidth = baseW * 2;
    canvas.strokeCircle(cx, cy, radius * 9.0 / 10);
    // 目盛終点
    var tr = radius * 8.3 / 10;
    // 角度
    var deg = 0;
    canvas.lineWidth = baseW;
    // 繰り返し
    (60).times(function(i) {
      // 目盛始点（5分毎の目盛か判定）
      var fr = deg % 30 === 0 ? radius * 7.5 / 10 : radius * 8.0 / 10;
      // 目盛描画
      var rad = Math.degToRad(deg);
      var fromX = cx + fr * Math.cos(rad);
      var fromY = cx + fr * Math.sin(rad);
      var toX = cx + tr * Math.cos(rad);
      var toY = cx + tr * Math.sin(rad);
      canvas.drawLine(fromX, fromY, toX, toY);
      // 角度を進める
      deg += 6;
    });
    // 現在の時刻
    var d = new Date();
    // 時は12時間表記へ
    var hour = d.getHours() - 12;
    var minute = d.getMinutes();
    var second = d.getSeconds();
    // 短針の長さ
    var hr = radius * 6.0 / 10;
    // 長針、秒針の長さ
    var sr = radius * 7.7 / 10;
    // 角度補正値
    var adj = 90;
    // 短針描画
    this.drawRadialLine(cx, cy, hr, hour * 30 + minute / 2 - adj);
    // 長針描画
    this.drawRadialLine(cx, cy, sr, minute * 6 - adj);
    // 秒針描画
    canvas.lineWidth = baseW / 2;
    this.drawRadialLine(cx, cy, sr, second * 6 - adj);
    this.drawRadialLine(cx, cy, -radius * 2.0 / 10, second * 6 - adj);
    // 中央円
    canvas.save();
    canvas.fillStyle = canvas.strokeStyle;
    canvas.fillCircle(cx, cy, radius * 0.4 / 10);
    canvas.restore();
  },
  // 中心から放射状に線を描画
  drawRadialLine: function(cx, cy, radius, deg) {
    var rad = Math.degToRad(deg);
    // 円周上の点計算
    var toX = cx + radius * Math.cos(rad);
    var toY = cy + radius * Math.sin(rad);
    // 線描画
    this.canvas.drawLine(cx, cy, toX, toY);
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
