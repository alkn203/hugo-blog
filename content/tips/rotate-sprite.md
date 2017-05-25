+++
date = "2017-05-12T20:15:06+09:00"
draft = false
slug = ""
tags = ["tips","sprite", "update", "rotation"]
title = "Spriteを回転させる"
eyecatch = "/images/rotate-sprite.gif"
+++ 

## 今回のTips
スプライト画像を回転する方法について説明します。

![rotate-sprite.gif](/images/rotate-sprite.gif)

<center><a href="http://runstant.com/alkn203/projects/81cfe487" target="_blank">[runstantで確認]</a></center>

### スプライトの回転
**Sprite**の**update**関数でプロパティ**rotation**の値を変更することで回転させることができます。

```js
    // スプライト回転
    sprite.update = function() {
      sprite.rotation++;
    };
```

* 今回は**rotation**の値を1ずつ増やしています。

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
    // スプライト回転
    sprite.update = function() {
      sprite.rotation++;
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