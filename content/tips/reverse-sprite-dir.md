+++
date = "2017-05-22T21:15:06+09:00"
draft = false
slug = ""
tags = ["tips","sprite", "frameanimaton", "scale"]
title = "Sprite の進行方向に応じて表示する向きを変更する"
eyecatch = "/images/reverse-sprite-dir.gif"
+++ 

## 今回のTips
前回のTipsでは、スプライトは右に移動しても左方向を向いたままです。
そこで今回は、スプライトの向きと移動方向が一致するようにします。

![reverse-sprite-dir.gif](/images/reverse-sprite-dir.gif)

<center><a href="http://runstant.com/alkn203/projects/3e687aa0" target="_blank">[runstantで確認]</a></center>

### スプライトと画面端との判定
**update**関数内に処理を追加します。

```js
    // 更新イベント
    sprite.update = function() {
      // 移動
      sprite.x += sprite.vx;
      // 画面端との判定
      if (sprite.left < 0 || 640 < sprite.right) {
        // 速度を反転する
        sprite.vx *= -1;
        // 向きを反転する
        sprite.scaleX *= -1;
      }
    };
```

* 付け加えたのは1行だけで、**sprite.scaleX *= -1**とすることで向きを反転させることができます。

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
        // 向きを反転する
        sprite.scaleX *= -1;
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