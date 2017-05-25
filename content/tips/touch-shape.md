+++
date = "2017-04-27T00:13:06+09:00"
draft = false
slug = ""
tags = ["tips","shape", "touch", "event", "pointstart"]
title = "Shapeのタッチイベントを登録する"
eyecatch = "/images/touch-shape.gif"
+++ 

## 今回のTips
**Shape**に対してタッチイベントを登録する方法について説明します。

![touch-shape.gif](/images/touch-shape.gif)

### タッチイベント（onpointstart）

タッチイベントを登録するためには、**shape**の**onpointstart**関数を実装します。
その前に、**setInteractive(true)**としてタッチ自体を有効にする必要があります。

```js
    // タッチ可能にする
    shape.setInteractive(true);
    // タッチイベント登録
    shape.onpointstart = function() {
      alert('タッチされました');
    };
};
```

<a href="http://runstant.com/alkn203/projects/f25e4ad6" target="_blank">[runstantで確認]</a>

### タッチイベント登録（on + pointstart）

**on**と**enterframe**の組み合わせの時と同様に複数のメソッドを登録可能で、その内容が登録順に実行されます。

```js
    // タッチイベント登録
    shape.on('pointstart', function() {
      alert('タッチされました');
    });
    // タッチイベント登録
    shape.on('pointstart', function() {
      shape.backgroundColor = 'blue';  
    });
```

<a href="http://runstant.com/alkn203/projects/fe769fe5" target="_blank">[runstantで確認]</a>