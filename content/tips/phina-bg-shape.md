+++
date = "2016-10-09T00:01:08+09:00"
draft = false
slug = ""
tags = ["tips", "shape"]
title = "004-Shapeの背景色指定"
eyecatch = "/images/phina-bg-shape.png"
+++

## 内容
**Shape**の背景色の指定方法について説明します。

![phina-bg-shape](/images/phina-bg-shape.png)

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
