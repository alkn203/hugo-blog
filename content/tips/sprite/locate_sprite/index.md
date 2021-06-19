+++
date = "2021-06-18T23:55:06+09:00"
draft = false
slug = ""
tags = ["sprite"]
title = "Spriteを表示して位置を指定する"
eyecatch = "locate-sprite.png"
+++

![locate-sprite](locate-sprite.png)

## Spriteをシーンに表示する
ゲーム作りで定番のスプライト画像を画面に表示します。

## アセットの定義
スプライトを表示するためには、アセットとして読み込む画像等を以下のように定義する必要があります。

```js
// アセット
var ASSETS = {
  // 画像
  image: {
    'tomapiko': 'https://cdn.jsdelivr.net/gh/phinajs/phina.js@develop/assets/images/tomapiko.png',
  },
};
```

* **image**はアセットの種類です。
* 連想配列の左側にキー名、右側に使用するアセットの場所を書きます。

## アセットの読み込み
読み込むためには、**main** 関数の**GameApp**のコンストラクタのプロパティ**assets**に定義したアセットを引き渡します。

```js
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

## Spriteの位置指定
**Shape**の位置指定と同様に行うことができます。

```js
// Sprite
 Sprite('tomapiko').addChildTo(this).setPosition(320, 480);
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
    // Sprite
    Sprite('tomapiko').addChildTo(this).setPosition(320, 480);
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
https://runstant.com/alkn203/projects/acd8dea9