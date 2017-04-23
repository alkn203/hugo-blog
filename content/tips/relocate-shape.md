+++
date = "2017-04-21T00:28:06+09:00"
draft = false
slug = ""
tags = ["tips","shape"]
title = "Shapeの位置を指定する"
eyecatch = "/images/relocate-shape.gif"
+++

## 今回のTips
**Shape**の位置を以下のように指定します。

![locate-shape](/images/relocate-shape.gif)

<center>[[runstantで確認]](http://runstant.com/alkn203/projects/4e8bc6e6)</center>

### Shapeの位置指定

```js
// Shapeを作成してシーンに追加
var shape = Shape().addChildTo(this);
// 移動
shape.x = 320;
shape.y = 480;
```

* **Shape**の位置変更は、**x, y**プロパティに直接数値を指定することで可能です。

### 他の位置指定方法

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

* 他に、**Vector2**クラスを使ってベクトル値の加算で位置指定する方法もあります。

```js
var v = Vector2(100, 200);
shape.position.add(v);
```
[[runstantで実行]](http://runstant.com/alkn203/projects/6a11250f)
