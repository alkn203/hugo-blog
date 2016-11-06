+++
date = "2016-07-04T23:59:06+09:00"
draft = false
slug = ""
tags = ["tutorial", "minesweeper"]
title = "ゲーム作成チュートリアル（マインスイーパー）（その３）【パネルを開く】"

+++

### 今回の目標
[前回](http://alkn203.github.io/blog/2016/07/03/minesweeper-tut-02/)はランダムに爆弾を配置しました。
今回はパネルを開いて周りにある爆弾の数が表示されるようにします。

<div class='runstant'><iframe src='http://runstant.com/alkn203/projects/4edca5d9/full' width='100%' height='640px' style='border:0px;box-shadow:0px 0px 2px 0px #aaa'></iframe></div>

### コード説明

#### MainScene
#### init関数
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
  
#### openPanel関数(新規追加)
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
  
#### getPanel関数（新規追加）
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

### 今回はここまで
ここまでで、パネルを開いて爆弾数を表示することが出来ました。
次回は、再帰処理を使ってパネルが連鎖で開くようにします。

### runstantプロジェクトへのリンク
http://runstant.com/alkn203/projects/a24d063d