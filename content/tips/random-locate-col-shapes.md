+++
date = "2017-04-23T15:13:06+09:00"
draft = false
slug = ""
tags = ["tips","shape", "random", "times", "hsl", "format"]
title = "Shapeをランダムな位置にたくさん表示する"
eyecatch = "/images/random-locate-shapes.gif"
+++ 

## 今回のTips
**Shape**を画面上のランダムな位置にたくさん表示します。
[前回のTips](http://alkn203.github.io/blog/2017/04/22/random-locate-shape/)に繰り返し処理を追加します。

![random-locate-shape.png](/images/random-locate-shapes.gif)

<center><a href="http://runstant.com/alkn203/projects/4d83a38d" target="_blank">[runstantで確認]</a></center>

### timesメソッドを使った繰り返し処理

**javascript**で繰り返し処理を行う時には、**for**を使うのが一般的です。
**phina.js**のTipsということもありますので、今回は元々の**Number**クラスの拡張メソッド**times**を使います。使い方は以下のとおりです。

```js
(10).times(function(i) {
  console.log(i);
});
```

* **function**の内部の処理を冒頭に指定された数の回数繰り返します。
* 変数**i**にインデックス値が入ってきますので、上の例では結果として**0から9**の数字が出力されます。
* 繰り返しが目的であれば、**i**の値を必ずしも使用する必要はありません。

### サンプルコード

```js
// グローバルに展開
phina.globalize();
// 定数
var SCREEN_WIDTH = 640;
var SCREEN_HEIGHT = 960;
var SHAPE_SIZE = 16;
var SHAPE_HALF = SHAPE_SIZE / 2;
var NUM = 50;
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
    // function内でthisを参照するための設定
    var self = this;
    // 繰り返し処理
    (NUM).times(function(i) {
      // Shapeを作成してシーンに追加
      var shape = Shape({
        width: SHAPE_SIZE,
        height: SHAPE_SIZE,
        backgroundColor: 'red',
      }).addChildTo(self);
      // 画面上に収まるランダムな位置に配置
      shape.x = Random.randint(SHAPE_HALF, SCREEN_WIDTH - SHAPE_HALF);
      shape.y = Random.randint(SHAPE_HALF, SCREEN_HEIGHT - SHAPE_HALF);
    });
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

* **times**メソッドの回数に指定する数値は変数でも良いので、このサンプルのように定数定義しておくと便利です。
* **times**メソッドの**function**内部で**this**を使うと**function**そのものを参照することになるので、その前に**var self = this**で退避して、内部では**self**を使うことで正しい参照になるようにしています。