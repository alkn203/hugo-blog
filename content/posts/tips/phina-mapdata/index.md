+++
date = "2019-08-17T20:55:06+09:00"
draft = true
slug = ""
tags = ["phina.js","tips","map"]
title = "【phina.js】マップデータの読み込みにおける一工夫"
eyecatch = "mycollision.gif"
+++

![mycollision.gif](mycollision.gif)

### はじめに
ゲーム作りにおいてステージを作成する時、オブジェクトが少ない場合は一つ一つ位置を指定して追加しても良いかもしれませんが
最終的には2次元配列でマップデータを作成して、プログラム内から読み込むのがメジャーな方法です。

### 2次元配列のマップデータ
通常は以下のようなデータを作成することになると思います。

```javascript

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
  // コンストラクタ
  init: function() {
    // 親クラス初期化
    this.superInit();
    // 背景
    this.backgroundColor = 'skyblue';
    // 判定対象
    var shape = Shape({
      backgroundColor: 'blue',
      x: this.gridX.center(),
      y: this.gridY.center(),
      width: 100,
      height: 100,
    }).addChildTo(this);
    // 当たり判定用の矩形
    var rect = RectangleShape({
      width: 40,
      height: 54,
      fill: null,
      stroke: null,
    }).addChildTo(this);

    rect.x = this.gridX.span(1);
    rect.y = this.gridY.span(1);
    // スプライトを子要素として追加
    Sprite('tomapiko').addChildTo(rect);
    // タッチイベント
    this.onpointmove = function(e) {
      // スプライトをタッチ位置に
      rect.x = e.pointer.x;
      rect.y = e.pointer.y;
    };
    // 更新処理
    this.update = function() {
      // 矩形判定
      if (rect.hitTestElement(shape)) {
        shape.backgroundColor = 'red';
      }
      else {
        shape.backgroundColor = 'blue';
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