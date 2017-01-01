+++
date = "2016-02-18T23:59:06+09:00"
draft = false
slug = ""
tags = ["tips","DisplayElement","shape","group"]
title = "オブジェクトのグループ間移動のサンプル"
eyecatch = "/images/phina-group-change.png"
+++

### 今回のサンプル
**phina.js**において複数のオブジェクトを取り扱う時には、自ずとグループ管理が必須になってきます。
グループ管理には、**DisplayElement**(旧CanvasElement)を活用しますが、例えば、AグループとBグループがあった場合

```js
Aグループの要素.addChildTo(Bグループ);
```

とすることで、**要素はAグループからBグループへ移動**します。今回のサンプルでは、この仕様を利用して**落下するオブジェクトと固定されたオブジェクトの当たり判定**を行っています。

<center>![phina-group-change](/images/phina-group-change.png)</center>

### コード
```js
phina.globalize();
// 定数
var SCREEN_HEIGHT = 960;
// メインシーン
phina.define('MainScene', {
  superClass: 'DisplayScene',
  // コンストラクタ
  init: function() {
    // 親クラス初期化
    this.superInit();
    // 背景色
    this.backgroundColor = 'gray';
    // 落下グループ
    this.dynamicGroup = DisplayElement().addChildTo(this);
    // 固定グループ
    this.staticGroup = DisplayElement().addChildTo(this);
  },
  // 毎フレーム更新
  update: function(app) {
    // 一定フレーム毎にブロック作成
    if (app.frame % 50 === 0) {
      Block().addChildTo(this.dynamicGroup)
             .setPosition(this.gridX.center(Random.randint(-1, 1)), 0)
             .physical.gravity.set(0, 0.25);
    }
    
    var self = this;
    // 画面下との当たり判定
    this.dynamicGroup.children.each(function(block) {
      if (block.bottom > SCREEN_HEIGHT) {
        block.disable();
        // 位置補正
        block.bottom = SCREEN_HEIGHT;
        // 固定グループへ移動
        block.addChildTo(self.staticGroup);
      }  
    });
    // 固定ブロックとの当たり判定
    this.dynamicGroup.children.each(function(block) {
      self.staticGroup.children.each(function(target) {
        if (block.hitTestElement(target)) {
          block.disable();
          block.bottom = target.top;
          block.addChildTo(self.staticGroup);
        }  
      });
    });
  },
});
// ブロッククラス
phina.define('Block', {
  // RectangleShapeクラスを継承
  superClass: 'RectangleShape',
  // コンストラクタ
  init: function() {
    this.superInit({
      width: 40,
    });
  },
  // 固定ブロック仕様にする
  disable: function() {
    this.physical.gravity.y = 0;
    this.physical.velocity.y = 0;
    this.fill = 'silver';
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

<a href="http://runstant.com/alkn203/projects/8421fe5a" target="_blank">[runstantで確認]</a>


