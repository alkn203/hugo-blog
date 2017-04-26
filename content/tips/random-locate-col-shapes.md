+++
date = "2017-04-23T15:13:06+09:00"
draft = false
slug = ""
tags = ["tips","shape", "random", "times", "hsl", "format"]
title = "カラフルなShapeをランダムな位置にたくさん表示する"
eyecatch = "/images/random-locate-col-shapes.gif"
+++ 

## 今回のTips
様々な色の**Shape**を画面上のランダムな位置にたくさん表示します。
[Shapeをランダムな位置にたくさん表示する](http://alkn203.github.io/blog/2017/04/22/random-locate-shapes/)に色指定を追加します。

![random-locate-col-shape.png](/images/random-locate-col-shapes.gif)

<center><a href="http://runstant.com/alkn203/projects/7058dacc" target="_blank">[runstantで確認]</a></center>

## hslとformatメソッドを使った色指定

* **HSL色空間**は、**色相**（Hue）、**彩度**（Saturation）、**輝度**（Lightness）で色を指定します。使い方は、[Shapeの背景色を指定する](http://alkn203.github.io/blog/2017/04/22/background-shape/)を参考にして下さい。
* **format**は、**String**クラスの拡張メソッドで、引数で与えられた値を文字列に引き渡します。以下の様に使います。

```js
var hue = Random.randint(0, 360);
var color = 'hsl({0}, 75%, 50%)'.format(hue);
```
* 上の例ですと、変数**hue**に代入されたランダムな整数値を**format**メソッドに与えることで前段の文字列内の**{0}**が**hue**の値に置き換わります。
* **'hsl({0}, {1}, 50%)'.format(hue, saturation)**のように引数を増やすこともできます。

## サンプルコード

```js
// グローバルに展開
phina.globalize();
// 定数
var SCREEN_WIDTH = 640;
var SCREEN_HEIGHT = 960;
var SHAPE_SIZE = 16;
var SHAPE_HALF = SHAPE_SIZE / 2;
var NUM = 50;
var SATURATION = 100;	// 彩度
var LIGHTNESS = 50;	// 輝度
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
      }).addChildTo(self);
      // 画面上に収まるランダムな位置に配置
      shape.x = Random.randint(SHAPE_HALF, SCREEN_WIDTH - SHAPE_HALF);
      shape.y = Random.randint(SHAPE_HALF, SCREEN_HEIGHT - SHAPE_HALF);
      // 背景色をランダムに設定
      var hue = Random.randint(0, 360);
      shape.backgroundColor = 'hsl({0}, 75%, 50%)'.format(hue);
    });
  },
});
/*
 * メイン処理
 */phina.main(function() {
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
