+++
date = "2019-07-02T20:55:06+09:00"
draft = false
slug = ""
tags = ["tweener","shape"]
title = "【phina.js】Tweenerとupdateを使ってShapeの色を変化させる"
eyecatch = "tweenercolor.gif"
+++

![tweenercolor.gif](tweenercolor.gif)

## はじめに
**phina.js**の**Tweener**はキャラクターの動きをコントロールする場合などに使えるとても勉強な機能です。今回は、**Tweener**と**update**を併用したTipsを紹介します。

## Shapeの色変化アニメーション
**Shape**の色を一定時間で変化させるアニメーションを作ります。方法は以下のとおりです。

 - **Shape**に**rgb**の3つのプロパティを追加する。
 - **Shape**の**tweener**でプロパティの値を変化させる。
 - **Shape**の**update**でプロパティの値を使って色を変化させる。

## プロパティを追加する
**javascript**は、後から好きな時にプロパティを追加できる柔軟さを持ってますので、これを利用します。

```javascript
// shape
var shape = StarShape().addChildTo(this);
shape.setPosition(this.gridX.center(), this.gridY.center());
shape.radius = 128;
// プロパティ追加
shape.r = 0;
shape.g = 0;
shape.b = 0;
```

## プロパティ値を変化させる
追加したプロパティ値を**tweener**で変化させます。

```javascript
// プロパティ値変化
shape.tweener.to({r: 10, g: 100, b: 200}, 2000)
             .to({r: 200, g: 200, b: 10}, 2000)
             .setLoop(true).play();
```

## 色を変化させる
- **update**で**Shape**の塗りつぶし色である**fill**にプロパティ値を代入します。
- **rgb**は文字列指定する必要があるため、**format**メソッドの引数から渡しています。

```javascript
// 毎フレーム処理
shape.update = function() {
  // 色指定
  shape.fill = 'rgb({0}, {1}, {2})'.format(shape.r, shape.g, shape.b);    
};
```

## まとめ
**phina.js**の**tweener**だけで完結する処理がメジャーですが、工夫次第でこのような表現もできるので、皆さんも色々と試してみて下さい。

## 実行サンプル

[サンプル(runstant)](https://runstant.com/alkn203/projects/a7341442)
