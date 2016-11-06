+++
date = "2016-09-29T01:55:06+09:00"
draft = false
slug = ""
tags = ["tips", "template"]
title = "phina.jsの基本テンプレートについて"
eyecatch = "/images/phina-template.png"
+++

## phina.jsの基本テンプレート
自分自身の復習のためにも、私が普段１からコーディングをする時に使用しているテンプレートを紹介します。
実行結果は以下のように黒い画面が表示されるだけです。

![phina_template_image](/images/phina-template.png)

<center>[[runstantで実行]](http://runstant.com/alkn203/projects/8f0388a4)</center>

## コード説明

```js
// グローバルに展開
phina.globalize();
/*
 * メインシーン
 */
phina.define("MainScene", {
  // 継承
  superClass: 'DisplayScene',
  // 初期化
  init: function() {
    // 親クラス初期化
    this.superInit();
    // 背景色
    this.backgroundColor = 'black';
    // 以下にコードを書いていく
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
  });
  // fps表示
  //app.enableStats();
  // 実行
  app.run();
});
```

* 基本的には、**MainScene**の**init**関数内に自分のコードを書いていきます。
* **this.backgroundColor**で背景色を指定しないと、デフォルトでは白になります。
* **main**関数内の**GameApp**関数では、**startLabel**を**main**としていますが、これは実行結果をすぐに確認するためにタイトル画面をスキップしたいからです。
* **main**関数内の**enableStats**関数は、画面左上に**fps**を常時表示する便利な機能ですが、処理が重くなるので一旦コメントアウトしています。