+++
date = "2017-05-02T20:15:06+09:00"
draft = false
slug = ""
tags = ["tips","sprite", "asset"]
title = "Spriteを表示する"
eyecatch = "/images/display-sprite.gif"
+++ 

## 今回のTips
ゲーム作成ではポピュラーなスプライト画像を表示する方法について説明します。

![display-sprite.gif](/images/display-sprite.gif)

<center><a href="http://runstant.com/alkn203/projects/91bff4d6" target="_blank">[runstantで確認]</a></center>

### アセットの読み込み
スプライトを表示するためには、まず使用する画像をアセットとして読み込む必要があります。

### アセットの定義
アセットは以下のように定義します。

```js
// アセット
var ASSETS = {
  // 画像
  image: {
    'tomapiko': 'https://rawgit.com/phi-jp/phina.js/develop/assets/images/tomapiko.png',
  },
};
```

* **image**はアセットの種類です。
* 連想配列の左側にキー名、右側に使用するアセットの場所を書きます。

### アセットの読み込み
読み込むためには、メイン関数の**GameApp**のコンストラクタのプロパティ**assets**に定義したアセットを引き渡します。

```js
phina.main(function() {
  // アプリケーションを生成
  var app = GameApp({
    // MainScene から開始
    startLabel: 'main',
    // アセット読み込み
    assets: ASSETS,
  });
```

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