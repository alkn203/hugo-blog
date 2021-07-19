+++
date = "2021-06-16T19:55:06+09:00"
draft = false
slug = ""
tags = ["shape"]
title = "Shapeの背景色を指定する"
eyecatch = "color-shape.png"
+++

![color-shape](color-shape.png)

## Shapeの背景色指定

**Shape**の背景色は**backgroundColor**プロパティで指定します。 **CSS**と同じ感覚で指定できます。

#### 文字列で指定
```js
shape.backgroundColor = 'red';
```

#### 16進数で指定
```js
shape.backgroundColor = '#ffff00';
```

#### RGB値で指定
```js
shape.backgroundColor = `rgb(0, 255, 255)`;
```
#### hsl値で指定
```js
shape.backgroundColor = `hsl(300, 75%, 50%)`;
```

## サンプルコード
<details>
<summary>コードを見る</summary>

```js
/// グローバルに展開
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
    this.backgroundColor = 'black';
    // 背景色（文字列）
    var shape = Shape().addChildTo(this).setPosition(320, 480);
    shape.backgroundColor = 'red';
    // 背景色（RGB値）
    var shape1 = Shape().addChildTo(this).setPosition(150, 480);
    shape1.backgroundColor = '#ffff00';
    // 背景色（hsl値）
    var shape2 = Shape().addChildTo(this).setPosition(490, 480);
    shape2.backgroundColor = 'hsl(300, 75%, 50%)';
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
{{< targetblank "https://runstant.com/alkn203/projects/017932f4" >}}