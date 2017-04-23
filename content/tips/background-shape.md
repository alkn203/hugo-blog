+++
date = "2017-04-22T00:28:06+09:00"
draft = false
slug = ""
tags = ["tips","shape"]
title = "Shapeの背景色を指定する"
eyecatch = "/images/background-shape.gif"
+++ 

## 今回のTips
**Shape**の背景色を指定します。

![backgruond-shape.png](/images/background-shape.gif)

## 背景色指定

背景色は**backgroundColor**プロパティで指定します。

```js
shape.backgroundColor = 'red';
```
<a href="http://runstant.com/alkn203/projects/57cab291" target="_blank">[runstantで確認]</a>

## 他の指定方法

### 16進数
**css**と同様に指定できます。

```js
shape.backgroundColor = '#ffff00';
```
<a href="http://runstant.com/alkn203/projects/6a5a9d1d" target="_blank">[runstantで確認]</a>

### RGB
**Rgb**クラスを使って指定できます。

```js
shape.backgroundColor = `rgb(0, 255, 255)`;
```
<a href="http://runstant.com/alkn203/projects/cb651549" target="_blank">[runstantで確認]</a>

### hsl
**css**と同様に指定できます。

```js
shape.backgroundColor = `hsl(300, 75%, 50%)`;
```

<a href="http://runstant.com/alkn203/projects/accb7a5a" target="_blank">[runstantで確認]</a>