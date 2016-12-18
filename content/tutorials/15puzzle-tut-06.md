+++
date = "2016-01-05T23:59:06+09:00"
draft = false
slug = ""
tags = ["tutorial", "15puzzle"]
title = "【phina.js】ゲーム作成チュートリアル（15パズル）（最終回）【ゲーム的要素の追加】"
eyecatch = "/images/15puzzle-tut-6.png"
+++

## 前回
前回の[【phina.js】ゲーム作成チュートリアル（15パズル）（その５）【tweenerで移動処理】](http://alkn203.github.io/blog/2016/01/01/15puzzle-tut-05/)では、**tweener**を使ってピースをアニメーション移動させました。

## 今回の目標
今回は、もう少しゲームらしくするために以下の要素を追加したいと思います。

* ピースのシャッフル処理
* クリア判定

## ピースのシャッフル

ピースをシャッフルするボタンを追加します。

<center>![15puzzle-tut-2](/images/15puzzle-tut-6.png)</center>

コードは以下のとおりです。

```js
phina.globalize();
// 定数
var SCREEN_WIDTH = 640;            // 画面横サイズ
var SCREEN_HEIGHT = 960;           // 画面縦サイズ
var GRID_SIZE = SCREEN_WIDTH / 4;  // グリッドのサイズ
var PIECE_SIZE = GRID_SIZE * 0.95; // ピースの大きさ
var PIECE_NUM_XY = 4;              // 縦横のピース数
var PIECE_OFFSET = GRID_SIZE / 2;  // オフセット値
// メインシーン
phina.define('MainScene', {
  superClass: 'DisplayScene',
  // コンストラクタ
  init: function() {
    // 親クラス初期化
    this.superInit();
    // 背景色
    this.backgroundColor = 'gray';
    // グリッド
    var grid = Grid(SCREEN_WIDTH, PIECE_NUM_XY);
    // ピースグループ
    var pieceGroup = DisplayElement().addChildTo(this);
    var self = this;
    // ピース配置
    PIECE_NUM_XY.times(function(spanX) {
      PIECE_NUM_XY.times(function(spanY) {
        // 番号
        var num = spanY * PIECE_NUM_XY + spanX + 1;
        // ピース作成
        var piece = Piece(num).addChildTo(pieceGroup);
        // Gridを利用して配置
        piece.x = grid.span(spanX) + PIECE_OFFSET;
        piece.y = grid.span(spanY) + PIECE_OFFSET;
        // タッチを有効にする
        piece.setInteractive(true);
        // タッチされた時の処理
        piece.onpointend = function() {
          // ピース移動処理
          self.movePiece(this);
        };
        // 16番のピースは非表示
        if (num === 16) piece.hide();
      });
    });
    // シャッフルボタン
    var shuffleButton = Button({
      text: 'SHUFFLE',
    }).addChildTo(this).setPosition(this.gridX.center(), this.gridY.span(13));
    // ボタンプッシュ時処理
    shuffleButton.onpush = function() {
      // ピースをシャッフル
      (100).times(function() {
        self.shufflePieces();  
      });
    };
    // 参照用
    this.pieceGroup = pieceGroup;
    this.shuffleButton = shuffleButton;
  },
  // 16番ピース（空白）を取得
  getBlankPiece: function() {
    var result = null;
    this.pieceGroup.children.some(function(piece) {
      // 16番ピースを結果に格納
      if (piece.num === 16) {
        result = piece;
        return true;
      }
    });
    return result;
  },
  // ピースの移動処理
  movePiece: function(piece, isInstantly) {
    // 空白ピースを得る
    var blank = this.getBlankPiece();
    // 即入れ替え
    if (isInstantly) {
      var tmpX = piece.x;
      var tmpY = piece.y;
      piece.setPosition(blank.x, blank.y);
      blank.setPosition(tmpX, tmpY);
      return;
    }
    // x, yの座標差の絶対値
    var dx = Math.abs(piece.x - blank.x);
    var dy = Math.abs(piece.y - blank.y);
    // 隣り合わせの判定
    if ((piece.x === blank.x && dy === GRID_SIZE) || 
        (piece.y === blank.y && dx === GRID_SIZE)) {
      // タッチされたピース位置を記憶
      var touchX = piece.x;
      var touchY = piece.y;
      var self = this;
      // tweenerで移動処理
      piece.tweener.clear()
                   .to({x: blank.x, y: blank.y}, 200, "easeOutCubic")
                   .call(function() {
                     // 空白ピースをタッチされたピースの位置へ
                     blank.setPosition(touchX, touchY);
                   });
    }
  },
  // 指定の位置のピースを返す
  getPieceByXY: function(x, y) {
    var result = null;
    this.pieceGroup.children.some(function(piece) {
      // 指定した座標なら
      if (piece.x === x && piece.y === y) {
        result = piece;
        return true;
      }
    });
    return result;
  },
  // ピースをシャッフルする
  shufflePieces: function() {
    var self = this;
    // 隣接ピース格納用
    var pieces = [];
    // 空白ピースを得る
    var blank = this.getBlankPiece();
    // 上下左右隣りのピースがあれば配列に追加
    [1, 0, -1].each(function(i) {
      [1, 0, -1].each(function(j) {
        if (i != j) {
          var x = blank.x + i * GRID_SIZE;
          var y = blank.y + j * GRID_SIZE;
          var target = self.getPieceByXY(x, y);
          if (target) pieces.push(target);
        }
      });
    });
    // 隣接ピースからランダムに選択して空白ピースと入れ替える
    this.movePiece(pieces.random(), 'instantly');
    pieces.clear();
  },
});
// ピースクラス
phina.define('Piece', {
  // RectangleShapeを継承
  superClass: 'phina.display.RectangleShape',
    // コンストラクタ
    init: function(num) {
      // 親クラス初期化
      this.superInit({
        width: PIECE_SIZE,
        height: PIECE_SIZE,
        cornerRadius: 10,
        fill: 'silver',
        stroke: 'white',
      });
      // 数字
      this.num = num;
      // 数字表示用ラベル
      this.label = Label({
        text: this.num + '',
        fontSize: PIECE_SIZE * 0.8,
        fill: 'white',
      }).addChildTo(this);
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
<a href="http://runstant.com/alkn203/projects/9322fad6" target="_blank">[runstantで確認]</a>

### コード説明

```js
// シャッフルボタン
var shuffleButton = Button({
  text: 'SHUFFLE',
}).addChildTo(this).setPosition(this.gridX.center(), this.gridY.span(13));
// ボタンプッシュ時処理
shuffleButton.onpush = function() {
    // ピースをシャッフル
    (100).times(function() {
      self.shufflePieces();  
    });
};
```

* **MainScnene**に**Button**クラスを使ってシャッフルボタンを追加しています。
* **shuffleButton.onpush**にボタンが押された時の処理を記述しています。**times**を使って100回シャッフルしています。**shufflePieces**関数については後述します。

```js
// 指定の位置のピースを返す
  getPieceByXY: function(x, y) {
    var result = null;
    this.pieceGroup.children.some(function(piece) {
      // 指定した座標なら
      if (piece.x === x && piece.y === y) {
        result = piece;
        return true;
      }
    });
    return result;
  },

```

**getPieceByXY**は、指定された位置のピースを返します。該当ピースがあればそのピース、なければ**null**を返します。この後の**shufflePieces**から呼ばれます。

```js
// ピースをシャッフルする
  shufflePieces: function() {
    var self = this;
    // 隣接ピース格納用
    var pieces = [];
    // 空白ピースを得る
    var blank = this.getBlankPiece();
    // 上下左右隣りのピースがあれば配列に追加
    [1, 0, -1].each(function(i) {
      [1, 0, -1].each(function(j) {
        if (i != j) {
          var x = blank.x + i * GRID_SIZE;
          var y = blank.y + j * GRID_SIZE;
          var target = self.getPieceByXY(x, y);
          if (target) pieces.push(target);
        }
      });
    });
    // 隣接ピースからランダムに選択して空白ピースと入れ替える
    this.movePiece(pieces.random(), 'instantly');
    pieces.clear();
  },
  ```

  * **空白ピースの上下左右隣りのピースがあるか調べて、その中からランダムに選んで移動させる**という処理をしています。
  * **movePiece**の引数を一つ増やして、**tweener**を使わずに即座に移動するかどうかを指定しています。

  ```js
  // ピースの移動処理
  movePiece: function(piece, isInstantly) {
    // 空白ピースを得る
    var blank = this.getBlankPiece();
    // 即入れ替え
    if (isInstantly) {
      var tmpX = piece.x;
      var tmpY = piece.y;
      piece.setPosition(blank.x, blank.y);
      blank.setPosition(tmpX, tmpY);
      return;
    }
    // x, yの座標差の絶対値
    var dx = Math.abs(piece.x - blank.x);
    var dy = Math.abs(piece.y - blank.y);
    // 隣り合わせの判定
    if ((piece.x === blank.x && dy === GRID_SIZE) ||
        (piece.y === blank.y && dx === GRID_SIZE)) {
      // タッチされたピース位置を記憶
      var touchX = piece.x;
      var touchY = piece.y;
      var self = this;
      // tweenerで移動処理
      piece.tweener.clear()
                   .to({x: blank.x, y: blank.y}, 200, "easeOutCubic")
                   .call(function() {
                     // 空白ピースをタッチされたピースの位置へ
                     blank.setPosition(touchX, touchY);
                   });
    }
```

* 第二引数が指定されていた場合、シャッフル処理の一環と判断して**tweener**でアニメーションさせずに即座に移動させます。最初に書いてた移動処理を復活させただけです。
* 15パズルのシャッフルのポイントは、実際に可能な移動をさせるという点です。そうすればパズルが解けなくなるという心配もありません。

## クリア判定
15パズルにおけるクリアとは、言うまでもなく**全てのピースが正しい位置に収まった時**です。
シャッフルボタンを押した後にピースを正しい位置に揃えると、リザルトシーンに移行するようにします。
コードは以下のとおりです。

```js
phina.globalize();
// 定数
var SCREEN_WIDTH = 640;            // 画面横サイズ
var SCREEN_HEIGHT = 960;           // 画面縦サイズ
var GRID_SIZE = SCREEN_WIDTH / 4;  // グリッドのサイズ
var PIECE_SIZE = GRID_SIZE * 0.95; // ピースの大きさ
var PIECE_NUM_XY = 4;              // 縦横のピース数
var PIECE_OFFSET = GRID_SIZE / 2;  // オフセット値
// メインシーン
phina.define('MainScene', {
  superClass: 'DisplayScene',
  // コンストラクタ
  init: function() {
    // 親クラス初期化
    this.superInit();
    // 背景色
    this.backgroundColor = 'gray';
    // グリッド
    var grid = Grid(SCREEN_WIDTH, PIECE_NUM_XY);
    // ピースグループ
    var pieceGroup = DisplayElement().addChildTo(this);
    var self = this;
    // ピース配置
    PIECE_NUM_XY.times(function(spanX) {
      PIECE_NUM_XY.times(function(spanY) {
        // 番号
        var num = spanY * PIECE_NUM_XY + spanX + 1;
        // ピース作成
        var piece = Piece(num).addChildTo(pieceGroup);
        // Gridを利用して配置
        piece.x = grid.span(spanX) + PIECE_OFFSET;
        piece.y = grid.span(spanY) + PIECE_OFFSET;
        // タッチを有効にする
        piece.setInteractive(true);
        // タッチされた時の処理
        piece.onpointend = function() {
          // ピース移動処理
          self.movePiece(this);
        };
        // 16番のピースは非表示
        if (num === 16) piece.hide();
      });
    });
    // シャッフルボタン
    var shuffleButton = Button({
      text: 'SHUFFLE',
    }).addChildTo(this).setPosition(this.gridX.center(), this.gridY.span(13));
    // ボタンプッシュ時処理
    shuffleButton.onpush = function() {
      // ピースをシャッフル
      (100).times(function() {
        self.shufflePieces();  
      });
    };
    // 参照用
    this.pieceGroup = pieceGroup;
    this.shuffleButton = shuffleButton;
  },
  // 16番ピース（空白）を取得
  getBlankPiece: function() {
    var result = null;
    this.pieceGroup.children.some(function(piece) {
      // 16番ピースを結果に格納
      if (piece.num === 16) {
        result = piece;
        return true;
      }
    });
    return result;
  },
  // ピースの移動処理
  movePiece: function(piece, isInstantly) {
    // 空白ピースを得る
    var blank = this.getBlankPiece();
    // 即入れ替え
    if (isInstantly) {
      var tmpX = piece.x;
      var tmpY = piece.y;
      piece.setPosition(blank.x, blank.y);
      blank.setPosition(tmpX, tmpY);
      return;
    }
    // x, yの座標差の絶対値
    var dx = Math.abs(piece.x - blank.x);
    var dy = Math.abs(piece.y - blank.y);
    // 隣り合わせの判定
    if ((piece.x === blank.x && dy === GRID_SIZE) || 
        (piece.y === blank.y && dx === GRID_SIZE)) {
      // タッチされたピース位置を記憶
      var touchX = piece.x;
      var touchY = piece.y;
      var self = this;
      // tweenerで移動処理
      piece.tweener.clear()
                   .to({x: blank.x, y: blank.y}, 200, "easeOutCubic")
                   .call(function() {
                     // 空白ピースをタッチされたピースの位置へ
                     blank.setPosition(touchX, touchY);
                   });
    }
  },
  // 指定の位置のピースを返す
  getPieceByXY: function(x, y) {
    var result = null;
    this.pieceGroup.children.some(function(piece) {
      // 指定した座標なら
      if (piece.x === x && piece.y === y) {
        result = piece;
        return true;
      }
    });
    return result;
  },
  // ピースをシャッフルする
  shufflePieces: function() {
    var self = this;
    // 隣接ピース格納用
    var pieces = [];
    // 空白ピースを得る
    var blank = this.getBlankPiece();
    // 上下左右隣りのピースがあれば配列に追加
    [1, 0, -1].each(function(i) {
      [1, 0, -1].each(function(j) {
        if (i != j) {
          var x = blank.x + i * GRID_SIZE;
          var y = blank.y + j * GRID_SIZE;
          var target = self.getPieceByXY(x, y);
          if (target) pieces.push(target);
        }
      });
    });
    // 隣接ピースからランダムに選択して空白ピースと入れ替える
    this.movePiece(pieces.random(), 'instantly');
    pieces.clear();
  },
});
// ピースクラス
phina.define('Piece', {
  // RectangleShapeを継承
  superClass: 'phina.display.RectangleShape',
    // コンストラクタ
    init: function(num) {
      // 親クラス初期化
      this.superInit({
        width: PIECE_SIZE,
        height: PIECE_SIZE,
        cornerRadius: 10,
        fill: 'silver',
        stroke: 'white',
      });
      // 数字
      this.num = num;
      // 数字表示用ラベル
      this.label = Label({
        text: this.num + '',
        fontSize: PIECE_SIZE * 0.8,
        fill: 'white',
      }).addChildTo(this);
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

<a href="http://runstant.com/alkn203/projects/90d6fc13" target="_blank">[runstantで確認]</a>

## コード説明

```js
// Gridを利用して配置
piece.x = grid.span(spanX) + PIECE_OFFSET;
piece.y = grid.span(spanY) + PIECE_OFFSET;
// 正解の位置を記憶させておく
piece.correctX = piece.x;
piece.correctY = piece.y;
```

ピースの初期位置をプロパティとして保存しておきます。

```js
// 一回でも押されたかどうか
shuffleButton.isPushed = false;
// ボタンプッシュ時処理
shuffleButton.onpush = function() {
    // ピースをシャッフル
    (2).times(function() {
      self.shufflePieces();  
    });
    // 残りステップリセット
    self.step = 200;
    // 押されたフラグ立て
    this.isPushed = true;
};
```

* シャッフルボタンに**isPushed**というフラグを作っています。これはシャッフルされた後だけにクリア判定するために使われます。
* スコア用に残りステップ数を設けています。

```js
// tweenerで移動処理
piece.tweener.clear()
             .to({x: blank.x, y: blank.y}, 200, "easeOutCubic")
             .call(function() {
               // 空白ピースをタッチされたピースの位置へ
               blank.setPosition(touchX, touchY);
               // 残りステップを減らす
               self.step--;
               // クリアチェック
               if (self.shuffleButton.isPushed) self.checkPiecePosition();
             });
```

ピースが移動し終わったタイミングでクリア判定を行っています。**checkPiecePosition**については後述します。

```js
// クリア判定
checkPiecePosition: function() {
  // 正しくない位置のピースがあるかチェックする
  var result = this.pieceGroup.children.some(function(piece) {
    if (piece.x != piece.correctX || piece.y != piece.correctY) return true;
  });
  // 全て正しい位置ならクリア画面へ
  var score = this.step;
  if (!result) {
    this.exit({
      score: score,
      message: '15 Puzzle Clear!'
    });
  }
},
```

* 「全てのピースが正しい位置」ということは「正しくない位置のピースが一つでもある」と同じ意味なので、該当ピースがあったら即座に関数を抜けるようにしています。
* 全て正しい位置なら、残りステップ数を**score**に渡して**exit**でリザルトシーンに遷移させます。

## まとめ
今回で[phina.js](http://phinajs.com/)を使った15パズル作成のチュートリアルは終わりです。分かりづらい部分も多々あったかと思いますが、少しでも[phina.js](http://phinajs.com/)を使ったゲーム作りのヒントになれば幸いです。最後まで読んで頂きありがとうございました。