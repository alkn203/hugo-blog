+++
date = "2016-01-01T21:55:06+09:00"
draft = false
slug = ""
tags = ["tutorial", "15puzzle"]
title = "【phina.js】ゲーム作成チュートリアル（15パズル）（その１）【ピースの配置】"

+++

### はじめに
以前にQiitaに[tmlib.js ＝15パズル＝ チュートリアル編](http://qiita.com/alkn203/items/3220d55d85a13c69e6c6)を投稿しましたが、今回は、これを[phina.js](http://phinajs.com/)で作り直すのが目標です。

このチュートリアルでは、[phina.js](http://phinajs.com/)の公式エディタである[runstant](http://runstant.com/about)を使っていきたいと思います。
それでは、早速作っていきます。

### ひな形の用意
[runstant](http://runstant.com/about)で用意した以下のひな形をベースに作成していきます。

<div class='runstant'><iframe src='http://goo.gl/flgqTs' width='100%' height='640px' style='border:0px;box-shadow:0px 0px 2px 0px #aaa'></iframe></div>

### コード説明
```js
phina.globalize();
```

**phina.game.GameApp**のように階層的に定義されているクラスを**GameApp**だけで呼び出せるようにする処理などをしています。とりあえずは、必要なおまじないと覚えておいてください。

```js
// メインシーン
phina.define('MainScene', {
  superClass: 'CanvasScene',
  // コンストラクタ
  init: function() {
    // 親クラス初期化
    this.superInit();
  },
});
```

* **phina.define**は、クラスを定義する関数です。
* **CanvasScene**を継承した**MainScene**を定義しています。
* **init**関数は、いわゆる**コンストラクタ**です。
* **this.superInit**で親クラスの**コンストラクタ**を呼び出しています。この後に、実際のゲーム処理コードを書いていくことになります。

```js
// メイン
phina.main(function() {
  var app = GameApp({
    startLabel: 'main',
  });
  app.run();
});
```

* **phina.main**は、メイン関数です。
* **GameApp**のコンストラクタに、**連想配列形式**でパラメータを与えます。
* **startLabel**で最初に読み込む**Scene**を指定します。今回は**main**が指定されているので、**MainScene**が最初に表示されます。

現時点では、[runstant](http://runstant.com/about)画面上部メニューの**play**で実行すると何もない画面が表示されるだけです。

### 今回の目標
以下のように、15パズルのピースを配置します。
<div class='runstant'><iframe src='http://goo.gl/1oucz4' width='100%' height='640px' style='border:0px;box-shadow:0px 0px 2px 0px #aaa'></iframe></div>

### コード説明
### 定数の定義
メインシーンの前にピースサイズなどを定数として定義します。

```js
// 定数
var SCREEN_WIDTH = 640;            // 画面横サイズ
var SCREEN_HEIGHT = 960;           // 画面縦サイズ
var GRID_SIZE = SCREEN_WIDTH / 4;  // グリッドのサイズ
var PIECE_SIZE = GRID_SIZE * 0.95; // ピースの大きさ
var PIECE_NUM_XY = 4;              // 縦横のピース数
var PIECE_OFFSET = GRID_SIZE / 2;  // オフセット値
```

### ピースクラスの定義
後々の利便性を考え、**phina.define**で以下のようにクラス化します。

```js
// ピースクラス
phina.define('Piece', {
  // RectangleShapeを継承
  superClass: 'phina.display.RectangleShape',
    // コンストラクタ
    init: function() {
      // 親クラス初期化
      this.superInit({
        width: PIECE_SIZE,
        height: PIECE_SIZE,
        cornerRadius: 10,
        fill: 'silver',
        stroke: 'white',
      });
    },
});
```

* **Piece**クラスは[phina.js](http://phinajs.com/)で最初から用意されている**RectangleShape**クラス（矩形）を継承し、**init**関数内で親クラス(RectangleShape）にパラメータを渡して初期化しています。
* ピースは正方形ですので、**width**と**height**には同じ**PIECE_SIZE**を与えています。
* **cornerRadius**プロパティを設定することで、角丸の四角形にすることができます。
* **fill**は塗りつぶしの色、**stroke**は枠の色です。

[phina.js](http://phinajs.com/)には他にも円や三角形などの**Shape**（基本図形）が用意されており、どれも最小限のパラメータで描画することができるので、簡単な動作確認の際に重宝します。
**Shape**の使い方については、作者である[phi](https://twitter.com/phi_jp)さんの記事[phina.js を使って様々な図形を表示してみよう](http://phiary.me/phinajs-shape-collection/)が参考になります。

### ピースの配置
今回の目的であるピースの配置について説明します。

```js
// グリッド
var grid = Grid(SCREEN_WIDTH, PIECE_NUM_XY);
// ピースグループ
var pieceGroup = CanvasElement().addChildTo(this);
// ピース配置
PIECE_NUM_XY.times(function(spanX) {
  PIECE_NUM_XY.times(function(spanY) {
    // ピース作成
    var piece = Piece().addChildTo(pieceGroup);
    // Gridを利用して配置
    piece.x = grid.span(spanX) + PIECE_OFFSET;
    piece.y = grid.span(spanY) + PIECE_OFFSET;
  });
});
```

* 画面幅をピースの並び数で区割りした**Grid**を作成しています。**Grid**クラスの使い方については、手前味噌ですみませんが[【phina.js】Gridクラスを使いこなそう](http://qiita.com/alkn203/items/d176a10d4e38d15e4062)を参考にして下さい。

* **CanvasElement**クラスを使って、**pieceGroup**という名前のグループを作っています。グループ管理については、こちらも手前味噌ですが[【phina.js】グループ管理の基本テクニック](http://qiita.com/alkn203/items/8ad0b80175d23d03bd49)を参考にして下さい。

* ピースの配置は2重ループ処理で行いますが、今回は、[phina.js](http://phinajs.com/)独自の**times**メソッドを使用しています。**PIECE_NUM_XY**には**4**という数値が入っていますので、引数で与えられた**function**内の処理を**4回繰り返す**という意味になります。**spanX**と**spanY**には、インデックス値である**0～3**の数値がループ処理で入ってきますので、これを上手く利用します。
* 次にピースを作成して、先に作った**pieceGroup**に追加しています。

* 最後に、グリッドを利用してピースを配置しています。定数の定義部分でグリッドのサイズよりピースのサイズを少し小さくしていますが、これにより**padding**に似た効果を得ることができます。なお、[phina.js](http://phinajs.com/)でのオブジェクトの座標値はオブジェクトの原点（デフォルトではオブジェクトの中心）となりますので、**PIECE_OFFSET**で位置を調整しています。**PIECE_OFFSET**の箇所を削除して実行してみると、結果の違いが分かるかと思います。

* 作成した**pieceGroup**を**MainScene**に**addChildTo**することで、ピースが画面に表示されます。

### 今回はここまで
ピースは配置できましたが、現時点ではただのタイルの集まりですね。
次回は、[ピースに数字を表示](http://alkn203.github.io/blog/2016/01/01/15puzzle-tut-02/)させたいと思います。
