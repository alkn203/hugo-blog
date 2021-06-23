+++
date = "2021-06-22T11:03:06+09:00"
draft = false
slug = ""
tags = ["label"]
title = "Labelのフォントを変更する"
eyecatch = "change-label-font.png"
+++

![change-label-font](change-label-font.png)

## ラベルのフォントを変更する
ラベルのフォントは、**fontFamily** プロパティで変更可能です。

```js
var label2 = Label('フォント１').addChildTo(this);
label2.setPosition(320, 600);
label2.fontFamily = "'sans-serif'";
label2.fontSize = 48;
label2.fill = 'red';
```

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
    this.backgroundColor = 'silver';
    // ラベル表示
    var label = Label('Time is money').addChildTo(this);
    label.setPosition(320, 480);

    var label2 = Label('フォント１').addChildTo(this);
    label2.setPosition(320, 600);
    label2.fontFamily = "'sans-serif'";
    label2.fontSize = 48;
    label2.fill = 'red';

    var label3 = Label('フォント２').addChildTo(this);
    label3.setPosition(320, 720);
    label2.fontFamily = "'serif'";
    label3.fontSize = 60;
    label3.fill = 'yellow';
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
https://runstant.com/alkn203/projects/a22c0250