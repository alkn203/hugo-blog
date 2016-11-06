+++
date = "2016-10-03T00:01:08+09:00"
draft = false
slug = ""
tags = ["tips", "shape"]
title = "002-Shapeの位置指定"
eyecatch = "/images/phina-locate-shape.png"
+++

## 内容
**Shape**の位置を以下のように指定します。

![phina_locate_shape_image](/images/phina-locate-shape.png)

## コード

```js
// グローバルに展開
phina.globalize();
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
    var shape = Shape().addChildTo(this);
    // 移動
    shape.x = 320;
    shape.y = 480;
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
[[runstantで実行]](http://runstant.com/alkn203/projects/4e8bc6e6)
## コード説明
* **Shape**の位置変更は、**x, y**プロパティに直接数値を指定することで可能です。

## 他の位置指定方法
* **setPosition**関数を使えば、**x, y**の値を一括で指定することができ、生成から一気にチェインメソッドで繋げて書くこともできるので便利です。

```js
var shape = Shape().addChildTo(this).shape.setPosition(320, 480);
```
[[runstantで実行]](http://runstant.com/alkn203/projects/4c21e518)

* **Shape**のコンストラクタにパラメータとして与えてることでも指定できます。

```js
var shape = Shape({
  x: 320,
  y: 480
}).addChildTo(this)
```
[[runstantで実行]](http://runstant.com/alkn203/projects/81ae38a5)

* **moveBy**関数を使えば、**x, y**の移動量で位置を変更することができます。

```js
shape.setPosition(320, 480).moveBy(100, 200);
```
[[runstantで実行]](http://runstant.com/alkn203/projects/af6327d3)

* 変則的ですが、**Vector2**クラスを使ってベクトル値の加算で位置指定する方法もあります。

```js
var v = Vector2(100, 200);
shape.position.add(v);
```
[[runstantで実行]](http://runstant.com/alkn203/projects/6a11250f)