+++
date = "2017-05-22T22:15:06+09:00"
draft = false
slug = ""
tags = ["tips","sprite", "frameanimaton", "freaquency"]
title = "フレームアニメーション速度を動的に変更する"
eyecatch = "/images/change-anim-speed.gif"
+++ 

## 今回のTips
外部ファイルとして読み込まれたフレームアニメーション設定を後から変更する方法について説明します。
今回はフレームアニメーションの速度を変更します。

![change-anim-speed.gif](/images/change-anim-speed.gif)

* 下のサンプルでは、スプライトをタッチする度に速度が遅くなっていきます。

<center><a href="http://runstant.com/alkn203/projects/d13981a6" target="_blank">[runstantで確認]</a></center>

### フレームアニメーション設定を外部ファイルに定義
フレームアニメーション設定は外部ファイルとして定義して、アセットとして読み込むこともできます。

```js
// フレームアニメーション情報
  spritesheet: {
    'tomapiko_ss': 'https://rawgit.com/phi-jp/phina.js/develop/assets/tmss/tomapiko.tmss',
  },
```


### アニメーションのfreaquencyプロパティ

```js
      // アニメーション速度変更
      anim.ss.getAnimation('left').frequency += 1;
```

* **anim**を**FrameAnimation**クラスのインスタンスとした場合、**ss**で**SpriteSheet**を参照することができます。
* **getAnimation**で指定したアニメーションを参照することができるので、そのプロパティ**freaquency**の値を変更します。

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
  // フレームアニメーション情報
  spritesheet: {
    'tomapiko_ss': 'https://rawgit.com/phi-jp/phina.js/develop/assets/tmss/tomapiko.tmss',
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
    var sprite = Sprite('tomapiko', 64, 64).addChildTo(this);
    // スプライトにフレームアニメーションをアタッチ
    var anim = FrameAnimation('tomapiko_ss').attachTo(sprite);
    // アニメーションを指定する
    anim.gotoAndPlay('left');
    // 初期位置
    sprite.x = this.gridX.center();
    sprite.y = this.gridY.center();
    // タッチを有効に
    sprite.setInteractive(true);
    // スプライトのタッチ処理
    sprite.onpointend = function() {
      // アニメーション速度変更
      anim.ss.getAnimation('left').frequency += 1;
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
