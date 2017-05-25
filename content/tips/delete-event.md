+++
date = "2017-04-30T00:13:06+09:00"
draft = false
slug = ""
tags = ["tips","shape", "event", "off"]
title = "登録したイベントを削除する"
eyecatch = "/images/touch-shape.gif"
+++ 

## 今回のTips
登録したイベントを削除する方法について説明します。

![touch-shape.gif](/images/touch-shape.gif)

## イベントの削除

イベントを削除するためには、**off**メソッドを使います。

```js
    // タッチ関数（一度のみ実行）
  	shape.one('pointstart', function() {
	    alert("タッチされました");
  	});
```

<a href="http://runstant.com/alkn203/projects/8ce700ac" target="_blank">[runstantで確認]</a>

* **off**では第2引数に関数を指定する必要があるため、**touchFunc**として別に定義しています。
* 実行すると、タッチイベントが1回だけしか発生しないのが分かります。