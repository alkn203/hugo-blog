+++
date = "2016-03-09T23:59:06+09:00"
draft = false
slug = ""
tags = ["tips"]
title = "スポットライトを作ってみた"

+++

### サンプル
スポットライト的なエフェクトのサンプルです。

* ドラッグして移動することができます。
* マスク用の**Shape**を土台にして、**canvas**の**clear**（矩形クリア）と**arc**を使ってスポットを作っています。

<div class='runstant'><iframe src='http://goo.gl/idCD53' width='100%' height='640px' style='border:0px;box-shadow:0px 0px 2px 0px #aaa'></iframe></div>

[[runstantで開く](http://goo.gl/ZrpjNP)]

### コード

```js
phina.globalize();
// アセット
var ASSETS = {
  // 画像
  image: {
    'logo': 'https://raw.githubusercontent.com/phi-jp/phina.js/develop/logo.png',
  },
};
// 定数
LOGO_SIZE = 512;
PART_SIZE = 128;
// メインシーン
phina.define('MainScene', {
  superClass: 'DisplayScene',
  // コンストラクタ
  init: function() {
    // 親クラス初期化
    this.superInit({
      // 画面サイズ指定
      width: LOGO_SIZE,
      height: LOGO_SIZE,
    });
    // 背景色
    this.backgroundColor = 'skyblue';
    // 中心座標
    var cx = this.gridX.center();
    var cy = this.gridY.center();
    // ロゴ追加
    var logo = Sprite('logo').addChildTo(this).setPosition(cx, cy);
    // スポットライト
    var spot = SpotShape({
      width: LOGO_SIZE * 2,
      height: LOGO_SIZE * 2,
      spotRadius: 64,
      alpha: 0.75,
    }).addChildTo(this).setPosition(cx, cy);
    // ドラッグ可能にする
    Draggable().attachTo(spot);
  },
});
//
phina.define('SpotShape', {
  superClass: 'Shape',
  // コンストラクタ 
  init: function(options) {
    // 親クラス初期化
    this.superInit({
      width: options.width,
      height: options.height,
      backgroundColor: options.fill || 'black',
    });
    // 透明度
    if (options.alpha) {
      this.alpha = options.alpha;
    }
    // スポット半径
    this.spotRadius = options.spotRadius ? options.spotRadius : 32;
  },
  // 描画
   render: function(canvas) {
      // クリアカラー
      canvas.clearColor(this.backgroundColor);
      // 中心に座標を移動
      canvas.transformCenter();

      var r = this.spotRadius;
      var r2 = r * 2;
      // 一旦矩形で切り抜き
      canvas.clear(-r, -r, r2, r2);
      canvas.fillStyle = this.backgroundColor;
      // スポット円外側塗りつぶし
      // 下部分
      canvas.beginPath()
            .arc(0, 0, r, 0, Math.degToRad(180), false)
            .lineTo(-r, r).lineTo(r, r).lineTo(r, -r)
            .fill();
      // 上部分
      canvas.beginPath()
            .arc(0, 0, r, 0, Math.degToRad(180), true)
            .lineTo(-r, -r).lineTo(r, -r).lineTo(r, -r)
            .fill();      // 描画後処理
      return this;
    },
});
// メイン
phina.main(function() {
  var app = GameApp({
    startLabel: 'main',
    // アセット読み込み
    assets: ASSETS,
    // 画面サイズ指定
    width: LOGO_SIZE,
    height: LOGO_SIZE,
  });
  app.run();
});
```
