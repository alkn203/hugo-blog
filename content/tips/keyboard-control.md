+++
date = "2017-05-24T22:18:06+09:00"
draft = false
slug = ""
tags = ["sprite", "keyboard"]
title = "Spriteをキーボードで操作する"
eyecatch = "/images/keyboard-control.gif"
+++ 

## 今回のTips
スプライトをキーボードで動かす方法について説明します。

![keyboard-control.gif](/images/keyboard-control.gif)

<center><a href="http://runstant.com/alkn203/projects/1d78bbe7" target="_blank">[runstantで確認]</a></center>

### getKeyメソッドの使い方

```js
    // 毎フレーム処理
    sprite.update = function(app) {
      var key = app.keyboard;
      // 上下左右移動
      if (key.getKey('left')) { sprite.x -= SPEED; }
      if (key.getKey('right')) { sprite.x += SPEED; }
      if (key.getKey('up')) { sprite.y -= SPEED; }
      if (key.getKey('down')) { sprite.y += SPEED; }
    };
```

* **app**経由で**keyboard**を入手しています。
* **getKey**は、引数で指定されたキーが押されているかを返します。 
* 各方向キーが押されているかを判定して、適切な方向にスプライトを移動させています。

### コード
```js
// グローバルに展開
phina.globalize();
// アセット
var ASSETS = {
  // 画像
  image: {
    'tomapiko': 'https://rawgit.com/phi-jp/phina.js/develop/assets/images/tomapiko.png',
  },
};
// 定数
var SPEED = 4;
/*
 * メインシーン
 */
phina.define("MainScene", {
  // 継承
  superClass: 'DisplayScene',
  // コンストラクタ
  init: function() {
    // 親クラス初期化
    this.superInit();
    // 背景
    this.backgroundColor = 'skyblue';
    // スプライト画像作成
    var sprite = Sprite('tomapiko').addChildTo(this);
    // 初期位置
    sprite.x = this.gridX.center();
    sprite.y = this.gridY.center();
     // 毎フレーム処理
    sprite.update = function(app) {
      var key = app.keyboard;
      // 上下左右移動
      if (key.getKey('left')) { sprite.x -= SPEED; }
      if (key.getKey('right')) { sprite.x += SPEED; }
      if (key.getKey('up')) { sprite.y -= SPEED; }
      if (key.getKey('down')) { sprite.y += SPEED; }
    };
  },
});
/*
 * メイン処理
 */
phina.main(function() {
  // アプリケーションを生成
  var app = GameApp({
    title: 'キーボード操作',
    // MainScene から開始
    //startLabel: 'main',
    // アセット読み込み
    assets: ASSETS,
  });
  // fps表示
  //app.enableStats();
  // 実行
  app.run();
});
```