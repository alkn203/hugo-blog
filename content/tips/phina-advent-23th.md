+++
date = "2016-12-23T00:01:08+09:00"
draft = false
slug = ""
tags = ["tips", "class"]
title = "クラス継承をしてみよう"
eyecatch = "/images/phina-advent-23th.png"
+++

## はじめに
この記事は、[phina.js Advent Calendar 2016](http://qiita.com/advent-calendar/2016/phinajs) の23日目の記事です。

* 22日目：[Three.jsとPhina.js使った何か by kjunichiさん](http://qiita.com/advent-calendar/2016/phinajs)
* 24日目：[phina.jsとGLBoostの連携について by emadurandalさん](http://qiita.com/emadurandal/items/63e262df75b279dda253)

## クラス継承
**phina.js**を使い慣れてくると、**既存のクラスに機能を追加**したいと思うことがあります。もちろんイチから作るのもありですが、ちょっとした機能追加であれば既存のクラスを継承したクラスを作った方が効率的です。今回は**Button**クラスを例にその方法を説明します。

## AnimateButtonクラスを作る
現在の**Button**クラスの仕様はシンプルで、最低限の機能しかありません。今回はこの**Button**クラスを継承して**AnimateButton**というクラスを新しく作りたいと思います。追加する仕様は単純で、**「ボタンを押している間は縮小して押しているように見せる」**です。（下のボタンは画像なので押せません）

<center>![phina-advent-23th](/images/phina-advent-23th.png)</center>

### 手順１ Buttonクラスのソースをコピーする
本来であれば、**ローカル環境にソースをダウンロード→エディタで編集→ビルド→テスト**というのが手順かもしれませんが、今回は簡単な継承例ということでソースを丸ごとコピーして、**runstant**上で作業します。コピー場所はコードの冒頭にします。(phina.globalize()よりも前)

以下が**Button**クラスのソースです。

```js
/**
 * @class phina.ui.Button
 * Button
 */
phina.define('phina.ui.Button', {
  superClass: 'phina.display.Shape',
  /**
   * @constructor
   */
  init: function(options) {
    options = (options || {}).$safe(phina.ui.Button.defaults);
    this.superInit(options);

    this.cornerRadius = options.cornerRadius;
    this.text         = options.text;
    this.fontColor    = options.fontColor;
    this.fontSize     = options.fontSize;
    this.fontWeight     = options.fontWeight;
    this.fontFamily   = options.fontFamily;

    this.setInteractive(true);
    this.on('pointend', function() {
      this.flare('push');
    });
  },
  prerender: function(canvas) {
    canvas.roundRect(-this.width/2, -this.height/2, this.width, this.height, this.cornerRadius);
  },

  postrender: function(canvas) {
    var context = canvas.context;
    // text
    var font = "{fontWeight} {fontSize}px {fontFamily}".format(this);
    context.font = font;
    context.textAlign = 'center';
    context.textBaseline = 'middle';
    context.fillStyle = this.fontColor;
    context.fillText(this.text, 0, 0);
  },

  _static: {
    defaults: {
      width: 200,
      height: 80,
      backgroundColor: 'transparent',
      fill: 'hsl(200, 80%, 60%)',
      stroke: null,

      cornerRadius: 8,
      text: 'Hello',
      fontColor: 'white',
      fontSize: 32,
      fontWeight: '',
      fontFamily: "'HiraKakuProN-W3'", // Hiragino or Helvetica,
    },
  },

  _defined: function() {
    phina.display.Shape.watchRenderProperty.call(this, 'cornerRadius');
    phina.display.Shape.watchRenderProperty.call(this, 'text');
    phina.display.Shape.watchRenderProperty.call(this, 'fontColor');
    phina.display.Shape.watchRenderProperty.call(this, 'fontSize');
    phina.display.Shape.watchRenderProperty.call(this, 'fontFamily');
  },

});

```

### 手順２ クラス名を変更し、不要な部分を削除する

クラス名を変更し、更に**phina.ui.Button**を継承するように変更します。

```js
/**
   * @class phina.ui.AnimateButton
   * AnimateButton
   */
  phina.define('phina.ui.AnimateButton', {
    superClass: 'phina.ui.Button',
```

見た目や基本動作は**Button**クラスのものを使うので基底クラスに関する部分は削除し、以下のようにスッキリさせます。

```js
/**
   * @class phina.ui.AnimateButton
   * AnimateButton
   */
  phina.define('phina.ui.AnimateButton', {
    superClass: 'phina.ui.Button',
    /**
     * @constructor
     */
    init: function(options) {
      options = (options || {}).$safe(phina.ui.Button.defaults);
      this.superInit(options);
    },
    
    _static: {
      defaults: {
      },
    },
  });
```

### 手順３　押している間の見た目を変更する

押している間は少し縮小させたいので、**pointstay**イベント内に処理を書きます。
**scaleMin**は後のプロパティ設定で説明します。

```js
// プッシュ時の処理を追加
this.on('pointstay', function() {
  // 少し縮小
  this.setScale(scaleMin, scaleMin);
});
```

押し終わった後は元のサイズに戻さないといけないので、**pointend**イベント内に処理を書きます。
**1.0**が元のサイズです。

```js
this.on('pointend', function() {
  // 元に戻す
  this.setScale(1.0, 1.0);
});
```

### オプション　クラス特有プロパティを設定できるようにする

折角ですので、**AnimateButton**クラス特有のプロパティをコンストラクタで指定できるようにします。
今回は、縮小サイズを指定できるようにしたいと思います。

```js
init: function(options) {
  options = (options || {}).$safe(phina.ui.AnimateButton.defaults);
  this.superInit(options);
  // 独自プロパティ
  var scaleMin = options.scaleMin;
```

**$safe**はプロパティ（連想配列）をマージする関数で、**options**で与えられた値とデフォルトで定義されている値を結合します。
プロパティ名がバッティングした場合、**options**で与えられた値をデフォルトの値で上書きすることはありません。

独自プロパティの値は、**options**からもらいます。

```js
_static: {
  defaults: {
    scaleMin: 0.95,
  },
},
```

独自プロパティのデフォルト値は、上のように書きます。これで、コンストラクタで値を渡して指定できるようになります。

## 完成コード

```js
/**
 * @class phina.ui.AnimateButton
 * AnimateButton
 */
phina.define('phina.ui.AnimateButton', {
  superClass: 'phina.ui.Button',
  /**
   * @constructor
   */
  init: function(options) {
    options = (options || {}).$safe(phina.ui.AnimateButton.defaults);
    this.superInit(options);
    // 独自プロパティ
    var scaleMin = options.scaleMin;
    // プッシュ時の処理を追加
    this.on('pointstay', function() {
      // 少し縮小
      this.setScale(scaleMin, scaleMin);
    });
    this.on('pointend', function() {
      // 元に戻す
      this.setScale(1.0, 1.0);
    });
  },
  
  _static: {
    defaults: {
      scaleMin: 0.95,
    },
  },
});
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
  
  var txt = 'Push me';
  // デフォルトのボタン
  Button({text: txt}).addChildTo(this)
          .setPosition(this.gridX.center(), this.gridY.center(-2))
          .onpush = function() {
            console.log('Default');  
          };
  // 拡張ボタン
  AnimateButton({text: txt}).addChildTo(this)
                 .setPosition(this.gridX.center(), this.gridY.center(2))
                 .onpush = function() {
                   console.log('Animate');
                 };
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
// 実行
app.run();
});
```

<a href="http://runstant.com/alkn203/projects/9c1f8097" target="_blank">[runstantで確認]</a>

## おわりに
説明をだいぶ端折ってしまいましたが、**phina.js**でのクラス継承についての説明は以上です。このように拡張したクラスは自分専用に使っても良いですし、他のユーザーにも使ってもらいたいということであれば、[Github](https://github.com/phi-jp/phina.js)でプルリクエストをしても良いでしょう。**phina.js**への理解も深まりますので、お勧めです。