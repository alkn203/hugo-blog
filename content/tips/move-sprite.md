+++
date = "2017-05-11T20:15:06+09:00"
draft = false
slug = ""
tags = ["tips","sprite", "asset", "update"]
title = "Spriteを移動させる"
eyecatch = "/images/move-sprite.gif"
+++ 

## 今回のTips
スプライト画像を移動する方法について説明します。

![move-sprite.gif](/images/move-sprite.gif)

<center><a href="http://runstant.com/alkn203/projects/bab85342" target="_blank">[runstantで確認]</a></center>

### スプライトの移動
**Sprite**の**update**関数に座標を変更する処理を書くと、毎フレーム呼ばれるので移動させることができます。

```js
    // スプライト移動
    sprite.update = function() {
      sprite.moveBy(2, 1);
    };
```

* 今回使った**moveBy**関数は引数で与えれた数値ずつ**x,y**の値を増やす処理をします。

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
    // スプライト作成
    var sprite = Sprite('tomapiko').addChildTo(this);
    // 初期位置
    sprite.x = this.gridX.center();
    sprite.y = this.gridY.center();
    // スプライト移動
    sprite.update = function() {
      sprite.moveBy(2, 1);
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
    // アセット読み込み
    assets: ASSETS,
  });
  // fps表示
  //app.enableStats();
  // 実行
  app.run();
});
```