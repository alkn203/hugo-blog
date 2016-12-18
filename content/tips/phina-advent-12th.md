+++
date = "2016-12-12T00:01:08+09:00"
draft = false
slug = ""
tags = ["tips", "flow"]
title = "Flowを使った非同期処理の基本"
eyecatch = "/images/phina-advent-12th.png"
+++

## はじめに
この記事は、[phina.js Advent Calendar 2016](http://qiita.com/advent-calendar/2016/phinajs) の12日目の記事です。

* 11日目：[phina.jsでの多人数参加型ゲームの作製 by tamochu3141さん](http://qiita.com/tamochu3141/items/8094aec3348dc9a6ae9d)
* 13日目：[phina.jsでGamePadを使う by minimoさん](http://qiita.com/minimo/items/27020fdb66375f1f868b)

## 非同期処理
基本的にプログラムはソースに書かれたの順序で実行されます。これは一般的に**同期処理**と呼んでいます。
一方で、処理の一部を特定の処理が終わるまで待ってから実行するなど、書かれた順序に依存しない処理を**非同期処理**と呼んでおり、時間のかかる処理をバックグランドで実行させておく場合になどによく使用されます。

## Flowとは
**phina.js**には、**Flow**という非同期処理を実装するためのクラスが用意されています。**javascript**に詳しい人なら、**EcmaScript6**の**Promise**と言えばピンと来るかもしれません。(そう言う私もそれほど詳しくありませんが）
**Flow**はその**Promise**と基本的に同じ機能を**phina.js**で実装したものです。**tmlib.js**時代も**Flow**というクラス自体はありましたが、中身は大幅に違っています。

<center>![15puzzle-tut-2](/images/phina-advent-12th.png)</center>

## Flowの使い方
では、早速使い方を見ていきましょう。
以下がサンプルコードです。

```js
phina.globalize();
// メインシーン
phina.define('MainScene', {
  superClass: 'DisplayScene',
  // コンストラクタ
  init: function() {
    // 親クラス初期化
    this.superInit();
    // 背景色
    this.backgroundColor = 'black';
  
    var self = this;
    var duration = 2000;
    // flow設定
    var flow = Flow(function(resolve) {
      RectangleShape()
        .addChildTo(self)
        .setPosition(self.gridX.center(), 0)
        .tweener.to({y: self.gridY.center()}, duration)
        .call(function() {
          resolve();
        });
    });

    var rect = RectangleShape().addChildTo(this);
    rect.setPosition(self.gridX.center(1), self.gridY.center(1));
    // 非同期処理(resoveで呼ばれる)
    flow.then(function() {
      rect.tweener.to({y: self.gridY.width}, duration).play();
    });
    // 同期処理
    RectangleShape()
      .addChildTo(self)
      .setPosition(self.gridX.center(-1), self.gridY.center(1))
      .tweener.to({y: self.gridY.width}, duration);
  },
});
// メイン
phina.main(function() {
  var app = GameApp({
    startLabel: 'main',
  });
  app.run();
});
```

<a href="http://runstant.com/alkn203/projects/017e556a" target="_blank">[runstantで確認]</a>

実行すると、左下の矩形は実行と同時に下に移動しますが、右下の矩形は上の矩形が止まってから移動を開始します。

## コード説明
### Flowの設定

```js
// flow設定
var flow = Flow(function(resolve) {
  RectangleShape()
    .addChildTo(self)
    .setPosition(self.gridX.center(), 0)
    .tweener.to({y: self.gridY.center()}, duration)
    .call(function() {
      resolve('flow done');
      console.log(message);
    }).play();
});
```

* 非同期で実行したい処理を**Flow**内に記述します。
* 処理が終わった後は、**resolve**を呼び出します。
* 上の例では、**tweener**を使って一定時間かけて画面中央まで移動した後に、**resolve**をコールするようにしています。

### 後処理の記述

```js
// resoveで実行される
flow.then(function() {
  rect.tweener.to({y: self.gridY.width}, duration).play();
});
```
* **resolve**が呼ばれると、**Flow**クラスのメソッド**then**が実行されるため、この中に後から実行したい処理を記述します。


### 複数のFlowを取り扱う

これまでは1つの**Flow**オブジェクトを取り扱ってきました。
1つであれば**tweener**を使えば似たような処理が出来なくもないですが、さらに**Flow**では複数の非同期処理をウォッチすることができます。
サンプルコードは以下のとおりです。

```js
phina.globalize();
// メインシーン
phina.define('MainScene', {
  superClass: 'DisplayScene',
  // コンストラクタ
  init: function() {
    // 親クラス初期化
    this.superInit();
    // 背景色
    this.backgroundColor = 'black';
  
    var self = this;
    var flows = [];
    
    (10).times(function(i) {
      // flow作成
      var flow = Flow(function(resolve) {
        var x = Random.randint(64, self.gridX.width - 64);
        var y = Random.randint(64, self.gridY.width - 64);
        var duration = Random.randint(500, 5000); 
        // ランダムな位置に円を作成
        CircleShape()
          .addChildTo(self)
          .setPosition(x, y)
          .tweener.fadeOut(duration) // フェードアウト
          .call(function() {
            resolve(i + 'done');
          }).play();
        });
        // 配列に追加
        flows.push(flow);
    });
    // 全てのflow実行後
    Flow.all(flows).then(function(messages) {
      console.log('all done!', messages);
    });
  },
});
// メイン
phina.main(function() {
  var app = GameApp({
    startLabel: 'main',
  });
  app.run();
});
```

<a href="http://runstant.com/alkn203/projects/9665202b" target="_blank">[runstantで確認]</a>

* 実行すると、ランダムな位置に円が表示されランダムな時間でフェードアウトします。
* 最後の円がフェードアウトしてから、メッセージがコンソールに表示されます。

## コード説明
### Flowの配列を作って追加する

```js
var flows = [];

(10).times(function(i) {
  // flow作成
  var flow = Flow(function(resolve) {
    var x = Random.randint(64, self.gridX.width - 64);
    var y = Random.randint(64, self.gridY.width - 64);
    var duration = Random.randint(500, 5000); 
    // ランダムな位置に円を作成
    CircleShape()
      .addChildTo(self)
      .setPosition(x, y)
      .tweener.fadeOut(duration) // フェードアウト
      .call(function() {
        resolve(i + 'done');
      }).play();
    });
    // 配列に追加
    flows.push(flow);
```

* **Flow**を格納する配列を作って**push**しています。

### Flow.allで全てのFlowをウォッチ

```js
// 全てのflow実行後
Flow.all(flows).then(function(messages) {
  console.log('all done!', messages);
});
```

* **Flow.all**の引数にウォッチしたい**Flow**の配列を代入すると、与えれられた全ての非同期処理の終了をウォッチすることができます。
* 今回は配列でまとめて代入しましたが、個々に**Flow**を作成して代入することもできます。

## おわりに
説明不足のところもあるかもしれませんが、今回紹介した**Flow**を使うことによって、**phina.js**における非同期処理をよりスマートにコーディングすることができると思いますので、皆も色々と試してみて下さい。