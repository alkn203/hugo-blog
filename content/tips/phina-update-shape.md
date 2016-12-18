+++
date = "2016-11-29T00:01:08+09:00"
draft = false
slug = ""
tags = ["tips", "shape", "event"]
title = "006-Shapeの更新イベント処理"
eyecatch = "/images/phina-update-shape.png"
+++

## 内容
**Shape**に対してフレーム更新処理を登録する方法について説明します。

![phina-invisible-shape](/images/phina-update-shape.png)

## 更新イベント処理（update）

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

## 更新イベント処理（on + enterframe）

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