+++
date = "2017-05-22T22:18:06+09:00"
draft = false
slug = ""
tags = ["tips","label", "font"]
title = "ラベルのフォントを指定する"
eyecatch = "/images/change-label-font.gif"
+++ 

## 今回のTips
ラベルのフォントを変える方法について説明します。

![change-label-font.gif](/images/change-label-font.gif)

<center><a href="http://runstant.com/alkn203/projects/e2928435" target="_blank">[runstantで確認]</a></center>

### fontFamilyプロパティでフォントを指定する
ラベルのフォントは**fontFamily**で指定します。

```js
    // ラベル表示
    Label({
      text: 'Time is money',
      fontSize: 64,
      fill: 'blue',
      fontFamily: "'Consolas', 'Monaco', 'ＭＳ ゴシック'",
    }).addChildTo(this).setPosition(this.gridX.center(), this.gridY.center());
```

### コード
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
    // ラベル表示
    Label({
      text: 'Time is money',
      fontSize: 64,
      fill: 'blue',
      fontFamily: "'Consolas', 'Monaco', 'ＭＳ ゴシック'",
    }).addChildTo(this).setPosition(this.gridX.center(), this.gridY.center());
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