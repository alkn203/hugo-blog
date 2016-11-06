+++
date = "2016-07-11T23:59:06+09:00"
draft = false
slug = ""
tags = ["tutorial", "minesweeper"]
title = "ゲーム作成チュートリアル（マインスイーパー）（その５）【ゲームオーバーとクリア処理】"

+++

### 今回の目標
[前回](http://alkn203.github.io/blog/2016/07/04/minesweeper-tut-03/)はパネルが連鎖で開かれるようにしました。
今回は、ゲームオーバーとクリア処理を追加します。

<div class='runstant'><iframe src='http://runstant.com/alkn203/projects/825d660c/full' width='100%' height='640px' style='border:0px;box-shadow:0px 0px 2px 0px #aaa'></iframe></div>

### コード説明
#### ゲームオーバー処理
#### openPanel関数
  ```js
  // パネルを開く処理
  openPanel: function(panel) {
    // 爆弾ならゲームオーバー
    if (panel.isBomb) {
      Explosion().addChildTo(panel);
      this.showAllBombs();
      return;
    }
    // 既に開かれていた何もしない
    （略）
  },
```
  
* 開いたパネルが爆弾だった場合、爆発を表示して、全ての爆弾を表示します。
* **Explosion**クラスと**showAllBombs**関数については後述します。

#### Explosionクラス
  ```js
  // 爆発クラス
  phina.define('Explosion', {
    // StarShapeを継承
    superClass: 'StarShape',
      // コンストラクタ
      init: function() {
        // 親クラス初期化
        this.superInit({
          radius: (PANEL_SIZE + 5) / 2,
          sides: 10,
          sideIndent: 0.75,
          rotation: 15,
          fill: "red",
          stroke: "yellow",
        });
      },
  });
```
* **StarShape**のパラメータを変えて爆発のように見せています。
* パラメータを変えることで、このような一風違った表現も可能です。

#### showAllBombs関数
  ```js
  // 爆弾を全て表示する
  showAllBombs: function() {
    var self = this;
    
    this.panelGroup.children.each(function(panel) {
      panel.setInteractive(false);
      
      if (panel.isBomb) {
        Bomb().addChildTo(panel);
        panel.tweener.clear().scaleTo(1.2, 100)
                     .scaleTo(1.0, 100)
                     .call(function() {
                       // ラベル表示
                       Label({
                        text: 'TOUCH TO RESTART',
                        fill: 'white',
                       }).addChildTo(self).setPosition(320, 700);
                       // 画面をタッチ可能に
                       self.setInteractive(true);
                     });
      }
    });
  },
```
* **Panel**をループで調べて、**isBomb**が**true**なら爆弾を表示します。
* **tweener**で拡大・縮小のアニメーションをつけています。
* アニメーションした後は、ラベルを表示して画面のタッチを有効にしています。

#### リスタート処理 
```js
  // 画面タッチ可能な場合
 onpointstart: function() {
   // 再スタート
   this.exit({
     nextLabel: 'main',  
   });  
 },
```

* 先に出た**showAllBombs**で画面がタッチ有効になっている場合、画面をタッチするとこの関数が呼び出されます。
* **exit**関数で**nextLabel**に**main**を指定することで、メインシーンが呼び直されます。

#### クリア処理
マインスイーパーのクリアとは、爆弾以外のパネルが全部開かれた時です。
結果的には、「全体のパネル数から開かれたパネル数を引いた値が爆弾数と一致」すればよいということになります。
 
```js
// メインシーン
phina.define('MainScene', {
  superClass: 'DisplayScene',
  // コンストラクタ
  init: function() {
    // 親クラス初期化
    （略）
    // 参照用
    this.panelGroup = panelGroup;
    // クリア判定用
    this.oCount = 0;
  },
```

* 開かれたパネル数をカウントするための変数**oCount**を用意します。
* パネルを開いたら**oCount**を増やしていきます。

#### クリア判定
```js
// パネルタッチ時
panel.onpointstart = function() {
  self.openPanel(panel);
  // クリア判定
  if (self.oCount === PANEL_NUM_XY * PANEL_NUM_XY - BOMB_NUM) {
    // ラベル表示
    Label({
      text: 'GOOD JOB!',
      fill: 'white',
    }).addChildTo(self).setPosition(320, 700);
    // パネルを選択不可に
    this.panelGroup.children.each(function(panel) {
      panel.setInteractive(false);
    });
  }
};
```

* パネルを開いた時に開かれた数を調べてクリアチェックをしています。
* クリアならラベルを表示して、全てのパネルを選択不可にしています。

### 今回はここまで
ここまでで、ゲームオーバー処理とクリア処理を実装しました。
この状態で最低限のゲームとしては成立していると思いますが、次回は付加機能を追加したいと
思います。

### runstantプロジェクトへのリンク
http://runstant.com/alkn203/projects/825d660c
