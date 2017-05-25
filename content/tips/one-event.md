+++
date = "2017-05-02T00:13:06+09:00"
draft = false
slug = ""
tags = ["tips","shape", "event", "one"]
title = "一度だけ発行されるイベントを登録する"
eyecatch = "/images/touch-shape.gif"
+++ 

## 今回のTips
登録したイベントを１回だけ発行する方法について説明します。

![touch-shape.gif](/images/touch-shape.gif)

### one メソッド
１回だけ発行するイベントは**one**を使って登録すると便利です。

```js
    // タッチ関数（一度のみ実行）
  	shape.one('pointstart', function() {
	    alert("タッチされました");
  	});
```

<a href="http://runstant.com/alkn203/projects/551c7449" target="_blank">[runstantで確認]</a>

* 実行すると、タッチイベントが1回だけしか発生しないのが分かります。