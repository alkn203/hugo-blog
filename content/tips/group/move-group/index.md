+++
date = "2021-06-24T19:59:06+09:00"
draft = false
slug = ""
tags = ["group"]
title = "オブジェクトをグループ化して動かす"
eyecatch = "move-group.gif"
+++

![move-group](move-group.gif)

## オブジェクトをグループ化する
**phina.js**でオブジェクトのグループ化を行うときは、 以下のように**DisplayElement**の子要素として追加する方法が一般的です。

```js
// グループ
var group = DisplayElement().addChildTo(this);
// グループの中心座標
group.setPosition(320, 480);
// RectangleShape
RectangleShape({
  width: 128,
  height: 128,
  fill: 'red',
  stroke: 'white',
  strokeWidth: 16,
  cornerRadius: 16
}).addChildTo(group).setPosition(0, -140); // 相対座標
```

* 絶対座標をグループに対して設定します。
* 追加したオブジェクトの座標は**相対座標**とする必要があります。

## グループを動かす
グループの座標を変更すると、子要素がまとまって移動します。

```js
this.onpointmove = function(e) {
  group.x = e.pointer.x;
  group.y = e.pointer.y;
};
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
    // グループ
    var group = DisplayElement().addChildTo(this);
    // グループの中心座標
    group.setPosition(320, 480);
    // RectangleShape
    RectangleShape({
      width: 128,
      height: 128,
      fill: 'red',
      stroke: 'white',
      strokeWidth: 16,
      cornerRadius: 16
    }).addChildTo(group).setPosition(0, -140);
    // TriangleShape
    TriangleShape({
      fill: 'purple',
      stroke: 'white',
      strokeWidth: 16,
      radius: 64
    }).addChildTo(group).setPosition(140, 0);
    // PolygonShape
    PolygonShape({
      stroke: 'white',
      strokeWidth: 16,
      radius: 64,
      sides: 8,
    }).addChildTo(group).setPosition(0, 140);
    // HeartShape
    HeartShape({
      stroke: 'white',
      strokeWidth: 16,
      radius: 64,
    }).addChildTo(group).setPosition(-140, 0);
    //
    this.onpointmove = function(e) {
      group.x = e.pointer.x;
      group.y = e.pointer.y;
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

</details>

## runstantプロジェクト
https://runstant.com/alkn203/projects/bcb3d544