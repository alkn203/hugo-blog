+++
date = "2017-04-22T15:13:06+09:00"
draft = false
slug = ""
tags = ["tips","shape", "random"]
title = "Shapeをランダムな位置に表示する"
eyecatch = "/images/random-locate-shape.gif"
+++ 

## 今回のTips
実行する度に**Shape**を画面上のランダムな位置に表示します。

![random-locate-shape.png](/images/random-locate-shape.gif)

<center><a href="http://runstant.com/alkn203/projects/4b44a4dd" target="_blank">[runstantで確認]</a></center>

### Randomクラスを使う

ランダムな位置に表示するためには、ランダムな数値が必要です。**phina.js**にはこれを取り扱う**phina.util.Random**クラスが用意されています。
今回は**Random**クラスの**randint**メソッドを使います。使い方は以下のとおりです。

```js
var rnd = Random.randint(0, 100);
```

上の例ですと、変数**rnd**に**0から100**の間でランダムな**整数**が1つ代入されます。

### 使用例

```js
shape.x = Random.randint(SHAPE_HALF, SCREEN_WIDTH - SHAPE_HALF);
shape.y = Random.randint(SHAPE_HALF, SCREEN_HEIGHT - SHAPE_HALF);
```

* **SHAPE_HALF**は**Shape**のサイズの半分の値で、**Shape**が画面からはみ出さないように**randint**の範囲を調整しています。半分にしたのは、**Shape**の座標は**中心が起点(origin)**になっているためです。

## コード

```js
// グローバルに展開
phina.globalize();
// 定数
var SCREEN_WIDTH = 640;
var SCREEN_HEIGHT = 960;
var SHAPE_SIZE = 16;
var SHAPE_HALF = SHAPE_SIZE / 2;
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
    this.backgroundColor = 'black';
    // Shapeを作成してシーンに追加
    var shape = Shape({
      width: SHAPE_SIZE,
      height: SHAPE_SIZE,
      backgroundColor: 'red',
    }).addChildTo(this);
    // 画面上に収まるランダムな位置に配置
    shape.x = Random.randint(SHAPE_HALF, SCREEN_WIDTH - SHAPE_HALF);
    shape.y = Random.randint(SHAPE_HALF, SCREEN_HEIGHT - SHAPE_HALF);
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
  });
  // fps表示
  //app.enableStats();
  // 実行
  app.run();
});
```