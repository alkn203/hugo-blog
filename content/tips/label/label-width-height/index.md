+++
date = "2021-06-22T11:04:06+09:00"
draft = false
slug = ""
tags = ["label"]
title = "Labelの幅と高さを調べる"
eyecatch = "label-width-height.png"
+++

![label-width-height](label-width-height.png)

## ラベルの幅と高さを調べる
**Label**クラスは**Shape**クラスを継承しているので、デフォルトでは**64X64**のサイズが設定されています。そこで、実際の**Label**の幅と高さを調べる方法について説明します。

## calcCanvasHeight関数
ラベルの実際の高さを求めます。

```js
var height = label.calcCanvasHeight();
```

## calcCanvasWidth関数
ラベルの実際の幅を求めます。

```js
var width = label.calcCanvasWidth();
```

## サンプルコード
<details>
<summary>コードを見る</summary>

```js
// グローバルに展開
phina.globalize();
/*
 * メインシーン
 */
phina.define("MainScene", {
  // 継承
  superClass: 'DisplayScene',
  // 初期化
  init: function() {
    // 親クラス初期化
    this.superInit();
    // 背景色
    this.backgroundColor = 'silver';
    // ラベル表示
    var label = Label('Time is money').addChildTo(this);
    label.setPosition(this.gridX.center(), this.gridY.center());
    // 実際のサイズを算出
    label.width = label.calcCanvasWidth();
    label.height = label.calcCanvasHeight();
    // 枠を表示
    RectangleShape({
      width: label.width,
      height: label.height,
      fill: null,
      stroke: 'red',
    }).addChildTo(label);
  },
});
/*
 * メイン処理
 */
phina.main(function() {
  // アプリケーションを生成
  var app = GameApp({
    // MainScene から開始
    startLabel: 'main',
  });
  // fps表示
  //app.enableStats();
  // 実行
  app.run();
});
```

</details>

## runstantプロジェクト
https://runstant.com/alkn203/projects/b25eec2d