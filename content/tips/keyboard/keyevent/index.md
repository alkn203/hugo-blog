+++
date = "2021-06-24T17:59:06+09:00"
draft = false
slug = ""
tags = ["keyboard"]
title = "キーイベントの種類を理解する"
eyecatch = "keyevent.gif"
+++

![keyevent](keyevent.gif)

**phina.js**にはあらかじめ用意された**keyイベント**があります。

## getKey
特定のキーが押されているかを調べます。

## getKeyDown
特定のキーが押されたかを調べます。

## getKeyUp
特定のキーが離されたかを調べます。

## 使用例
これらのメソッドは、通常**update**の中で**app.keybord**を参照して使用します。

```js
// 毎フレーム処理
this.update = function(app) {
  var key = app.keyboard;
  // 移動
  if (key.getKey('left')) {
    shape.x -= SPEED;
  }
  // キーダウン

  if (key.getKeyDown('right')) {
    label1.text = 'getKeyDown: right';
  }
  // キーアップ
  }
  if (key.getKeyUp('up')) {
    label2.text = 'getKeyUp: up';
  }
};
```

## on+キーイベント名
**on+キーイベント名**でイベントドリブンな検出を行うことができます。

以下の例では、**onkeyup** イベント発火時に入力されたキーコードとキーコードに対応した文字をラベルに表示しています。

```js
// キーアップイベント
this.onkeyup = function(e) {
  label3.text = 'onkeyup: ' + e.keyCode + ':' + String.fromCharCode(e.keyCode);
};
```

## getKeyAngle
キーの方向を **angle(degree)** で取得します。

```js
// キーの方向をangleで取得
var angle = key.getKeyAngle();
label4.text = 'angle:' + angle; // left >> 180
```

## getKeyDirection
キーの方向を正規化した値(連想配列)で取得します。

```js
// キーの方向を正規化した値eで取得
var direction = key.getKeyDirection();
label5.text = 'direction:' + direction; // left >> {x:-1, y:0}
```

## サンプルコード
<details>
<summary>コードを見る</summary>

```js
// グローバルに展開
phina.globalize();

var LABEL_COLOR = 'yellow';
var SPEED = 2;
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

    var label1 = Label({
      text: '',
      fill: LABEL_COLOR,
    }).addChildTo(this).setPosition(320, 100);

    var label2 = Label({
      text: '',
      fill: LABEL_COLOR,
    }).addChildTo(this).setPosition(320, 220);

    var label3 = Label({
      text: '',
      fill: LABEL_COLOR,
    }).addChildTo(this).setPosition(320, 340);
    
    var label4 = Label({
      text: '',
      fill: LABEL_COLOR,
    }).addChildTo(this).setPosition(320, 600);

    var label5 = Label({
      text: '',
      fill: LABEL_COLOR,
    }).addChildTo(this).setPosition(320, 720);

    var shape = CircleShape().addChildTo(this).setPosition(320, 480);
    // 毎フレーム処理
    this.update = function(app) {
      var key = app.keyboard;
      // 上下左右移動
      if (key.getKey('left')) {
        shape.x -= SPEED;
      }
      if (key.getKey('right')) {
        shape.x += SPEED;
      }
      if (key.getKey('up')) {
        shape.y -= SPEED;
      }
      if (key.getKey('down')) {
        shape.y += SPEED;
      }
      // キーダウン
      if (key.getKeyDown('left')) {
        label1.text = 'getKeyDown: left';
      }
      if (key.getKeyDown('right')) {
        label1.text = 'getKeyDown: right';
      }
      if (key.getKeyDown('up')) {
        label1.text = 'getKeyDown: up';
      }
      if (key.getKeyDown('down')) {
        label1.text = 'getKeyDown: down';
      }
      // キーアップ
      if (key.getKeyUp('left')) {
        label2.text = 'getKeyUp: left';
      }
      if (key.getKeyUp('right')) {
        label2.text = 'getKeyUp: right';
      }
      if (key.getKeyUp('up')) {
        label2.text = 'getKeyUp: up';
      }
      if (key.getKeyUp('down')) {
        label2.text = 'getKeyUp: down';
      }
      // キーの方向をangleで取得
      var angle = key.getKeyAngle();
      label4.text = 'angle:' + angle;
      // キーの方向を正規化した値eで取得
      var direction = key.getKeyDirection();
      label5.text = 'direction:' + direction;
    };
    // キーアップイベント
    this.onkeyup = function(e) {
      label3.text = 'onkeyup: ' + e.keyCode + ':' + String.fromCharCode(e.keyCode);
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