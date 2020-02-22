+++
date = "2019-06-19T20:55:06+09:00"
draft = false
slug = ""
tags = ["shooting"]
title = "【phina.js】2Dシューティングチュートリアル =第1回 プログラミングの準備="
eyecatch = "runstant.jpeg"
+++

## プログラミングの準備
今回のチュートリアルでは、**phina.js**の公式オンラインエディタの位置付けである[Runstant](https://runstant.com/)を使用します。

![runstant.jpeg](runstant.jpeg)

## ひな形
私が普段使っている以下の[ひな形](https://qiita.com/alkn203/items/09274a38a0f31ee0c1d5)から作成します。

```javascript
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
  // 毎フレーム更新処理
  update: function() {
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