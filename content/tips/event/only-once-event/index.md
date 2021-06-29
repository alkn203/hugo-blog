+++
date = "2021-06-24T16:59:06+09:00"
draft = false
slug = ""
tags = ["event"]
title = "一度だけ発行されるイベントを登録する"
eyecatch = "only-once-event.png"
+++

![only-once-event](only-once-event.png)

## one メソッド
１回だけ発行するイベントは、**one**を使って登録すると便利です。

```js
// タッチ関数（一度のみ実行）
shape.one('pointstart', function() {
    alert("タッチされました");
});
```

実行すると、タッチイベントが1回だけしか発生しないのが分かります。

## サンプルコード
<details>
<summary>コードを見る</summary>

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
    // Shapeを作成してシーンに追加・位置指定
    var shape = Shape().addChildTo(this).setPosition(320, 480);
    // 背景色設定
    shape.backgroundColor = 'red';
    // タッチ可能にする
    shape.setInteractive(true);
    // タッチ関数（一度のみ実行）
  	shape.one('pointstart', function() {
	    alert("タッチされました");
  	});
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

</details>

## runstantプロジェクト
https://runstant.com/alkn203/projects/551c7449