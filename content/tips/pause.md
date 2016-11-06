+++
date = "2016-04-23T23:59:06+09:00"
draft = false
slug = ""
tags = ["tips"]
title = "ポーズ動作のサンプル"

+++

### サンプル
ゲームでよくあるポーズ機能です。
もっとスマートな方法があるかと思いますが、今回は**update**関数の差し替えを活用しています。

<div class='runstant'>
<iframe src='http://runstant.com/alkn203/projects/5b15c783' width='100%' height='640px' style='border:0px;box-shadow:0px 0px 2px 0px #aaa'>
</iframe>
</div>

### コード

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

    var label = Label({
      text: '画面タッチでポーズ切り替え',
      fontSize: 36,
      fill: 'yellow',
    }).addChildTo(this).setPosition(this.gridX.center(), this.gridY.center(-4));
    label.alpha = 0.7;

    var rect = RectangleShape().addChildTo(this);
    rect.setPosition(this.gridX.center(), this.gridY.center());
    this.rect = rect;
    this.update = this.start;
  },

  start: function() {
    this.rect.rotation += 8;
  },
  
  onpointstart: function() {
    this.update = this.update !== null ? null : this.start;
  } 
});

// メイン
phina.main(function() {
  app = GameApp({
    startLabel: 'main',
  });
  app.run();
});
```