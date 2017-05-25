+++
date = "2017-05-26T22:19:06+09:00"
draft = false
slug = ""
tags = ["sprite", "remove"]
title = "タッチしたスプライトを消す"
eyecatch = "/images/touch-remove.gif"
+++ 

## 今回のTips
タッチされたスプライトを消す方法について説明します。

![touch-remove.gif](/images/touch-remove.gif)

<center><a href="http://runstant.com/alkn203/projects/dbd8507a" target="_blank">[runstantで確認]</a></center>

### removeメソッド

```js
    // スプライト画像を大量作成
    (32).times(function(i) {
      var sprite = Sprite('tomapiko').addChildTo(self);
      var half = sprite.width / 2;
      // ランダムな位置
      sprite.x = Random.randint(half, 640 - half);
      sprite.y = Random.randint(half, 960 - half);
      // タッチを有効にする
      sprite.setInteractive(true);
      // タッチイベント登録
      sprite.onpointstart = function() {
        // 削除
        sprite.remove();
      };
    });
```

* **times**関数を使って、スプライトの作成を指定数繰り返しています。
* **Random**クラスの**randint**メソッドを使って、スプライトをランダムな位置に表示します。
* スプライトのタッチを有効にするために**setInteractive(true)**を忘れないようにします。
* スプライトの**onpointstart**イベントの中で**remove**を使ってスプライトを消しています。

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
    var self = this;
    // スプライト画像を大量作成
    (32).times(function(i) {
      var sprite = Sprite('tomapiko').addChildTo(self);
      var half = sprite.width / 2;
      // ランダムな位置
      sprite.x = Random.randint(half, 640 - half);
      sprite.y = Random.randint(half, 960 - half);
      // タッチを有効にする
      sprite.setInteractive(true);
      // タッチイベント登録
      sprite.onpointstart = function() {
        // 削除
        sprite.remove();
      };
    });
  },
});
/*
 * メイン処理
 */
phina.main(function() {
  // アプリケーションを生成
  var app = GameApp({
    title: 'タッチして削除',
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