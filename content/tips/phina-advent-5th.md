+++
date = "2016-12-05T00:01:08+09:00"
draft = false
slug = ""
tags = ["tips", "canvas"]
title = "PlainElementでCanvas描画"
eyecatch = "/images/phina-advent-5th.png"
+++

## はじめに
この記事は、[phina.js Advent Calendar 2016](http://qiita.com/tamochu3141/items/8094aec3348dc9a6ae9d) の5日目の記事です。

* 4日目：[phina.jsでの多人数参加型ゲームの作製 by tamochu3141さん](http://qiita.com/tamochu3141/items/8094aec3348dc9a6ae9d)
* 6日目：[Shapeを画像として使う by simiraaaaさん](http://qiita.com/simiraaaa/items/ba1f1648a6d0451b5abc)

## phina.jsの描画機能
**phina.js**の描画機能は、**HTML5 Canvas**がベースになっています。**Canvas**をある程度知っている人は、**phina.js**を使ってみてピンと来るところがあると思います。加えて、**phina.js**にはデフォルトの**Canvas**の機能を拡張した[**phina.graphics.Canvas**](https://github.com/phi-jp/phina.js/blob/develop/src/graphics/canvas.js)というクラスが用意されており、デフォルトのメソッドに加え、簡単に線を描画できる**drawLine**など便利なメソッド群が追加で実装されています。

**phina.js**で**Canvas**のメソッドを使用したい場合は、**phina.graphics.Canvas**のインスタンスを作ってそこに描画する方法がありますが、多少手間がかかるところがありますので、今回は手軽に**Canvas**の機能が使える**PlainElement**というクラスを紹介したいと思います。

## PlainElementクラス
**phina.display.PlainElement**は、内部に**Canvas**クラスを持っているだけのシンプルなクラスで、**Shape**の親クラスとして位置付けられています。
内部に**Canvas**を持っているので、**Canvas**で使えるメソッドは一通り使うことができます。

## PlainElementクラスの使用例（簡単なお絵描き風プログラム）
お絵描きと呼べる程ではありませんが、今回は、マウスドラッグで線が引けるだけの簡単なサンプルを作ってみました。

![phina-advent-5th](/images/phina-advent-5th.png)

コードは以下のとおりです。

```js
phina.globalize();
// メインシーン
phina.define('MainScene', {
  superClass: 'DisplayScene',
  // コンストラクタ
  init: function() {
    // 親クラス初期化
    this.superInit();
    // 背景色
    this.backgroundColor = 'black';
    // canvas要素描画用
    var elem  = PlainElement({
      width: this.gridX.width,
      height: this.gridY.width, 
    }).addChildTo(this).setPosition(this.gridX.center(), this.gridY.center());
    // canvasパラメータ指定
    elem.canvas.context.strokeStyle = 'white';
    elem.canvas.context.lineWidth = 4;
    // 参照用
    this.elem = elem;
  },
  // タッチ開始
  onpointstart: function(e) {
    // タッチされた位置を記憶
    this.prev = Vector2(e.pointer.x, e.pointer.y);
  },
  // タッチ移動中
  onpointmove: function(e) {
    // 現在のタッチ位置を記憶
    var current = Vector2(e.pointer.x, e.pointer.y);
    // 前の位置から現在の位置まで線を引く
    this.elem.canvas.drawLine(this.prev.x, this.prev.y, current.x, current.y);
    // 前の位置を現在の位置に更新
    this.prev = current;
  },
  // タッチ終了
  onpointend: function() {
    // 位置情報をクリア
    this.prev = null;  
  },
});
// メイン
phina.main(function() {
  var app = GameApp({
    startLabel: 'main',
  });
  app.run();
});
```

<a href="http://runstant.com/alkn203/projects/783bf264" target="_blank">[runstantで確認]</a>

## コード説明
### PlainElementの追加

```js
    // canvas要素描画用
    var elem  = PlainElement({
      width: this.gridX.width,
      height: this.gridY.width, 
    }).addChildTo(this).setPosition(this.gridX.center(), this.gridY.center());
```

* **PlainElement**を作成して、**Scene**に追加しています。
* コンストラクでは、横と縦のサイズを指定します。今回は、画面と同じサイズにして重ねています。

### canvasのcontext指定

```js
// canvasパラメータ指定
elem.canvas.context.strokeStyle = 'white';
elem.canvas.context.lineWidth = 4;
```

* **PlainElement**内部の**Canvas**には、**canvas**プロパティでアクセスできます。
* ここでは**canvas**の**context**を経由して、線の色と幅を指定しています。

### タッチ開始時処理

```js
// タッチ開始
onpointstart: function(e) {
  // タッチされた位置を記憶
  this.prev = Vector2(e.pointer.x, e.pointer.y);
},
```
* イベントからマウスが押された位置を取得します。
* 線の始点の座標を**Vector2**クラスを使って記憶します。

### タッチ移動時処理

```js
// タッチ移動中
onpointmove: function(e) {
  // 現在のタッチ位置を記憶
  var current = Vector2(e.pointer.x, e.pointer.y);
  // 前の位置から現在の位置まで線を引く
  this.elem.canvas.drawLine(this.prev.x, this.prev.y, current.x, current.y);
  // 前の位置を更新
  this.prev = current;
},
```

* 現在のタッチ位置を線の終点として記憶します。
* **canvas**の**drawLine**メソッドで線を引きます。
* 最後の前の位置を現在の位置に更新します。

### タッチ終了時処理

```js
// タッチ終了
onpointend: function() {
  // 位置情報をクリア
  this.prev = null;  
},
```

* 線を引き終わった後は、始点の位置情報をクリアします。

## 補足

**phinajs**には他に**PathShape**というクラスがあります。線の描画に特化したクラスで、座標値データ群を与えるだけでその点を結んだ線を自動で描画してくれる優れものです。こちらはまた別の機会に紹介したいと思います。

## おわりに
**phina.js**でゲームを作るときは**Sprite**画像を使うことが多いかもしれませんが、上で説明した**PlainElement**、つまり、**Canvas**の機能を併用することで、よりゲームの表現性が増すのではないでしょうか。可能であれば、今回のアドベントカレンダーの期間内で簡単なサンプルゲームを作ってみたいと思います。
