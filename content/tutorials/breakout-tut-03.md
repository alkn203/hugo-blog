+++
date = "2015-10-29T20:55:06+09:00"
draft = false
slug = ""
tags = ["tutorial", "breakout"]
title = "【phina.js】ゲーム作成チュートリアル（ブロック崩し）第3回＝パドルの作成＝"
eyecatch = "/images/breakout-tut-03.png"
+++

## はじめに

前回は、画面上部にブロックを配置しました。
今回は、プレイヤーが操作する**パドル**を作成したいと思います。

<center>![breakout-tut-03](/images/breakout-tut-03.png)</center>

## 定数とPaddleクラスを定義する
パドル用の定数を追加して、**Paddle**クラスを**phina.define**で定義します。コードは以下のとおりです。

```js
// グローバルに展開
phina.globalize();
/*
 * 定数
 */
var BLOCK_WIDTH = 40 * 2;
var BLOCK_HEIGHT = 60 / 2;
var PADDLE_WIDTH = BLOCK_WIDTH * 1.5;
var PADDLE_HEIGHT = BLOCK_HEIGHT;
/*
 * メインシーン
 */
phina.define("MainScene", {
  // 継承
  superClass: 'DisplayScene',
  // コンストラクタ
  init: function() {
    // 親クラス初期化
    this.superInit();
    // 背景色
    this.backgroundColor = 'black';
    // ブロックグループ
    this.blockGroup = DisplayElement().addChildTo(this);

    var self = this;
    // Gridを利用してブロック設置
    Array.range(2, 16, 2).each(function(spanX) {
      Array.range(1, 4, 0.5).each(function(spanY) {
        Block().addChildTo(self.blockGroup)
               .setPosition(self.gridX.span(spanX), self.gridY.span(spanY));
      });
    });
    // パドル移動ライン
    var paddleY = this.gridY.span(14.5);
    // パドル設置
    var paddle = Paddle().addChildTo(this)
                         .setPosition(this.gridX.center(), paddleY);
  },
});
/*
 * ブロッククラス
 */
phina.define('Block', {
  // 親クラス指定
  superClass: 'RectangleShape',
  // コンストラクタ
  init: function() {
    // 親クラス初期化
    this.superInit({
      width: BLOCK_WIDTH,
      height: BLOCK_HEIGHT,
    });
  },
});
/*
 * パドルクラス
 */
phina.define('Paddle', {
  // 親クラス指定
  superClass: 'RectangleShape',
  // コンストラクタ
  init: function() {
    // 親クラス初期化
    this.superInit({
      width: PADDLE_WIDTH,
      height: PADDLE_HEIGHT,
      fill: 'silver',
    });
  },
});
/*
 * メイン処理
 */
phina.main(function() {
  // アプリケーションを生成
  var app = GameApp({
    title: 'Break Out',
  });
  // 実行
  app.run();
});
```
<a href="http://runstant.com/alkn203/projects/305fd254" target="_blank">[runstantで確認]</a>

## コード説明

### 定数
```js
var PADDLE_WIDTH = BLOCK_WIDTH * 1.5;
var PADDLE_HEIGHT = BLOCK_HEIGHT;
```

* パドルの横幅はブロックの横幅の1.5倍、縦幅はブロックの縦幅と同じにします。

### パドルクラス
```js
phina.define('Paddle', {
  // 親クラス指定
  superClass: 'RectangleShape',
  // コンストラクタ
  init: function() {
    // 親クラス初期化
    this.superInit({
      width: PADDLE_WIDTH,
      height: PADDLE_HEIGHT,
      fill: 'silver',
    });
  },
});
```

* **phina.define**で**Paddle**クラスを定義しています。
* **Block**クラスと同様に**RectangleShape**クラスを継承し、**superInit**で親クラスへパラメータを渡しています。
* パラメータの**fill**は塗りつぶしの色です。 **phina.js**では、他に**rgb**や**hsl**形式でも色を指定できます。

### パドルの配置
```js
    // パドル移動ライン
    var paddleY = this.gridY.span(14.5);
    // パドル設置
    var paddle = Paddle().addChildTo(this)
                         .setPosition(this.gridX.center(), paddleY);
```

* **paddleY**は、パドルが移動するラインのy座標です。
* パドルを作成して**MainScene**に追加しています。
* 初期位置は、画面の中央下としています。**this.gridX.center()**で、画面中央のx座標を得ることができます。

