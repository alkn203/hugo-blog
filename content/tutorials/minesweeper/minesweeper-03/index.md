+++
date = "2020-10-16T20:55:06+09:00"
draft = false
slug = ""
tags = ["minesweeper"]
title = "【phina.js】ゲーム作成チュートリアル（マインスイーパー）=第3回 パネルを開く="
eyecatch = "minesweeper03.png"
+++

## 今回の目標
前回はランダムに爆弾を配置しました。今回はパネルを開いて周りにある爆弾の数が表示されるようにします。

![minesweeper03.png](minesweeper03.png)

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

    var self = this;
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
        // 開かれているかどうか
        panel.isOpen = false;
        // タッチ有効化
        panel.setInteractive(true);
        // パネルタッチ時
        panel.onpointstart = function() {
          self.openPanel(panel);
        };
        // 爆弾なら表示
        if (panel.isBomb) Bomb().addChildTo(panel);
      });
    });
    // 参照用
    this.panelGroup = panelGroup;
  },
  // パネルを開く処理
  openPanel: function(panel) {
    // 既に開かれていた何もしない
    if (panel.isOpen) return;
    // 開いたとフラグを立てる
    panel.isOpen = true;
    // タッチ不可にする
    panel.setInteractive(false);
    
    var bombs = 0;
    var indexs = [-1, 0, 1];
    var self = this;
    // 周りのパネルの爆弾数をカウント
    indexs.each(function(i) {
      indexs.each(function(j) {
        var pos = Vector2(panel.x + i * GRID_SIZE, panel.y + j * GRID_SIZE);
        var target = self.getPanel(pos);
        if (target && target.isBomb) bombs++;
      });
    });
    // パネルに数を表示
    panel.num = bombs === 0 ? '' : bombs;
    Label({
      text: panel.num,
      fill: 'white',
    }).addChildTo(panel);
    panel.fill = 'gray';
  },
  // 指定された位置のパネルを得る
  getPanel: function(pos) {
    var result = null;
    
    this.panelGroup.children.some(function(panel) {
      if (panel.position.equals(pos)) {
        result = panel;
        return true;
      } 
    });
    return result;
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

[runstantで確認](http://runstant.com/alkn203/projects/4edca5d9)

## MainScene
### init関数
```js
  init: function() {
    (略)
    var self = this;
    // ピース配置
    PANEL_NUM_XY.times(function(spanX) {
      PANEL_NUM_XY.times(function(spanY) {
        (略)
        // 開かれているかどうか
        panel.isOpen = false;
        // タッチ有効化
        panel.setInteractive(true);
        // パネルタッチ時
        panel.onpointstart = function() {
          self.openPanel(panel);
        };
        (略)
      });
    });
    // 参照用
    this.panelGroup = panelGroup;
  },
  ```
  
  * 後に**MainScene**を**this**として参照するために、**self**に代入しています。
  * パネルが既に開かれているかどうかのフラグ変数**isOpen**を作成しています。
  * パネルをタッチできるように**setInteractive(true)**としています。
  * パネルのタッチイベント**onpointstart**が発生した際に**openPanel**関数を呼び出すようにしています。
  * 最後に**panelGroup**を別の関数からも参照できるように**this.panelGroup**に代入しています。
  
### openPanel関数(新規追加)
  ```js
  // パネルを開く処理
  openPanel: function(panel) {
    // 既に開かれていた何もしない
    if (panel.isOpen) return;
    // 開いたとフラグを立てる
    panel.isOpen = true;
    // タッチ不可にする
    panel.setInteractive(false);
  ```
  
  * パネルを開いた直後の処理です。既に開かれていたら以降の処理をパスします。
  * フラグを変更して、**setInteractive(false)**でタッチ不可にします。
  
```js
    var bombs = 0;
    var indexs = [-1, 0, 1];
    var self = this;
    // 周りのパネルの爆弾数をカウント
    indexs.each(function(i) {
      indexs.each(function(j) {
        var pos = Vector2(panel.x + i * GRID_SIZE, panel.y + j * GRID_SIZE);
        var target = self.getPanel(pos);
        if (target && target.isBomb) bombs++;
      });
    });
    // パネルに数を表示
    panel.num = bombs === 0 ? '' : bombs;
    Label({
      text: panel.num,
      fill: 'white',
    }).addChildTo(panel);
    panel.fill = 'gray';
  },
```
  
  * インデックス値を利用して周りのパネルに爆弾が何個あるかを調べます。
  * **getPanel**関数は、与えられた位置（Vector2）にパネルがあるかを返します。
  * パネルが爆弾ならカウントして、ラベルで結果を表示しています。
  
### getPanel関数（新規追加）
  ```js
  // 指定された位置のパネルを得る  
  getPanel: function(pos) {
    var result = null;
    
    this.panelGroup.children.some(function(panel) {
      if (panel.position.equals(pos)) {
        result = panel;
        return true;
      } 
    });
    return result;
  },
  ```
  
* **panelGroup**を調べて指定された位置にパネルがあれば、それを返します。
* **some**を使って条件が合ったらループを抜けるようにしています。

## 今回はここまで
ここまでで、パネルを開いて爆弾数を表示することが出来ました。
次回は、再帰処理を使ってパネルが連鎖で開くようにします。