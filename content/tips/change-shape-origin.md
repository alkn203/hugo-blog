+++
date = "2017-04-25T00:13:06+09:00"
draft = false
slug = ""
tags = ["tips","shape", "origin"]
title = "Shapeの原点を変更する"
eyecatch = "/images/change-shape-origin.gif"
+++ 

## 今回のTips
**phina.js**では、デフォルトでオブジェクトの原点は中心となっており、配置や回転処理などではこの原点が基準となります。
今回は**Shape**を例にオブジェクトの原点を変更する方法について説明します。

![change-shape-origin.png](/images/change-shape-origin.gif)

<center><a href="http://runstant.com/alkn203/projects/7058dacc" target="_blank">[runstantで確認]</a></center>

上の例では、同じ矩形でも回転の中心が違うのが分かるかと思います。

### originプロパティと原点の位置
オブジェクトの原点は**origin**で指定でき、位置関係は以下のようになっています。

* **origin.set(0.5, 0.5)**・・・中心（デフォルト）
* **origin.set(0, 0)**・・・左上
* **origin.set(0, 1)**・・左下
* **origin.set(1, 0)**・・右上
* **origin.set(1, 1)**・・右下

例えば**shape**の原点を左上にしたい場合、以下のようにします。

```js
shape.origin.set(0, 0)
```

### サンプルコード

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
    var shape = Shape().addChildTo(this);
    // 移動
    shape.x = 320;
    shape.y = 480;
    // 回転
    shape.update = function() {
      shape.rotation += 2;  
    };

    var shape2 = Shape().addChildTo(this);
    // 移動
    shape2.x = 320;
    shape2.y = 240;
    // 左上を原点に
    shape2.origin.set(0, 0);
    // 回転
    shape2.update = function() {
      shape2.rotation += -2;  
    };
    
    var shape3 = Shape().addChildTo(this);
    // 移動
    shape3.x = 320;
    shape3.y = 720;
    // 右下を原点に
    shape3.origin.set(1, 1);
    // 回転
    shape3.update = function() {
      shape3.rotation += -2;  
    };
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