### パドルの移動処理

このままではハドルが画面に表示されただけですので、移動処理を追加します。
コードは以下のとおりです。

```js
// グローバルに展開
phina.globalize();
/*
 * 定数
 */
var BLOCK_WIDTH = 40 * 2;
var BLOCK_HEIGHT = 60 / 2;
var PADDLE_WIDTH = BLOCK_WIDTH * 1.5;
var PADDLE_HEIGHT = BLOCK_HEIGHT;
/*
 * メインシーン
 */
phina.define("MainScene", {
  // 継承
  superClass: 'DisplayScene',
  // コンストラクタ
  init: function() {
    // 親クラス初期化
    this.superInit();
    // 背景色
    this.backgroundColor = 'black';
    // ブロックグループ
    this.blockGroup = DisplayElement().addChildTo(this);
    // 位置判定用のRect
    var screenRect = Rect(0, 0, 640, 960);

    var self = this;
    // Gridを利用してブロック設置
    Array.range(2, 16, 2).each(function(spanX) {
      Array.range(1, 4, 0.5).each(function(spanY) {
        Block().addChildTo(self.blockGroup)
               .setPosition(self.gridX.span(spanX), self.gridY.span(spanY));
      });
    });
    // パドル移動ライン
    var paddleY = this.gridY.span(14.5);
    // パドル設置
    var paddle = Paddle().addChildTo(this)
                         .setPosition(this.gridX.center(), paddleY);
    // 画面上でのタッチ移動時
    this.onpointmove = function(e) {
      // タッチ位置に移動
      paddle.setPosition(e.pointer.x | 0, paddleY);
      // 画面はみ出し防止
      if (paddle.left < screenRect.left) { paddle.left = screenRect.left; }
      if (paddle.right > screenRect.right) { paddle.right = screenRect.right; }
    };
  },
});
/*
 * ブロッククラス
 */
phina.define('Block', {
  // 親クラス指定
  superClass: 'RectangleShape',
  // コンストラクタ
  init: function() {
    // 親クラス初期化
    this.superInit({
      width: BLOCK_WIDTH,
      height: BLOCK_HEIGHT,
    });
  },
});
/*
 * パドルクラス
 */
phina.define('Paddle', {
  // 親クラス指定
  superClass: 'RectangleShape',
  // コンストラクタ
  init: function() {
    // 親クラス初期化
    this.superInit({
      width: PADDLE_WIDTH,
      height: PADDLE_HEIGHT,
      fill: 'silver',
    });
  },
});
/*
 * メイン処理
 */
phina.main(function() {
  // アプリケーションを生成
  var app = GameApp({
    title: 'Break Out',
  });
  // 実行
  app.run();
});
```
<a href="http://runstant.com/alkn203/projects/247775fb" target="_blank">[runstantで確認]</a>

## コード説明

### 画面の矩形
```js
var screenRect = Rect(0, 0, 640, 960);
```
* 画面端との位置判定用に、画面と同じサイズの**Rect**オブジェクトを作成します。**Rect**クラスは、実体は無く、矩形の位置やサイズ情報だけを持ったものと理解して下さい。

### 移動処理
```js
    // 画面上でのタッチ移動時
    this.onpointmove = function(e) {
      // タッチ位置に移動
      paddle.setPosition(e.pointer.x | 0, paddleY);
      // 画面はみ出し防止
      if (paddle.left < screenRect.left) { paddle.left = screenRect.left; }
      if (paddle.right > screenRect.right) { paddle.right = screenRect.right; }
    };
```

* パドルの移動処理です。
* 画面上でタッチが移動した時に**onpointmove**イベントが発生しますので、それを拾って関数に処理を書きます。
* 引数の**e**にポイント関係の情報が入ってきますので、**e.pointer.x**でタッチされているx座標だけを取得します。
* **setPosition**で、パドルをタッチされている位置に移動させます。この時、Y座標は固定したままにします。
* 次の**if**文は、パドルが画面からはみ出さないように調整しています。
* **Shape**などは、**left、right、top、bottom**プロパティを持っているので、それを活用して上記のような位置合わせを簡単に行うことができます。

## 今回はここまで
ここまでで、パドルを動かせるようになり、少しずつブロック崩しらしくなってきました。
次回は、ボールを追加したいと思います。
