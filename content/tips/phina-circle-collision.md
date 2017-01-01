+++
date = "2016-12-31T23:59:06+09:00"
draft = false
slug = ""
tags = ["tips","circle","collision","physical"]
title = "円と円の衝突運動プログラム"
eyecatch = "/images/phina-circle-collision.png"
+++

## 今回のサンプル

**phina.js**の作者[phi](https://twitter.com/phi_jp)さんの過去のブログ記事[[New] JavaScript ライブラリ tmlib.js で円同士の衝突プログラムを作ってみた](http://tmlife.net/programming/javascript-tmlib-js-circle-collision.html)を参考にして、円同士の衝突運動プログラムを作ってみました。
こういった動きは物理エンジンに頼っても良いと思いますが、一度自分で書いてみることで理解が深まります。

<center>![phina-circle-collision](/images/phina-circle-collision.png)</center>

## ソースコード

```js
// グローバルに展開
phina.globalize();
// 定数
var REBOUND  = 0.8;    // 反発係数
var FRICTION = 0.99;   // 摩擦係数
var GRAVITY  = 0.8;    // 重力
var BALL_MAX_NUM = 15; // ボールの数
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
    // 画面の矩形
    var scrRect = Rect(0, 0, 640, 960);
    // ボールグループ
    var ballGroup = DisplayElement().addChildTo(this);
    var self = this;
    // ランダムな場所にボールを作成
    (BALL_MAX_NUM).times(function() {
      var rx = Random.randint(scrRect.left, scrRect.right);
      var ry = Random.randint(scrRect.top, scrRect.bottom);
      var ball = Ball().addChildTo(ballGroup).setPosition(rx, ry);
    });
    // 参照用
    this.ballGroup = ballGroup;
    this.scrRect = scrRect;
  },
  // 毎フレーム更新
  update: function() {
    // ボール同士
    this.checkHitBalls();
    // ボールと壁
    this.checkHitWalls();
  },
  // ボール同士の当たり判定処理
  checkHitBalls: function() {
    var balls = this.ballGroup.children;

    balls.each(function(ball, i) {
      balls.each(function(target, j) {
        // 自分以外の相手
        if (i !== j) {
          // 衝突してたら
          if (Collision.testCircleCircle(ball, target)) {
            // 自分から相手へのベクトル
            var abVec = Vector2.sub(target.position, ball.position);
            var len = abVec.length();
            if (len === 0) return;
            // 正規化
            abVec.normalize();
            // 自分と相手の距離
            var distance = (ball.radius + target.radius) - len;
            // めり込んだ量の半分
            var sinkVec = Vector2.mul(abVec, (distance / 2));
            // それぞれで押し出す
            ball.position.sub(sinkVec);
            target.position.add(sinkVec);
            // 反発ベクトルを求める
            var V = Vector2;
            var m0 = ball.m;
            var m1 = target.m;
            var e = REBOUND;
            var vs = ball.physical.velocity;
            var vo = target.physical.velocity;

            var ma = ((m1 / (m0 + m1)) * (1 + e)) * V.dot(V.sub(vo, vs), abVec);
            var mb = ((m0 / (m0 + m1)) * (1 + e)) * V.dot(V.sub(vs, vo), abVec);
       
            vs.add(V.mul(abVec, ma));
            vo.add(V.mul(abVec, mb));
          }
        }
      });    
    });
  },
  // タッチ終了時
  onpointend: function() {
    // ボールを散らす
    this.ballGroup.children.each(function(ball) {
      ball.physical.velocity.random(0, 360, 32);
    });
  },
  // ボールと壁との辺り判定
  checkHitWalls: function() {
    var balls = this.ballGroup.children;
    var rect = this.scrRect;
    
    balls.each(function(ball) {
      if (ball.top < rect.top) {
        ball.top = rect.top;
        ball.physical.velocity.y *= -1;
      }
      if (ball.bottom > rect.bottom) {
        ball.bottom = rect.bottom;
        ball.physical.velocity.y *= -1;
      }
      if (ball.left < rect.left) {
        ball.left = rect.left;
        ball.physical.velocity.x *= -1;
      }
      if (ball.right > rect.right) {
        ball.right = rect.right;
        ball.physical.velocity.x *= -1;
      }
    });
  },
});
/*
 * ボールクラス
 */
phina.define('Ball', {
  // CircleShapeを継承
  superClass: 'CircleShape',
  // コンストラクタ
  init: function() {
    // 親クラス初期化
    this.superInit({
      fill: null,
    });
    // 重力
    this.physical.gravity.set(0, GRAVITY);
    // 摩擦
    this.physical.friction = FRICTION;
    // 半径
    this.radius = Random.randint(25, 50);
    // 面積 = 質量にする
    this.m = Math.PI * this.radius * this.radius;
    // 初期移動量
    this.physical.velocity.random(0, 360, 32);
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
  // 実行
  app.run();
});
```

<a href="http://runstant.com/alkn203/projects/75048ec9" target="_blank">[runstantで確認]</a>
