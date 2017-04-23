+++
date = "2017-04-22T12:13:06+09:00"
draft = false
slug = ""
tags = ["tips","shape", "alpha"]
title = "Shapeを透明化・非表示にする"
eyecatch = "/images/phina-template.gif"
+++ 

## 今回のTips
**Shape**を透明化または非表示にします。

![phina-template.png](/images/phina-template.gif)

### Shapeの透明化

透明にするには**alpha**プロパティに**0**を指定します。

```js
shape.alpha = 0;
```
<a href="http://runstant.com/alkn203/projects/7e1a191a" target="_blank">[runstantで確認]</a>

**alpha**プロパティには**0から1**が指定できるので、好みの透明度にすることができます。

### Shapeの非表示

**phina.js**では、透明化の他に**hide**メソッドでオブジェクトそのものを非表示にすることができます。

```js
shape.hide();
```

<a href="http://runstant.com/alkn203/projects/3df6a8b8" target="_blank">[runstantで確認]</a>

再表示するには、**show**メソッドを使います。

```js
shape.show();
```

### 透明化と非表示の違い
透明化ではオブジェクト自体は存在するので、当たり判定やタッチ処理は有効ですが、非表示にした場合は描画自体されないので、当たり判定やタッチ処理は無効になります。