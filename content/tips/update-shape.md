+++
date = "2017-04-26T00:13:06+09:00"
draft = false
slug = ""
tags = ["tips","shape", "update", "enterframe"]
title = "Shapeの更新イベントを登録する"
eyecatch = "/images/update-shape.gif"
+++ 

## 今回のTips
**Shape**に対してフレーム更新処理を登録する方法について説明します。

![upate-shape.gif](/images/update-shape.gif)

### 更新イベント処理（update）

更新処理を登録するためには、**shape**の**update**関数を実装します。
**update**関数が実装されていると、その内容が毎フレーム実行されます。

```js
// 毎フレーム更新処理
shape.update = function() {
  // 移動
  shape.moveBy(1, 1);  
};
```

<a href="http://runstant.com/alkn203/projects/d709760f" target="_blank">[runstantで確認]</a>

### 更新イベント処理（on + enterframe）

**update**では1つのメソッドしか登録できません。一方、**on**と**enterframe**を組み合わせると複数のメソッドを登録可能で、その内容が並行して実行されます。

```js
// 毎フレーム更新処理
shape.on('enterframe', function() {
  // 移動
  shape.moveBy(1, 1);  
});
// 毎フレーム更新処理
shape.on('enterframe', function() {
  // 回転
  shape.rotation++;  
});
```

<a href="http://runstant.com/alkn203/projects/0705ecfc" target="_blank">[runstantで確認]</a>