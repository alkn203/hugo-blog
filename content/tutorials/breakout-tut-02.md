+++
date = "2015-10-29T20:50:06+09:00"
draft = false
slug = ""
tags = ["tutorial", "breakout"]
title = "【phina.js】ゲーム作成チュートリアル（ブロック崩し）第2回＝ブロックの配置＝"
eyecatch = "/images/breakout-tut-02.png"
+++

## 目的

[前回](http://alkn203.github.io/blog/2015/10/26/beakout-tut-01/)は、[phina.js](https://github.com/phi-jp/phina.js)のテンプレートからゲーム作成の下準備をしました。今回からゲーム内容のコーディングをしていきます。
まず最初に以下のように**ブロック**を配置したいと思います。

<center>![breakout-tut-02](/images/breakout-tut-02.png)</center>

## 定数とBlockクラスを定義する
ゲーム内で使用する定数は通常の**var**で、**Block**クラスは**phina.define**を使って定義します。コードは以下のとおりです。

```js
phina.globalize();
/*
 * 定数
 */
var BLOCK_WIDTH = 40 * 2;
var BLOCK_HEIGHT = 60 / 2;
/*
 * メインシーン
 */
phina.define('MainScene', {
  superClass: 'DisplayScene',
  // コンストラクタ
  init: function() {
    // 親クラス初期化
    this.superInit();
    // 背景色
    this.backgroundColor = 'black';
  },
});
/*
 * ブロッククラス
 */
phina.define('Block', {
  // 親クラス指定
  superClass: 'RectangleShape',
  // コンストラクタ
  init: function() {
    // 親クラス初期化
    this.superInit({
      width: BLOCK_WIDTH,
      height: BLOCK_HEIGHT,
    });
  },
});
/*
 * メイン処理
 */
phina.main(function() {
  // アプリケーションを生成
  var app = GameApp({
    title: 'Break Out',
  });
  // 実行
  app.run();
});
```

## コード説明
### 定数

```js
var BLOCK_WIDTH = 40 * 2;
var BLOCK_HEIGHT = 60 / 2;
```
* **BLOCK_WIDTH**はブロックの横幅、**BLOCK_HEIGHT**はブロックの縦幅です。
* 40 や 60という数値を基準にしているのは、**Scene**にデフォルトで設定されている**Grid**の存在を意識しているためです。**Grid**については後述します。

### ブロッククラス

```js
phina.define('Block', {
  // 親クラス指定
  superClass: 'RectangleShape',
  // コンストラクタ
  init: function() {
    // 親クラス初期化
    this.superInit({
      width: BLOCK_WIDTH,
      height: BLOCK_HEIGHT,
    });
  },
});
```
* **phina.define**で**Block**クラスを定義しています。
* ブロックのグラフィックは、画像ファイルを使わずに**phina.js**で用意されている基本図形の**Shape**を活用します。
* 今回はその中の１つである**RectangleShape**（矩形）クラスを継承しています。
* **this.superInit**で、親クラス(**RectangleShape**)の**width**と**height**プロパティに、定数で定義したブロックのサイズをそれぞれ渡しています。

## ブロックを配置する

オーソドックスなブロック崩しの場合、ブロックはタイル状に並んで配置されています。そこで、**phina.js**で用意されている**Grid**クラスを利用してタイル状に並べたいと思います。
まずはコードをご覧下さい。

```js
// グローバルに展開
phina.globalize();
/*
 * 定数
 */
var BLOCK_WIDTH = 40 * 2;
var BLOCK_HEIGHT = 60 / 2;
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
    // 背景色
    this.backgroundColor = 'black';
    // ブロックグループ
    this.blockGroup = DisplayElement().addChildTo(this);

    var self = this;
    // Gridを利用してブロック設置
    Array.range(2, 16, 2).each(function(spanX) {
      Array.range(1, 4, 0.5).each(function(spanY) {
        Block().addChildTo(self.blockGroup)
               .setPosition(self.gridX.span(spanX), self.gridY.span(spanY));
      });
    });
  },
});
/*
 * ブロッククラス
 */
phina.define('Block', {
  // 親クラス指定
  superClass: 'RectangleShape',
  // コンストラクタ
  init: function() {
    // 親クラス初期化
    this.superInit({
      width: BLOCK_WIDTH,
      height: BLOCK_HEIGHT,
    });
  },
});
/*
 * メイン処理
 */
phina.main(function() {
  // アプリケーションを生成
  var app = GameApp({
    title: 'Break Out',
  });
  // 実行
  app.run();
});
```
<a href="http://runstant.com/alkn203/projects/8308f430" target="_blank">[runstantで確認]</a>

## コード説明
### ブロックグループの作成

```js
this.blockGroup = DisplayElement().addChildTo(this);
```
* 作成するブロックは**GameScene**に直接追加しても良いのですが、利便性を考えて、ブロック管理用のグループを作成します。
* グループは**DisplayElement**クラスで作成し、**addChildTo(this)**で**GameScene**に追加しています。
* 個々のブロックは、後のループ処理でグループに追加するようにします。

### Gridを使った配置：
ループ使って、ブロックをタイル状に配置する処理です。

```js
    var self = this;
    // Gridを利用してブロック設置
    Array.range(2, 16, 2).each(function(spanX) {
      Array.range(1, 4, 0.5).each(function(spanY) {
        Block().addChildTo(self.blockGroup)
               .setPosition(self.gridX.span(spanX), self.gridY.span(spanY));
      });
    });
```

まずループ処理について見てみましょう。

```js
Array.range(2, 16, 2).each(function(spanX) {
```

**phina.js**では、元々の**javascript**の**Array**に様々な拡張が施されています。
**range**はその中の１つで、**範囲指定した数値の配列**を新たに作ってくれます。**Python**の**range**と同じ仕様です。

```js
Array.range(開始、終了、ステップ数)
```

今回の場合は、**[2,4,6,8,10,12,14]**という配列を作成したことになります。
次の**each**は、基本的に**javascript**の**forEach**と同じで、引数で与えられたコールバック関数内の処理を配列の各要素に適用します。
まとめると、**[2,4,6,8,10,12,14]**という配列の各々の要素に対して、**function**内の処理を適用するということになります。各要素は、引数の**spanX**で参照することができます。
同様に、以下で縦方向の繰り返しを行っています。

```js
Array.range(1, 4, 0.5).each(function(spanY) {
```

ループ処理について、今回は**phina.js**らしい書き方にしてみましたが、もちろん通常の**for**文を使っても全く問題ありません。

最後に、個々のブロックを作成して追加する処理について説明します。

```js
        Block().addChildTo(self.blockGroup)
               .setPosition(self.gridX.span(spanX), self.gridY.span(spanY));
```

* **Block**クラスのオブジェクトを作成して、前もって作っておいた**blockGroup**に追加しています。
* **this.blockGroup**と書きたいところですが、**function**内の**this**と**GameScene**内での**this**は参照先が異なるため、そのままだとエラーになってしまいます。
* そこで、ループ処理に入る前に

```js
var self = this;
```
と書いて、**self**が正しい参照(**GameScene**)となるようにしています。

* 次の**setPosition**で、ブロックの位置を決めています。
* オブジェクトの作成から配置まで、**jquery**のチェインメソッドのように繋げています。**phina.js**では、大半がチェインメソッドに対応していますので、スッキリ書けて便利です。
* **gridX.span(spanX)**は、横方向の**Grid**で**spanX**グリッド目の位置を表します。**gridY.span(spanY)**は縦方向です。
* **Grid**を活用して縦と横方向にループ処理することで、このようにブロックをタイル状に配置することができます。

## Gridについて
**phina.js**では、**Scene**にデフォルトで**Grid**というものが設定されています。
**Grid**は、**Scene**(スクリーン)を格子状に分けた情報を持っており、横方向、縦方向にそれぞれ16分割されています。
横方向のグリッドは**this.gridX**、縦方向は**this.gridY**でアクセスできます。
オブジェクトの配置の際に**Grid**はとても便利な機能ですので、別の機会に掘り下げて説明したいと思います。

## 今回はここまで
説明が長くなってしまいましたが、ここまでで、ブロックを配置することができました。しかし、ゲームとしての動きが何もないため面白くないですね。
次回は、プレイヤーが操作するパドルを作成して、徐々にゲームらしくしていきたいと思います。

---
*2015/10/30 修正*
*本文中でブロックとBrickが混在していたため、BrickクラスからBlockクラスに変更しました。*