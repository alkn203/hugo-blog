+++
date = "2020-10-11T20:55:06+09:00"
draft = false
slug = ""
tags = ["minesweeper"]
title = "【phina.js】ゲーム作成チュートリアル（マインスイーパー）=第2回 爆弾の配置="
eyecatch = "minesweeper02.png"
+++

## 今回の目標
前回はパネルをグリッド状に配置しました。今回はマインスイーパーの主役とも言える爆弾を配置します。爆弾はランダムな場所に配置されるようにします。

![minesweeper02.png](minesweeper02.png)

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
var BOMB_NUM = 10; // 爆弾数
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
    // 爆弾位置をランダムに決めた配列を作成
    var bombs = [];
    (PANEL_NUM_XY * PANEL_NUM_XY).times(function() {
      bombs.push(false);
    });
    bombs.fill(true, 0, 10).shuffle();
    // ピース配置
    PANEL_NUM_XY.times(function(spanX) {
      PANEL_NUM_XY.times(function(spanY) {
        // パネル作成
        var panel = Panel().addChildTo(panelGroup);
        // Gridを利用して配置
        panel.x = grid.span(spanX) + PANEL_OFFSET;
        panel.y = grid.span(spanY) + PANEL_OFFSET;
        // パネルに爆弾情報を紐づける
        panel.isBomb = bombs[spanX * PANEL_NUM_XY + spanY];
        // 爆弾なら表示
        if (panel.isBomb) Bomb().addChildTo(panel);
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
// 爆弾クラス
phina.define('Bomb', {
  // Shapeを継承
  superClass: 'Shape',
    // コンストラクタ
    init: function() {
      // 親クラス初期化
      this.superInit({
        width: GRID_SIZE,
        height: GRID_SIZE,
        backgroundColor: 'transparent',
      });
      // 導線
      RectangleShape({
        width: PANEL_SIZE / 8,
        height: PANEL_SIZE / 8,
        fill: "navy",
        stroke: 'white',
        y: -20,
      }).addChildTo(this);
      // 本体
      CircleShape({
        radius: PANEL_SIZE / 4,
        fill: "navy",
        stroke: 'white',
      }).addChildTo(this);
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

[runstantで確認](http://runstant.com/alkn203/projects/a24d063d)

## 定数の定義
```js
// 定数
（略）
var BOMB_NUM = 10; // 爆弾数
```
設置する爆弾数を定義します。

## 爆弾位置情報の作成
```js
init: function() {
  （略）
  // 爆弾位置をランダムに決めた配列を作成
  var bombs = [];
  (PANEL_NUM_XY * PANEL_NUM_XY).times(function() {
    bombs.push(false);
  });
  bombs.fill(true, 0, 10).shuffle();
```

* 爆弾位置格納用の配列を作ります。
* 一旦**false**で埋めて、先頭の10個まで**true**に置き換えてから**shuffle**関数で要素をランダムに並び替えています。

## 爆弾位置情報をパネルに紐づける
```js
  // ピース配置
  PANEL_NUM_XY.times(function(spanX) {
    PANEL_NUM_XY.times(function(spanY) {
      （略）
      // パネルに爆弾情報を紐づける
      panel.isBomb = bombs[spanX * PANEL_NUM_XY + spanY];
      // 爆弾なら表示
      if (panel.isBomb) Bomb().addChildTo(panel);
    });
  });
},
```

* パネルの**isBomb**プロパティに爆弾かどうかの情報**true** or **false**を設定しています。
* **isBomb**プロパティが**true**なら、爆弾を配置します。

## 爆弾クラス

```js
// 爆弾クラス
phina.define('Bomb', {
  // Shapeを継承
  superClass: 'Shape',
    // コンストラクタ
    init: function() {
      // 親クラス初期化
      this.superInit({
        width: GRID_SIZE,
        height: GRID_SIZE,
        backgroundColor: 'transparent',
      });
      // 導線
      RectangleShape({
        width: PANEL_SIZE / 8,
        height: PANEL_SIZE / 8,
        fill: "navy",
        stroke: 'white',
        y: -20,
      }).addChildTo(this);
      // 本体
      CircleShape({
        radius: PANEL_SIZE / 4,
        fill: "navy",
        stroke: 'white',
      }).addChildTo(this);
    },
});
```

* 新たに**Bomb**クラスを作成しています。
* 爆弾はスプライト画像を使っても良いのですが、今回は敢えて**Shape**を組み合わせて表現してみました。
* 素の**Shape**は**backgroundColor**が設定されているので、これを透明にします。
* 導線は本体の少し上になるようにy座標を指定しています。この場合、親から見た**相対座標**になるということに注意してください。

## 今回はここまで
ここまでで、爆弾をランダムに配置することができました。読み込まれる度に配置が異なることが確認できるかと思います。
次回は、パネルを開く処理を追加します。