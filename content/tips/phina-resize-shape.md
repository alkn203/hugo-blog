+++
date = "2016-10-06T00:01:08+09:00"
draft = false
slug = ""
tags = ["tips", "shape"]
title = "003-Shapeの幅・高さ指定"
eyecatch = "/images/phina-resize-shape.png"
+++

## 内容
**Shape**の幅・高さの指定方法について説明します。

![phina-resize-shape](/images/phina-resize-shape.png)

## 幅指定

幅は**width**プロパティで指定します。

```js
shape.width = 128;
```
[[runstantで確認]](http://runstant.com/alkn203/projects/7fad0439)

## 高さ指定

高さは**height**プロパティで指定します。

```js
shape.height = 128;
```
[[runstantで確認]](http://runstant.com/alkn203/projects/5da92e05)

## 幅・高さを一括指定

**setSize**を使えば、幅と高さを一括で指定できます。

```js
shape.setSize(128, 256);
```
[[runstantで確認]](http://runstant.com/alkn203/projects/8de8694c)

## コンストラクタ内で指定
位置指定と同じくコンストラクタ内で幅・高さを指定することも可能です。

```js
    var shape = Shape({
      // 位置・幅・高さ指定
      x: 320,
      y: 480,
      width: 128,
      height: 256,
    }).addChildTo(this);
```
[[runstantで確認]](http://runstant.com/alkn203/projects/1a3e59cf)
