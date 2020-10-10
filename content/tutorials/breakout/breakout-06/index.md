+++
date = "2020-10-08T20:55:06+09:00"
draft = false
slug = ""
tags = ["breakout"]
title = "【phina.js】ゲーム作成チュートリアル（ブロック崩し）=最終回 ゲームオーバーとクリア="
eyecatch = "breakout05.png"
+++

## はじめに
前回は、**Tweener**を使ったアニメーション処理を追加しました。今回は最終回として、ゲームオーバーとクリア処理を追加したいと思います。

![breakout05.png](breakout05.png)

[runstantで確認](http://runstant.com/alkn203/projects/211ed4f6)

## ゲームオーバー処理

ボールが画面下に落下したら、**GAME OVER**という文字を表示してタイトル画面に戻るようにします。
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
    this.dummyGroup = DisplayElement().addChildTo(this);
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
      var self = this;

      if (ball.top > screenRect.bottom) {
        // ゲームオーバー表示
        var label = Label({
          text: 'GAME OVER',
          fill: 'yellow',
          fontSize: 64,
        }).addChildTo(this);
        label.setPosition(this.gridX.center(), this.gridY.center());
        // 少し待ってからタイトル画面へ
        label.tweener.clear()
                     .wait(2000)
                     .call(function() {
                       self.nextLabel = 'title';
                       self.exit();
                     });
      }
      // パドルとの反射
      if (ball.hitTestElement(paddle) && ball.vy > 0) {
        ball.bottom = paddle.top;
        ball.vy = -ball.vy;
        // 当たった位置で角度を変化させる
        var dx = paddle.x - ball.x;
        ball.vx = -dx / 5;
      }
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

## ゲームオーバー判定

```js
      // 落下
      var self = this;

      if (ball.top > screenRect.bottom) {
        // ゲームオーバー表示
        var label = Label({
          text: 'GAME OVER',
          fill: 'yellow',
          fontSize: 64,
        }).addChildTo(this);
        label.setPosition(this.gridX.center(), this.gridY.center());
        // 少し待ってからタイトル画面へ
        label.tweener.clear()
                     .wait(2000)
                     .call(function() {
                       self.nextLabel = 'title';
                       self.exit();
                     });
      }
```

* **Label**クラスを使って表示用のラベルを追加しています。
* **tweener**を使って、**wait**で2秒ほど待ってからタイトル画面に移動するようにしています。
* **nextLabel**で移動先のシーン名を指定します。
* **exit**で現在のシーンを抜けます。

## クリア処理

全てのブロックを崩したらクリアとして、スコアを結果画面に表示してみます。
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
    this.dummyGroup = DisplayElement().addChildTo(this);
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
    // スコア
    this.score = 0;
    var scoreLabel = Label({
      text: this.score + '',
      fill: 'lime',
      fontSize: 64,
    }).addChildTo(this);
    scoreLabel.setPosition(this.gridX.center(), this.gridY.center());
    scoreLabel.alpha = 0.6;
    // 連続ヒット数
    this.hitNumber = 0;
    // ボール作成
    this.ball = Ball().addChildTo(this);
    // シーン全体から参照可能にする
    this.paddle = paddle;
    this.screenRect = screenRect;
    this.scoreLabel = scoreLabel;
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
      var self = this;

      if (ball.top > screenRect.bottom) {
        // ゲームオーバー表示
        var label = Label({
          text: 'GAME OVER',
          fill: 'yellow',
          fontSize: 64,
        }).addChildTo(this);
        label.setPosition(this.gridX.center(), this.gridY.center());
        // 少し待ってからタイトル画面へ
        label.tweener.clear()
                     .wait(2000)
                     .call(function() {
                       self.nextLabel = 'title';
                       self.exit();
                     });
      }
      // パドルとの反射
      if (ball.hitTestElement(paddle) && ball.vy > 0) {
        ball.bottom = paddle.top;
        ball.vy = -ball.vy;
        // 当たった位置で角度を変化させる
        var dx = paddle.x - ball.x;
        ball.vx = -dx / 5;
      }
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
    // クリアチェック
    if (this.blockGroup.children.length === 0) {
      // スコアをリザルトシーンに渡す
      this.exit({
        score: this.score,  
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
    // スコア加算
    this.addScore();
  },
  // スコア加算処理
  addScore: function() {
    this.hitNumber++;
    this.score += this.hitNumber * 10;
    this.scoreLabel.text = this.score;
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

[runstantで確認](http://runstant.com/alkn203/projects/5873b593)

## スコア表示

```js
// スコア
this.score = 0;
var scoreLabel = Label({
  text: this.score + '',
  fill: 'lime',
  fontSize: 64,
}).addChildTo(this);
scoreLabel.setPosition(this.gridX.center(), this.gridY.center());
scoreLabel.alpha = 0.6;
// 連続ヒット数
this.hitNumber = 0;
```

* **this.score**は、スコア用の変数です。
* **Label**クラスを使ってスコア表示用のラベルを追加しています。ラベルの**text**プロパティにスコア値を設定しています。
* **alpha**は透明度で、0に近づくほど透明になります。
* **this.hitNumber**ブロックを連続で崩した回数を記録します。

## スコア加算

```js
  // スコア加算
  this.addScore();
},
// スコア加算処理
addScore: function() {
  this.hitNumber++;
  this.score += this.hitNumber * 10;
  this.scoreLabel.text = this.score;
},
});
```
* ブロックを崩した時に**addScore**関数を呼び出して、スコアを加算します。
* **addScore**関数内では、連続ヒット数の場合、スコアを上乗せしています。
* スコア表示用ラベルの**text**プロパティに現在のスコアを反映させれば、ラベルが更新されます。

## クリア判定

```js
// クリアチェック
if (this.blockGroup.children.length === 0) {
  // スコアをリザルトシーンに渡す
  this.exit({
    score: this.score,  
  });
}
```
* ブロックを全て崩した時がクリアの条件ですので、**length**の値をチェックします。
* **exit**にスコアを渡して、リザルトシーンに移動します。**nextLabel**を設定していないのは、**main**の次はデフォルトで**result**が設定されているためです。
* 実際にクリアすると、リザルト画面にスコアが引き渡されているのが分かるかと思います。

## まとめ

今回のチュートリアルでは、**phina.js**を使った簡単なブロック崩しの作成について触れさせて頂きました。
**phina.js**には、今回では紹介しきれていない機能がまだまだありますので、これからこのブログなどで私なりに紹介できればと思っております。

今回の一連のエントリーに関する質問などは、私([@alkn203](https://twitter.com/alkn203))までお気軽にどうぞ。
**phina.js**に関する質問は、作者のphi氏([@phi-jp](https://twitter.com/phi_jp))に直接して頂くか、[slack](https://phina.slack.com/)でして頂ければ、メンバーが真剣に答えてくれると思います。(微力ながら私も参加しています）

今回のチュートリアルが、**phina.js**を使ったゲーム作成の際に少しでも参考になれば幸いです。
最後までお付き合い頂きありがとうございました。