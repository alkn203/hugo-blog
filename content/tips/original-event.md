+++
date = "2017-04-28T00:13:06+09:00"
draft = false
slug = ""
tags = ["tips","shape", "event", "flare"]
title = "Shapeに独自のイベントを登録する"
eyecatch = "/images/touch-shape.gif"
+++ 

## 今回のTips
**Shape**に対して独自のイベントを登録・発行する方法について説明します。

![touch-shape.gif](/images/touch-shape.gif)

### 独自イベントの登録（on + イベント名）

独自イベントを登録するためには、**on** + **イベント名**の関数を**shape**に実装します。
今回はイベント名を**hit**としたいので**onhit**としました。

```js
　　// hitイベント登録
    shape.onhit = function() {
      alert('Hit!');  
    };
```

### 独自イベント発行

イベントの発行は、**flare(イベント名)**で行います。

```js
     // タッチイベント登録
    shape.onpointstart = function() {
      // hitイベント発行
      shape.flare('hit');
    };
```

<a href="http://runstant.com/alkn203/projects/c57dbef8" target="_blank">[runstantで確認]</a>