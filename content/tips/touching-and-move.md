+++
date = "2017-05-24T23:19:06+09:00"
draft = false
slug = ""
tags = ["sprite", "onpointmove"]
title = "タッチしている位置に徐々にSpriteを移動させる"
eyecatch = "/images/onpointmove.gif"
+++ 

## 今回のTips
タッチしている位置に徐々にスプライトを移動させる方法について説明します。

![onpointmove.gif](/images/onpointmove.gif)

<center><a href="http://runstant.com/alkn203/projects/de52206d" target="_blank">[runstantで確認]</a></center>

### 移動先を管理して一定の割合で減速させる

```js
    // 移動先を管理する変数
    var targetX = sprite.x;
    var targetY = sprite.y;
    // タッチ開始
    this.onpointstart = function(e) {
      // 移動先を設定
      targetX = e.pointer.x;
      targetY = e.pointer.y;
    };
    // タッチ保持イベント
    this.onpointmove = function(e) {
      // 移動先を設定
      targetX = e.pointer.x;
      targetY = e.pointer.y;
    };
    // 更新
    this.update = function(app) {
      // タッチ中のみスプライトを動かす
      if (app.pointer.getPointing()) {
        // 徐々にタッチした位置に近づける
        var moveX = (targetX - sprite.x) * 0.25;
        var moveY = (targetY - sprite.y) * 0.25;
        sprite.moveBy(moveX, moveY);
      }
    };
```

* 移動先を管理する変数**targetX**と**targetY**を作成します。
* タッチ時に移動先を更新します。
* **app.pointer.getPointing()**で現在タッチされてるかを取得できます。
* 移動先とスプライトの現在の位置から方向ベクトルを求めて、一定割合で速度を減らして移動させます。

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
    // 移動先を管理する変数
  	var targetX = sprite.x;
  	var targetY = sprite.y;
		// タッチ開始
  	this.onpointstart = function(e) {
	    // 移動先を設定
	    targetX = e.pointer.x;
	    targetY = e.pointer.y;
	  };
    // タッチ保持イベント
    this.onpointmove = function(e) {
      // 移動先を設定
	    targetX = e.pointer.x;
	    targetY = e.pointer.y;
    };
    // 更新
    this.update = function(app) {
	    // タッチ中のみスプライトを動かす
	    if (app.pointer.getPointing()) {
    		// 徐々にタッチした位置に近づける
    		var moveX = (targetX - sprite.x) * 0.25;
    		var moveY = (targetY - sprite.y) * 0.25;
    		sprite.moveBy(moveX, moveY);
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
    title: '徐々に移動',
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