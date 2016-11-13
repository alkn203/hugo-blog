+++
date = "2015-11-05T20:55:06+09:00"
draft = false
slug = ""
tags = ["tutorial", "breakout"]
title = "【phina.js】ゲーム作成チュートリアル（ブロック崩し）第6回＝Tweenerアニメーション＝"
eyecatch = "/images/breakout-tut-05.png"
+++

## はじめに
前回は、ブロックの消去処理を追加しました。
今回は、ゲームとしての見栄えを良くするために、ブロックがアニメーションして消えるようにしたいと思います。

<center>![breakout-tut-05](/images/breakout-tut-05.png)</center>

## Tweenerを使ったアニメーション処理

**phina.js**には**Tweener**という機能があり、オブジェクトに対して移動、拡大・縮小、回転などといったアニメーションを簡単に設定することができます。
今回は、ブロックが**縮小して消える**ようにしてみます。

## アニメーション用ダミーブロックの作成

ブロックそのものに**Tweener**を設定しても良いのですが、消去アニメーション中も当たり判定が有効だと予期しない動作を招きかねないので、アニメーション用のダミーブロックを別途作成してから、それに**Tweener**を設定する方法をとります。
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
var BALL_RADIUS = BLOCK_WIDTH / 8;
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
    // ダミーブロックグループ
    this.dummyGroup = CanvasElement().addChildTo(this);
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
    // 画面上でタッチが離れた時
    this.onpointend = function() {
      if (self.status === 'ready') {
        // ボール発射
        self.ball.vy = -self.ball.speed;
        self.status = 'move';
      }
    };
    // ボール作成
    this.ball = Ball().addChildTo(this);
    // シーン全体から参照可能にする
    this.paddle = paddle;
    this.screenRect = screenRect;
    // ゲーム状態
    this.status = 'ready';
  },
    // 毎フレーム更新
  update: function() {
    var ball = this.ball;
    var paddle = this.paddle;
    var screenRect = this.screenRect;
    // ボール待機中
    if (this.status === 'ready') {
      // ボールはパドルの真上
      ball.vx = ball.vy = 0;
      ball.x = paddle.x;
      ball.bottom = paddle.top;
    }
    // ボール移動中
    if (this.status === 'move') {
      // ボール移動
      ball.moveBy(ball.vx, ball.vy);
      // 画面端反射
      // 上
      if (ball.top < screenRect.top) {
        ball.top = screenRect.top;
        ball.vy = -ball.vy;
      }
      // 左
      if (ball.left < screenRect.left) {
        ball.left = screenRect.left;
        ball.vx = -ball.vx;
      }
      // 右
      if (ball.right > screenRect.right) {
        ball.right = screenRect.right;
        ball.vx = -ball.vx;
      }
      // 落下
      if (ball.top > screenRect.bottom) {
        // 準備状態へ
        this.status = 'ready';
      }
      // パドルとの反射
      if (ball.hitTestElement(paddle) && ball.vy > 0) {
        ball.bottom = paddle.top;
        ball.vy = -ball.vy;
        // 当たった位置で角度を変化させる
        var dx = paddle.x - ball.x;
        ball.vx = -dx / 5;
      }
      var self = this;
      // ブロックとの反射
      this.blockGroup.children.some(function(block) {
        // ヒットなら
        if (ball.hitTestElement(block)) {
          // 左上かど
          if (ball.top < block.top && ball.left < block.left) {
            // 位置補正
            ball.right = block.left;
            ball.bottom = block.top;
            // 移動方向設定
            ball.vx = -ball.speed;
            ball.vy = -ball.speed;
            // ブロック消去
            self.disableBlock(block);
            return true;
          }
          // 右上かど
          if (block.top < ball.top && block.right < ball.right) {
            ball.left = block.right;
            ball.bottom = block.top;
            ball.vx = ball.speed;
            ball.vy = -ball.speed;
            self.disableBlock(block);
            return true;
          }
          // 左下かど
          if (block.bottom < ball.bottom && ball.left < block.left) {
            ball.right = block.left;
            ball.top = block.bottom;
            ball.vx = -ball.speed;
            ball.vy = ball.speed;
            self.disableBlock(block);
            return true;
          }
          // 右下かど
          if (block.bottom < ball.bottom && block.right < ball.right) {
            ball.left = block.right;
            ball.top = block.bottom;
            ball.vx = ball.speed;
            ball.vy = ball.speed;
            self.disableBlock(block);
            return true;
          }
          // 左側
          if (ball.left < block.left) {
            ball.right = block.left;
            ball.vx = -ball.vx;
            self.disableBlock(block);
            return true;
          }
          // 右側
          if (block.right < ball.right) {
            ball.left = block.right;
            ball.vx = -ball.vx;
            self.disableBlock(block);
            return true;
          }
          // 上側
          if (ball.top < block.top) {
            ball.bottom = block.top;
            ball.vy = -ball.vy;
            self.disableBlock(block);
            return true;
          }
          // 下側
          if (block.bottom < ball.bottom) {
            ball.top = block.bottom;
            ball.vy = -ball.vy;
            self.disableBlock(block);
            return true;
          }
        }
      });
    }
  },
  // ブロックの消去処理
  disableBlock: function(block) {
    // 消去アニメーション用のダミーブロック
    var dummy = Block().addChildTo(this.dummyGroup);
    // 属性コピー
    dummy.x = block.x;
    dummy.y = block.y;
    // ブロック削除
    block.remove();
    // 縮小して消えるアニメーション
    dummy.tweener.clear()
                 .to({scaleX: 0.1, scaleY: 0.1}, 200)
                 .call(function() {
                   dummy.remove();
                 });
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
 * ボールクラス
 */
phina.define('Ball', {
  // 親クラス指定
  superClass: 'CircleShape',
  // コンストラクタ
  init: function() {
    // 親クラス初期化
    this.superInit({
      radius: BALL_RADIUS,
      fill: 'silver',
    });
    // スピード
    this.speed = 6;
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
  // fps変更
  app.fps = 60;
  // 実行
  app.run();
});
```
<a href="http://runstant.com/alkn203/projects/d1392646" target="_blank">[runstantで確認]</a>

## コード説明

### ダミーブロック

```js
    // ダミーブロックグループ
    this.dummyGroup = CanvasElement().addChildTo(this);
```

* **dummyGroup**は、ダミーブロック用のグループです。

### ブロックの消去処理

```js
// ブロックの消去処理
disableBlock: function(block) {
  // 消去アニメーション用のダミーブロック
  var dummy = Block().addChildTo(this.dummyGroup);
  // 属性コピー
  dummy.x = block.x;
  dummy.y = block.y;
  // ブロック削除
  block.remove();
  // 縮小して消えるアニメーション
  dummy.tweener.clear()
               .to({scaleX: 0.1, scaleY: 0.1}, 200)
               .call(function() {
                 dummy.remove();
               });
},
```

* **disableBlock**では、まず引数で与えられたブロックの位置情報をコピーしてダミーブロックを作っています。そして、元のブロックを消去しています。

```js
// 縮小して消えるアニメーション
dummy.tweener.clear()
             .to({scaleX: 0.1, scaleY: 0.1}, 200)
             .call(function() {
               dummy.remove();
             });
```
* ダミーブロックの**Tweener**設定を行っています。
* **clear()**で一旦初期化し、**to**でパラメータの変更を行います。
* 今回は縮小を行うので、**scaleX**と**scaleY**に縮小後の大きさを割合で指定します。**0.1**の場合は、元の0.1倍の大きさになります。
* その後の数字で、そのアニメーションをどれ位の時間をかけて行うかを指定します。単位は**ミリ秒**です。
* 次の**call**で、任意の処理を呼び出すことができます。ここでは、ダミーを消去したいので**remove**としています。
* 一連の処理は、チェインメソッドで繋げることができ、順番に実行されます。
* つまり今回は、**200ミリ秒かけて0.1倍まで縮小させた後に、自分自身を消去する**という一連の処理になります。

### remove関数の置き換え

```js
            // ブロック削除処理
            self.disableBlock(block);
```

* 前回までの**remove**関数を上記のコードに置き換えます。

## 今回はここまで
今回は**phina.js**の目玉機能の１つとも言える**Tweener**を使って、ブロックの消去アニメーションを作ってみました。
次回はいよいよ最終回です。最終回では、もう少しゲームらしくするために、ゲームオーバーとクリア処理を追加してみたいと思います。
