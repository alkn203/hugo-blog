+++
date = "2017-05-13T21:15:06+09:00"
draft = false
slug = ""
tags = ["tips","sprite", "scale"]
title = "Spriteを拡大・縮小させる（setScale使用）"
eyecatch = "/images/resize-sprite.gif"
+++ 

## 今回のTips
スプライト画像に拡大・縮小率を指定する方法について説明します。

![resize-sprite.gif](/images/resize-sprite.gif)

<center><a href="http://runstant.com/alkn203/projects/e7e075a0" target="_blank">[runstantで確認]</a></center>

### スプライトの拡大・縮小（setScale）
**Sprite**の拡大・縮小率を指定するには**setScale**関数を使用します。

```js
    // スプライト拡大
    sprite.setScale(2.0, 2.0);
```

* **1.0**を基準として、小さければ縮小、大きければ拡大になります。

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
    // スプライト拡大
    sprite.setScale(2.0, 2.0);
  },
});
/*
 * メイン処理
 */
phina.main(function() {
  // アプリケーションを生成
  var app = GameApp({
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