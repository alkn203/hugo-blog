+++
date = "2017-05-02T00:15:06+09:00"
draft = false
slug = ""
tags = ["tips","shape", "event", "clear"]
title = "特定の種類のイベントを全て削除する"
eyecatch = "/images/touch-shape.gif"
+++ 

## 今回のTips
特定の種類のイベントを全て削除する方法について説明します。

![touch-shape.gif](/images/touch-shape.gif)

### clear メソッド
特定のイベントの登録を全て削除するには、以下のように**clear**を使います。

```js
shape.clear('enterframe');
```

以下の例では、**enterframe**イベントを2つ登録しており、タッチするとそれが削除されるようにしています。結果として、**shape**のサイズ拡大が止まります。

```js
    // 更新イベント登録
    shape.on('enterframe', function() {
      shape.width++;  
    });
    // 更新イベント更新登録
    shape.on('enterframe', function() {
      shape.height++;  
    });
    // タッチイベント登録
    shape.on('pointstart', function() {
      // 特定の種類のイベント登録を全て削除する
      shape.clear('enterframe');
    });
```
<a href="http://runstant.com/alkn203/projects/d6d50d82" target="_blank">[runstantで確認]</a>