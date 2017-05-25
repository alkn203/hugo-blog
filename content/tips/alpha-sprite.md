+++
date = "2017-05-14T21:15:06+09:00"
draft = false
slug = ""
tags = ["tips","sprite", "alpha"]
title = "Spriteを透明にする"
eyecatch = "/images/alpha-sprite.gif"
+++ 

## 今回のTips
スプライトを透明にする方法について説明します。

![alpha-sprite.gif](/images/alpha-sprite.gif)

<center><a href="http://runstant.com/alkn203/projects/21f2a1e1" target="_blank">[runstantで確認]</a></center>

## スプライトを透明にする（alpha）
**Sprite**の**alpha**プロパティを変更します。初期値は**1.0**で**0**に近づくにつれて透明度が増していきます。

```js
    // 徐々に透明にする
    sprite.update = function() {
      if (sprite.alpha < 0.01) {
        sprite.alpha = 0;
        return;
      }
      sprite.alpha -= 0.01;
    };
```

* スプライトの**update**関数を利用して徐々に透明にしています。
* **canvas**の仕様で**alpha**が負の値になると透明度が無効になるため、条件式で**0**にしています。

##サンプルコード
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
    // 徐々に透明にする
    sprite.update = function() {
      if (sprite.alpha < 0.01) {
        sprite.alpha = 0;
        return;
      }
      sprite.alpha -= 0.01;
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