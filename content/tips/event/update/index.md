+++
date = "2021-06-24T10:59:06+09:00"
draft = false
slug = ""
tags = ["update"]
title = "更新イベントを設定する"
eyecatch = "event-update.gif"
+++

![event-update](event-update.gif)

## 更新イベント処理（update）
* 更新処理を行うためには、オブジェクトに**update**関数を実装します。
* **update**関数が実装されていれば、その内容が毎フレーム実行されます。

```js
// Shapeを作成してシーンに追加
var shape = Shape().addChildTo(this).setPosition(320, 480);
// 更新処理
shape.update = function() {
  shape.moveBy(2, 2);
};
```

## 更新イベント処理（on + enterframe）
**update** では1つの処理しか登録できません。一方、**on** と**enterframe**を組み合わせると複数のメソッドを登録可能で、その内容が並行して実行されます。

```js
// 毎フレーム更新処理
shape2.on('enterframe', function() {
  // 移動
  shape2.moveBy(-2, -2);  
});
// 毎フレーム更新処理
shape2.on('enterframe', function() {
  // 回転
  shape2.rotation += 2;  
});
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
    this.backgroundColor = 'black';
    // Shapeを作成してシーンに追加
    var shape = Shape().addChildTo(this).setPosition(320, 480);
    // 更新処理
    shape.update = function() {
      shape.moveBy(2, 2);
    };
    
    var shape2 = Shape().addChildTo(this).setPosition(320, 480);
    // 毎フレーム更新処理
    shape2.on('enterframe', function() {
      // 移動
      shape2.moveBy(-2, -2);  
    });
    // 毎フレーム更新処理
    shape2.on('enterframe', function() {
      // 回転
      shape2.rotation += 2;  
    });
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
https://runstant.com/alkn203/projects/4a760b10