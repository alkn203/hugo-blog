+++
date = "2016-10-01T00:01:06+09:00"
draft = false
slug = ""
tags = ["tips", "shape"]
title = "001-Shapeを生成してSceneに追加する"
eyecatch = "/images/phina-add-shape.png"
+++

## 内容
今回は基本的な内容ではありますが、オブジェクトの基本形である**Shape**を以下のように画面に表示してみます。

![phina_add_shape_image](/images/phina-add-shape.png)

<center>[[runstantで実行]](http://runstant.com/alkn203/projects/c5ac89af)</center>

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
    var shape = Shape();
    shape.addChildTo(this);
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
## コード説明

* **Shape**クラスのコンストラクタで生成します。コンストラクタの前に**new**をつける必要はありません。
* **addChildTo(this)**で現在の**Scene**に追加します。**this**は**MainScene**を指しています。

```js
this.addChild(shape)
```
としても同じですし
```js
var shape = Shape().addChildTo(this)
```
として1行にまとめることもできます。

* 位置が指定されていない時は、画面左上(0,0)に表示されます。
* 変数に代入しなくても表示されますが、後にプロパティを操作することが多いので、とりあえずは変数に代入しておいた方が良いでしょう。