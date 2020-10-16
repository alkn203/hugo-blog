+++
date = "2020-10-16T20:56:06+09:00"
draft = false
slug = ""
tags = ["minesweeper"]
title = "【phina.js】ゲーム作成チュートリアル（マインスイーパー）=第４回 パネルを連鎖で開く="
eyecatch = "minesweeper03.png"
+++

## 今回の目標
前回はパネルを開くと周りにある爆弾の数が表示されるようにしました。今回は、周りに爆弾がない場合にパネルを連鎖で開くようにします。

## openPanel関数
  ```js
  // パネルを開く処理
  openPanel: function(panel) {
    (略)
    // パネルに数を表示
    panel.num = bombs === 0 ? '' : bombs;
    Label({
      text: panel.num,
      fill: 'white',
    }).addChildTo(panel);
    panel.fill = 'gray';
    // 周りに爆弾がなければ再帰的に調べる
    if (bombs === 0) {
      indexs.each(function(i) {
        indexs.each(function(j) {
          var pos = Vector2(panel.x + i * GRID_SIZE, panel.y + j * GRID_SIZE);
          var target = self.getPanel(pos);
          target && self.openPanel(target);
        });
      });
    }
  },
```
  
周りに爆弾がない場合、現在のパネルを起点にして**getPanel**を再帰呼び出しします。
今回の変更点はこれだけです。

## 今回はここまで
ここまでで、パネルを連鎖で開くことができるようになりました。
段々と形になってきたと思いますが、爆弾も開けたりと全然ゲームになっていません。
次回は、ゲームオーバーとクリア処理を追加します。

[runstantで確認](http://runstant.com/alkn203/projects/943f0dea)