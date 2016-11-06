+++
date = "2015-10-26T20:50:06+09:00"
draft = false
slug = ""
tags = ["tutorial", "breakout"]
title = "【phina.js】ゲーム作成チュートリアル（ブロック崩し）第1回＝テンプレートから作成する＝"
eyecatch = "/images/phina-template.png"
+++

## はじめに
前回では、[phina.js](https://github.com/phi-jp/phina.js)についての簡単な説明と、作成するゲームの仕上がりをお見せしました。それでは、今回からステップ毎に説明を始めていきたいと思います。

## phina.jsのテンプレート
Runstantで用意した[テンプレート](http://runstant.com/alkn203/projects/8f0388a4)を使います。後々の利便性を考え、リンクをブラウザの新しいタブで開いた方が良いでしょう。

<center>![phina-template](/images/phina-template.png)</center>

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

## コード説明
### グローバル展開
```js
phina.globalize();
```
**phina.game.GameApp**のように階層的に定義されているクラスを**GameApp**だけで呼び出せるようにする処理などをしています。とりあえずは、必要なおまじないと覚えておいてください。

### メインシーン
```js
phina.define("MainScene", {
  // 継承
  superClass: 'DisplayScene',
  // 初期化
  init: function() {
    // 親クラス初期化
    this.superInit();
```

* **phina.define**は、クラスを定義する関数です。
* **DisplayScene**を継承した**MainScene**を定義しています。
* **init**関数は、いわゆる**コンストラクタ**です。
* **this.superInit**で親クラスの**コンストラクタ**を呼び出しています。この後に、実際のゲーム処理コードを書いていくことになります。

### メイン処理
```js
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
* **phina.main**は、メイン関数です。
* **GameApp**のコンストラクタに、**連想配列形式**でパラメータを与えます。
* **startLabel**で最初に読み込む**Scene**を指定します。今回は**main**が指定されているので、**MainScene**が最初に表示されます。

## テンプレートの修正
今回作成するゲーム用に、テンプレートを修正します。

## コード説明（追加・修正部分）

### GameApp関数
```js
  var app = GameApp({
    title: 'Break Out',
  });
```

<a href="http://runstant.com/alkn203/projects/5c10f897" target="_blank">[runstantで確認]</a>

* **title**で**TitleScene**に表示されるタイトル名を設定します。
* 今回は**Break Out**というタイトルにしました。

## 今回はここまで
以上で、ゲームを作成していくための下準備は整いました。
次回から、具体的なゲーム処理を書いていきたいと思います。