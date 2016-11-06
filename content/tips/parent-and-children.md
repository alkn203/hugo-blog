+++
date = "2016-03-05T23:59:06+09:00"
draft = false
slug = ""
tags = ["tips"]
title = "グローバル座標とローカル座標を使ったサンプル"

+++

### はじめに
**phina.js**では、オブジェクトにオブジェクトを追加すると親子関係が成立します。子オブジェクトは親オブジェクトに追従するようになり、まとめて移動する必要がある時などに便利です。
この際に注意するべきことは、**子オブジェクトの座標は親オブジェクトの中心からの相対座標**になるという点です。
従って、子オブジェクトとの当たり判定などを行う時は、親のグローバル座標と子のローカル座標について把握しておく必要があります。

### サンプル
子オブジェクトとの当たり判定のサンプルです。
当たり判定処理では、子オブジェクトの座標値はそのまま使えませんので

```js
親のx座標 + 子のx座標（相対値）
親のy座標 + 子のy座標（相対値）
```
で補正した値を元に位置情報だけの**Rect**（矩形）を作り、**Collision**クラスを使ってその**Rect**と当たり判定を行っています。

<div class='runstant'><iframe src='http://goo.gl/IIEsns' width='100%' height='640px' style='border:0px;box-shadow:0px 0px 2px 0px #aaa'></iframe></div>

[[runstantで開く](http://goo.gl/IIEsns)]

### コード

```js
phina.globalize();
// 定数
var SCREEN_RECT = Rect(0, 0, 640, 960); // 画面サイズのRect
// メインシーン
phina.define('MainScene', {
  superClass: 'DisplayScene',
  // コンストラクタ
  init: function() {
    // 親クラス初期化
    this.superInit();
    // 背景
    this.backgroundColor = 'black';

    Label({
      text: 'Touch To Shot',
      fontSize: 48,
      fill: 'lime',
    }).addChildTo(this).setPosition(this.gridX.center(), this.gridY.span(2));
    // ショットグループ
    var shotGroup = DisplayElement().addChildTo(this);
    // 敵子供グループ
    var pitGroup = DisplayElement().addChildTo(this);
    // 自機
    var player = TriangleShape().addChildTo(this);
    player.setPosition(this.gridX.center(), this.gridY.span(15));
    // 敵
    var enemy = RectangleShape().addChildTo(this);
    enemy.setPosition(this.gridX.center(), this.gridY.center(-2));

    enemy.tweener.clear()
                 .by({x: -100}, 1000)
                 .by({x: 200}, 2000)
                 .by({x: -100}, 1000)
                 .setLoop(true);
    // 敵子供追加
    [0, 45, 90, 135, 180, 225, 270, 315].each(function(deg) {
      Pit(deg, 4).addChildTo(enemy);
    });
    // 参照用
    this.player = player;
    this.enemy = enemy;
    this.shotGroup = shotGroup;
    this.pitGroup = pitGroup;
  },
  // 画面タッチ時処理
  onpointstart: function() {
    // ショットが無ければ
    if (this.shotGroup.children.length === 0) {
      // 上方向にショット発射
      CircleShape({
        radius: 16,
      }).addChildTo(this.shotGroup)
        .setPosition(this.player.x, this.player.top)
        .physical.force(0, -16);
    }
  },
  // 毎フレーム更新
  update: function() {
    var enemy = this.enemy;
    // 画面からはみ出たショットは削除
    this.shotGroup.children.each(function(shot) {
      if (shot.bottom < SCREEN_RECT.top) shot.remove();      
    });
    // 敵に当たったらショットは削除
    this.shotGroup.children.each(function(shot) {
      if (shot.hitTestElement(enemy)) shot.remove();
    });
    // ショットと敵の子供のあたり判定
    this.shotGroup.children.each(function(shot) {
      enemy.children.each(function(pit) {
        // 相対座標を絶対座標に変換した当たり判定用の矩形を作る
        var rect = Rect(enemy.x + pit.x, enemy.y + pit.y, pit.width, pit.height);
        // Collisionクラスを利用して当たり判定
        if (Collision.testRectRect(shot, rect)) {
          shot.remove();
          pit.remove();
        }
      });
    });
  },
});
// 敵子供クラス
phina.define('Pit', {
  superClass: 'PolygonShape',
  // コンストラクタ
  init: function(deg, speed) {
    // 親クラス初期化
    this.superInit({
      radius: 16,
      sides: 6,
    });

    this.r = 32 * 3;
    this.speed = speed;
    this.deg = deg;
  },
  // 毎フレーム更新
  update: function() {
    this.rotation += -this.speed * 2;
    this.deg += this.speed;    
    var deg = this.deg;
    var r = this.r;
    // 円周上の位置（相対座標）
    this.x = r * Math.cos(Math.degToRad(deg));
    this.y = r * Math.sin(Math.degToRad(deg));
  },
});
// メイン
phina.main(function() {
  var app = GameApp({
    startLabel: 'main',
    title: 'Hit test children',
  });
  app.run();
});
```
