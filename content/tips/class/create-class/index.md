+++
date = "2021-06-25T13:59:06+09:00"
draft = false
slug = ""
tags = ["class"]
title = "クラスを作成する"
+++

## phina.defineを使ってクラスを作成する
**phina.define**でクラスを定義するときは、必ず何かを継承しなければいけないというわけではありません。以下のように、一般的なクラスも作成することができます。

```js
/*
 * 自作クラス
 */
phina.define("Car", {
  // 初期化
  init: function(maker, name) {
    // クラスメンバ
    this.maker = maker;
    this.name = name;
  },
});
```

## サンプルコード
<details><summary>コードを見る</summary>

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
    // 独自クラス
    var car1 = Car('Honda', 'City');
    var car2 = Car('Nissan', '180');
    // メンバにアクセス
    console.log(car1.name);
    console.log(car2.maker);
  },
});
/*
 * 自作クラス
 */
phina.define("Car", {
  // 初期化
  init: function(maker, name) {
    // クラスメンバ
    this.maker = maker;
    this.name = name;
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
{{< targetblank "https://runstant.com/alkn203/projects/d879cf62" >}}