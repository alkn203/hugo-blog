+++
date = "2017-05-22T22:16:06+09:00"
draft = false
slug = ""
tags = ["tips","label"]
title = "ラベル(文字)を表示する"
eyecatch = "/images/show-label.gif"
+++ 

## 今回のTips
ゲーム制作には欠かせない**ラベル**を表示する方法について説明します。

![show-label.gif](/images/show-label.gif)

<center><a href="http://runstant.com/alkn203/projects/d23ea2d3" target="_blank">[runstantで確認]</a></center>

* 上のサンプルでは、スプライトをタッチする度に速度が遅くなっていきます。

### Labelクラスを使ってラベル（文字）を表示する
以下がラベルを画面の中心に表示するコード例です。

```js
    // ラベル表示
    Label({
      text: 'Time is money',
      fontSize: 64,
    }).addChildTo(this).setPosition(this.gridX.center(), this.gridY.center());
```

**text**プロパティが表示する文字です。他に**fontSize**、**fontWeight**、**fontFamily**などのプロパティがあります。

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
