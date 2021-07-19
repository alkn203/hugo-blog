+++
date = "2021-06-25T14:59:06+09:00"
draft = false
slug = ""
tags = ["class"]
title = "Spriteクラスを継承して独自のクラスを作る"
+++

## クラスを継承する
**Sprite**クラスを継承して新たなクラスを作成する方法について説明します。

```js
/*
 * Tomapikoクラス
 */
phina.define("Tomapiko", {
  // Spriteクラスを継承
  superClass: 'Sprite',
  // コンストラクタ
  init: function() {
    // 親クラス初期化
    this.superInit('tomapiko');
  },
});
```

* **Sprite**クラスを継承した**Tomapikoクラス**を作成しています。
* **superClass**で継承元のクラスを指定します。
* **this.superInit** で継承元のクラス(**Sprite**)を初期化できるので、引数にアセット名を与えます。

## サンプルコード
<details><summary>コードを見る</summary>

```js
// グローバルに展開
phina.globalize();
// アセット
var ASSETS = {
  // 画像
  image: {
    'tomapiko': 'https://cdn.jsdelivr.net/gh/phinajs/phina.js@v0.2.3/assets/images/tomapiko.png',
  },
};
/*
 * メインシーン
 */
phina.define("MainScene", {
  // 継承
  superClass: 'DisplayScene',
  // コンストラクタ
  init: function() {
    // 親クラス初期化
    this.superInit();
    // 背景
    this.backgroundColor = 'skyblue';
    // プレイヤー作成
    var player = Tomapiko().addChildTo(this);
    // 初期位置
    player.x = this.gridX.center();
    player.y = this.gridY.center();
  },
});
/*
 * Tomapikoクラス
 */
phina.define("Tomapiko", {
  // Spriteクラスを継承
  superClass: 'Sprite',
  // コンストラクタ
  init: function() {
    // 親クラス初期化
    this.superInit('tomapiko');
  },
});
/*
 * メイン処理
 */
phina.main(function() {
  // アプリケーションを生成
  var app = GameApp({
    // MainScene から開始
    startLabel: 'main',
    // アセット読み込み
    assets: ASSETS,
  });
  // fps表示
  //app.enableStats();
  // 実行
  app.run();
});
```

</details>

## runstantプロジェクト
{{< targetblank "https://runstant.com/alkn203/projects/d8b84ede" >}}