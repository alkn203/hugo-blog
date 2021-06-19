+++
date = "2021-06-19T11:55:06+09:00"
draft = false
slug = ""
tags = ["sprite"]
title = "Spriteを拡大・縮小させる"
eyecatch = "scale-sprite.gif"
+++

![scale-sprite](scale-sprite.gif)

**Sprite** は、**Shape** と同じ基底クラスを継承していますので、共通のプロパティやメソッドを使用することができます。

## Spriteの拡大縮小

```js
// 拡大・縮小
var sp4 = Sprite('tomapiko').addChildTo(this).setPosition(200, 800);
sp4.scaleY = 1.5;
// setScale
Sprite('tomapiko').addChildTo(this).setPosition(440, 800).setScale(0.5, 0.5);
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
    // 拡大・縮小
    var sp4 = Sprite('tomapiko').addChildTo(this).setPosition(200, 800);
    sp4.scaleY = 1.5;
    Sprite('tomapiko').addChildTo(this).setPosition(440, 800).setScale(0.5, 0.5);
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
https://runstant.com/alkn203/projects/25819771