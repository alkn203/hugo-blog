+++
date = "2017-05-20T21:15:06+09:00"
draft = false
slug = ""
tags = ["tips","sprite", "frameanimaton"]
title = "Spriteを画面外に出ないように制御する"
eyecatch = "/images/bounce-move-sprite.gif"
+++ 

## 今回のTips
前回のTipsではスプライトは左に向かって移動しますが、最後は画面端から消えてしまいます。そこで今回は、スプライトを画面外に出ないように制御する方法について説明します。

![bounce-move-sprite.gif](/images/bounce-move-sprite.gif)

<center><a href="http://runstant.com/alkn203/projects/d62ea254" target="_blank">[runstantで確認]</a></center>

### スプライトと画面端との判定
**update**関数内に処理を追加します。

```js
    // 初期速度
    sprite.vx = -2;
    // 更新イベント
    sprite.update = function() {
      // 移動
      sprite.x += sprite.vx;
      // 画面端との判定
      if (sprite.left < 0 || 640 < sprite.right) {
        // 速度を反転する
        sprite.vx *= -1;
      }
    };
```

* 関数の前で、後に計算し易いように初期速度を**vx**という変数に一旦入れます。
* 画面左端は**0**で、デフォルトで右端は**640**ですので、スプライトの**left**と**right**でそれぞれ判定して、いずれかを超えた場合は速度を反転するようにしています。

### コード
```js
// グローバルに展開
phina.globalize();
// アセット
var ASSETS = {
  // 画像
  image: {
    'tomapiko': 'https://rawgit.com/phi-jp/phina.js/develop/assets/images/tomapiko_ss.png',
  },
  // スプライトシート
  spritesheet: {
    "tomapiko_ss":
    {
      // フレーム情報
      "frame": {
        "width": 64, // 1フレームの画像サイズ（横）
        "height": 64, // 1フレームの画像サイズ（縦）
        "cols": 6, // フレーム数（横）
        "rows": 3, // フレーム数（縦）
      },
      // アニメーション情報
      "animations" : {
        "walk": { // アニメーション名
          "frames": [12,13,14], // フレーム番号範囲
          "next": "walk", // 次のアニメーション
          "frequency": 6, // アニメーション間隔
        },
      }
    },
  }
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
    var sprite = Sprite('tomapiko', 64, 64).addChildTo(this);
    // スプライトにフレームアニメーションをアタッチ
    var anim = FrameAnimation('tomapiko_ss').attachTo(sprite);
    // アニメーションを指定する
    anim.gotoAndPlay('walk');
    // 初期位置
    sprite.x = this.gridX.center();
    sprite.y = this.gridY.center();
    // 初期速度
    sprite.vx = -2;
    // 更新イベント
    sprite.update = function() {
      // 移動
      sprite.x += sprite.vx;
      // 画面端との判定
      if (sprite.left < 0 || 640 < sprite.right) {
        // 速度を反転する
        sprite.vx *= -1;
      }
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