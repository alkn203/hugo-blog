+++
date = "2016-11-13T00:01:08+09:00"
draft = false
slug = ""
tags = ["tips", "shape"]
title = "005-Shapeを透明化する"
eyecatch = "/images/phina-invisible-shape.png"
+++

## 内容
**Shape**を透明化する方法について説明します。

![phina-invisible-shape](/images/phina-invisible-shape.png)

## Shapeの透明化

透明にするには**alpha**プロパティに**0**を指定します。

```js
shape.alpha = 0;
```
<a href="http://runstant.com/alkn203/projects/7e1a191a" target="_blank">[runstantで確認]</a>

**alpha**プロパティには**0から1**が指定できるので、好みの透明度にすることができます。

## Shapeの非表示

**phina.js**では、透明化の他に**hide**メソッドでオブジェクトそのものを非表示にすることができます。

```js
shape.hide();
```

<a href="http://runstant.com/alkn203/projects/3df6a8b8" target="_blank">[runstantで確認]</a>

再表示するには、**show**メソッドを使います。

```js
shape.show();
```

## 透明化と非表示の違い
透明化ではオブジェクト自体は存在するので、当たり判定やタッチ処理は有効ですが、非表示にした場合は描画自体されないので、当たり判定やタッチ処理は無効になります。