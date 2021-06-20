+++
date = "2021-06-19T11:57:06+09:00"
draft = false
slug = ""
tags = ["sprite"]
title = "Spriteの向きを反転させる"
eyecatch = "reverse-sprite.gif"
+++

![reverse-sprite](reverse-sprite.gif)

**Sprite**の向きは、専用の画像を使用せずに反転することができます。

## Spriteの向き反転
* 横方向に反転したい場合は、プロパティ**scaleX**に-1を乗じます。
* 縦方向に反転したい場合は、プロパティ**scaleY**に-1を乗じます。

以下のコードは、画面タッチでスプライトを反転表示させる例です。

```js
// 画面タッチ時処理
this.onpointstart = function() {
  // 横向き反転
  sprite.scaleX *= -1;
};
```

## サンプルコード
<details>
<summary>コードを見る</summary>

```js
// グローバルに展開
phina.globalize();
// アセット
var ASSETS = {
  // 画像
  image: {
    'tomapiko': 'https://cdn.jsdelivr.net/gh/phinajs/phina.js@develop/assets/images/tomapiko.png',
  },
};
/*
 * メインシーン
 */
phina.define("MainScene", {
  // 継承
  superClass: 'DisplayScene',
  // 初期化
  init: function() {
    // 親クラス初期化
    this.superInit();
    // 背景色
    this.backgroundColor = 'skyblue';
    // スプライト画像作成
    var sprite = Sprite('tomapiko', 64, 64).addChildTo(this);
    sprite.setPosition(320, 480);
    // 画面タッチ時処理
    this.onpointstart = function() {
      // 横向き反転
      sprite.scaleX *= -1;
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

</details>

## runstantプロジェクト
https://runstant.com/alkn203/projects/04522bf7