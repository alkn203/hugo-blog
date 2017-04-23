+++
date = "2017-04-20T00:28:06+09:00"
draft = false
slug = ""
tags = ["tips","shape"]
title = "Shapeを生成してSceneに追加する"
eyecatch = "/images/locate-shape.gif"
+++

## 今回のTips

オブジェクトの基本形である**Shape**を以下のように画面に表示します。
位置を指定しなければ、画面左上に表示されます。

![locate-shape](/images/locate-shape.gif)

<center>[[runstantで確認]](http://runstant.com/alkn203/projects/c5ac89af)</center>

### Shapeの追加

```js
    // Shapeを作成してシーンに追加
    Shape().addChildTo(this);
```

* **Shape**クラスを使って生成します。
* **phina.js**は、コンストラクタの前に**new**をつける必要はありません。
* **addChildTo(this)**で現在の**Scene**に追加します。**this**は**MainScene**を指しています。
* 位置が指定されていない時は、画面左上(0,0)を中心として表示されます。

## コード

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
    this.backgroundColor = 'black';
    // Shapeを作成してシーンに追加
    Shape().addChildTo(this);
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
