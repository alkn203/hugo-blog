+++
date = "2017-05-13T20:15:06+09:00"
draft = false
slug = ""
tags = ["tips","sprite", "update", "rotation"]
title = "Spriteを回転させる（setRotation）"
eyecatch = "/images/rotate-sprite.gif"
+++ 

## 今回のTips
スプライト画像の回転角度を指定する方法について説明します。

![rotate-sprite2.gif](/images/rotate-sprite2.gif)

<center><a href="http://runstant.com/alkn203/projects/7af9afc0" target="_blank">[runstantで確認]</a></center>

### スプライトの回転角度指定
**Sprite**の回転角度を指定するには、**setRotation**関数を使用します。

```js
// スプライト回転
sprite.setRotation(45);
```

* 引数はラジアン(radian)ではなく**度(degree)**です。

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
    // スプライト画像作成
    var sprite = Sprite('tomapiko').addChildTo(this);
    // 初期位置
    sprite.x = this.gridX.center();
    sprite.y = this.gridY.center();
    // スプライト回転
    sprite.setRotation(45);
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