+++
date = "2016-06-30T23:59:06+09:00"
draft = false
slug = ""
tags = ["tutorial", "minesweeper"]
title = "ゲーム作成チュートリアル（マインスイーパー）（その１）【パネルの配置】"
eyecatch = "/images/minesweeper-tut-1.png"
+++

## はじめに
以前にQiitaに[tmlib.js ＝マインスイーパー＝ チュートリアル編（目次）](http://qiita.com/alkn203/items/a533b1264b912dec9590)を投稿しましたが、今回は、これを[phina.js](http://phinajs.com/)で作り直すのが目標です。なお、本ブログのこれまでのチュートリアルを見て、「**phina.js**でのゲーム作り」を少しは知っているということを前提にしています。
このチュートリアルでは、[phina.js](http://phinajs.com/)の公式開発環境として更に便利になった[runstant](http://runstant.com/)を使っていきたいと思います。

## 今回の目標
以下のようにパネルを並べます。

<center>![15puzzle-tut-2](/images/minesweeper-tut-1.png)</center>

## 今回のコード
```js
phina.globalize();
// 定数
var SCREEN_WIDTH = 640; // 画面横サイズ
var PANEL_NUM_XY = 9; // 縦横のパネル数
var GRID_SIZE = (SCREEN_WIDTH - 10) / PANEL_NUM_XY; // グリッドのサイズ
var SCREEN_HEIGHT = GRID_SIZE * 11; // 画面縦サイズ
var PANEL_SIZE = GRID_SIZE * 0.9; // パネルの大きさ
var PANEL_OFFSET = (GRID_SIZE + 10) / 2; // オフセット値
// メインシーン
phina.define('MainScene', {
  superClass: 'DisplayScene',
  // コンストラクタ
  init: function() {
    // 親クラス初期化
    this.superInit({
      width: SCREEN_WIDTH,
      height: SCREEN_HEIGHT,
    });
    // 背景色
    this.backgroundColor = 'gray';
    // グリッド
    var grid = Grid(GRID_SIZE * PANEL_NUM_XY, PANEL_NUM_XY);
    // グループ
    var panelGroup = DisplayElement().addChildTo(this);
    // ピース配置
    PANEL_NUM_XY.times(function(spanX) {
      PANEL_NUM_XY.times(function(spanY) {
        // パネル作成
        var panel = Panel().addChildTo(panelGroup);
        // Gridを利用して配置
        panel.x = grid.span(spanX) + PANEL_OFFSET;
        panel.y = grid.span(spanY) + PANEL_OFFSET;
      });
    });
  },
});
// パネルクラス
phina.define('Panel', {
  // RectangleShapeを継承
  superClass: 'RectangleShape',
    // コンストラクタ
    init: function() {
      // 親クラス初期化
      this.superInit({
        width: PANEL_SIZE,
        height: PANEL_SIZE,
        fill: 'silver', // 塗りつぶし色
        stroke: 'white', // 枠の色
        cornerRadius: 2, // 角の丸み
      });
    },
});
// メイン
phina.main(function() {
  var app = GameApp({
    startLabel: 'main', // メイン画面からスタート
    width: SCREEN_WIDTH,
    height: SCREEN_HEIGHT,
  });
  app.run();
});
```

<a href="http://runstant.com/alkn203/projects/ada3aa47" target="_blank">[runstantで確認]</a>

## コード説明
### 定数の定義
```js
// 定数
var SCREEN_WIDTH = 640; // 画面横サイズ
var PANEL_NUM_XY = 9; // 縦横のパネル数
var GRID_SIZE = (SCREEN_WIDTH - 10) / PANEL_NUM_XY; // グリッドのサイズ
var SCREEN_HEIGHT = GRID_SIZE * 11; // 画面縦サイズ
var PANEL_SIZE = GRID_SIZE * 0.9; // パネルの大きさ
var PANEL_OFFSET = (GRID_SIZE + 10) / 2; // オフセット値
```
* ゲームに必要な各種定数を定義しています。
* グリッドのサイズは、出来るだけ大きいサイズでパネル数で分割出来るように計算で求めています。
* パネルの大きさをグリッドサイズより少し小さくしてるのは、配置するときに隙間を開けるためです。
* パネルを正しい位置に配置できるようにオフセット値を計算しています。

### Panelクラス
メイン処理の前にパネルクラスについて説明します。
```js
// パネルクラス
phina.define('Panel', {
  // RectangleShapeを継承
  superClass: 'RectangleShape',
    // コンストラクタ
    init: function() {
      // 親クラス初期化
      this.superInit({
        width: PANEL_SIZE,
        height: PANEL_SIZE,
        fill: 'silver', // 塗りつぶし色
        stroke: 'white', // 枠の色
        cornerRadius: 2, // 角の丸み
      });
    },
});
```
* **phina.define**でクラスを定義します。
* パネルは四角形なので**RectangleShape**（矩形）クラスを継承します。
* **this.superInit**で親クラスにパラメーターを与えて初期化します。
* **cornerRadius**は角の丸みを指定します。（tmlib.jsではRoundRectangleShape）

### メインシーン
```js
// コンストラクタ
init: function() {
  // 親クラス初期化
  this.superInit({
    width: SCREEN_WIDTH,
    height: SCREEN_HEIGHT,
  });
```

画面サイズを反映させるためには、**DisplayScene**の親クラスにも**width**と**height**を指定する必要があります。

```js
  // 背景色
  this.backgroundColor = 'gray';
```

**Scene**の背景色を指定しています。

```js
  // グリッド
  var grid = Grid(GRID_SIZE * PANEL_NUM_XY, PANEL_NUM_XY);
```

**Grid**クラスを使って、配置用の情報を生成しています。ここで1グリッドの大きさが決まります。

```js
  // グループ
  var panelGroup = DisplayElement().addChildTo(this);
```

**DisplayElement**を使ってパネルを格納するグループを作成しています。

```js
  // ピース配置
  PANEL_NUM_XY.times(function(spanX) {
    PANEL_NUM_XY.times(function(spanY) {
      // パネル作成
      var panel = Panel().addChildTo(panelGroup);
      // Gridを利用して配置
      panel.x = grid.span(spanX) + PANEL_OFFSET;
      panel.y = grid.span(spanY) + PANEL_OFFSET;
    });
  });
},
```

* パネルをグリッド状に配置する処理です。
* **times**関数は、**phina.js**独自の仕様で、前に付いた値の回数だけ処理を繰り返します。ここでは、**PANEL_NUM_XY**回、つまり9回の繰り返しです。入れ子にしてグリッド状に配置します。
* **Panel**を作成して**panelGroup**に追加しています。
* 最後にパネルの位置を指定します。**span**にインデックス値を指定することで、グリッド状に綺麗に配置することが出来ます。

## 今回はここまで
ここまでで、パネルを配置することができました。
次回は、[爆弾の配置](http://alkn203.github.io/blog/2016/07/03/minesweeper-tut-02/)です。