+++
date = "2017-05-24T22:19:06+09:00"
draft = false
slug = ""
tags = ["sprite", "onpointstart"]
title = "タッチした位置にSpriteを移動させる"
eyecatch = "/images/onpointstart.gif"
+++ 

## 今回のTips
タッチした位置にスプライトを移動させる方法について説明します。

![onpointstart.gif](/images/onpointstart.gif)

<center><a href="http://runstant.com/alkn203/projects/60f0531d" target="_blank">[runstantで確認]</a></center>

### イベントからタッチ座標を取得して移動させる

```js
    // タッチイベント
    this.onpointstart = function(e) {
      // スプライトをタッチ位置に
      sprite.x = e.pointer.x;
      sprite.y = e.pointer.y;
    };
```

* **this**つまり**MainScene**はデフォルトでタッチが有効になってますので、**onpointstart**でタッチイベントを取得します。
* **e.pointer**にタッチ座標が入っているので、**x, y**プロパティを使用してスプライトを移動させています。

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
// 定数
var SPEED = 4;
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
    // タッチイベント
    this.onpointstart = function(e) {
      // スプライトをタッチ位置に
      sprite.x = e.pointer.x;
      sprite.y = e.pointer.y;
    };
  },
});
/*
 * メイン処理
 */
phina.main(function() {
  // アプリケーションを生成
  var app = GameApp({
    title: 'タッチ先に移動',
    // MainScene から開始
    //startLabel: 'main',
    // アセット読み込み
    assets: ASSETS,
  });
  // fps表示
  //app.enableStats();
  // 実行
  app.run();
});
```