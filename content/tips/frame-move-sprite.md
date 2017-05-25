+++
date = "2017-05-16T21:15:06+09:00"
draft = false
slug = ""
tags = ["tips","sprite", "frameanimaton"]
title = "Spriteをフレームアニメーションしながら移動させる"
eyecatch = "/images/frame-move-sprite.gif"
+++ 

## 今回のTips
スプライトをフレームアニメーションしながら移動させる方法について説明します。

![frame-move-sprite.gif](/images/frame-move-sprite.gif)

<center><a href="http://runstant.com/alkn203/projects/762f81e9" target="_blank">[runstantで確認]</a></center>

### スプライトを移動させる
フレームアニメーションは自動で管理されていますので、**update**メソッド内に移動処理を書くだけで大丈夫です。

```js
    // 更新イベント
    sprite.update = function() {
      // 移動
      sprite.x -= 2;
    };
```

### サンプルコード
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
    // 更新イベント
    sprite.update = function() {
      // 移動
      sprite.x -= 2;
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