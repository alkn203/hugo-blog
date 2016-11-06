+++
date = "2016-01-01T23:59:06+09:00"
draft = false
slug = ""
tags = ["tutorial", "15puzzle"]
title = "【phina.js】ゲーム作成チュートリアル（15パズル）（その５）【tweenerで移動処理】"

+++

### 前回
前回の[【phina.js】ゲーム作成チュートリアル（15パズル）（その４）【ピースの移動】](http://alkn203.github.io/blog/2016/01/01/15puzzle-tut-04/)では、ピースをタッチで移動できるようにしました。

### 今回の目標
[phina.js](http://phinajs.com/)の目玉機能の1つである**tweener**を使ってピースの移動を滑らかにしてみます。
まずは以下で動きを確認してみてください。

<div class='runstant'><iframe src='http://goo.gl/y8M1QA' width='100%' height='640px' style='border:0px;box-shadow:0px 0px 2px 0px #aaa'></iframe></div>

前回までに比べて移動が滑らかになったかと思います。
[phina.js](http://phinajs.com/)では、このような動きを**tweener**という機能で実現できます。

### コード説明
**movePiece**関数を以下のように変更します。

```js
// ピースの移動処理
movePiece: function(piece) {
  // 空白ピースを得る
  var blank = this.getBlankPiece();
  // x, yの座標差の絶対値
  var dx = Math.abs(piece.x - blank.x);
  var dy = Math.abs(piece.y - blank.y);
  // 隣り合わせの判定
  if ((piece.x === blank.x && dy === GRID_SIZE) ||
      (piece.y === blank.y && dx === GRID_SIZE)) {
    // タッチされたピース位置を記憶
    var touchX = piece.x;
    var touchY = piece.y;
    // tweenerで移動処理
    piece.tweener.clear()
                 .to({x: blank.x, y: blank.y}, 200, "easeOutCubic")
                 .call(function() {
                   // 空白ピースをタッチされたピースの位置へ
                   blank.setPosition(touchX, touchY);
                 });
  }
},
```

tweenerの設定部分を説明します。
最初に**clear**で設定をリセットします。

```js
    piece.tweener.clear()

```

見やすいように改行していますが、次にさせたい処理をチェインメソッドで繋げます。

```js

                 .to({x: blank.x, y: blank.y}, 200, "easeOutCubic")

```

* **to**の{}内にパラメータを設定します。今回は指定の位置（空白ピース）へ移動と設定しています。その他、拡大縮小、回転といったパラメータが同時に設定可能です。

* 次の**200**という数字は、{}内の処理をどれくらいの時間をかけて行うかの設定です。単位は**ミリ秒**（1000分の1秒）です。

* 最後の**easeOutCubic**は、イージングといって、動きにエフェクトをかけることができます。**tweener**の使い方については、[simiraaaa](https://twitter.com/simiraaaa)さんの記事[[phina.js] Tweenerを使いこなそう! [Tweener 基本編]](http://qiita.com/simiraaaa/items/b80fec588fa2db272f67)がとても参考になります。

**call**を使うことで、以下のように通常の処理を繋げて**非同期で**実行させることができます。

```js
.call(function() {
  // 空白ピースをタッチされたピースの位置へ
  blank.setPosition(touchX, touchY);
});

```

以上をまとめると、**タッチされたピースを空白ピースの場所に移動させた後、空白ピースをタッチされたピースの元位置に移動（入れ替え）**という一連の処理になります。書き方に多少慣れが必要ですが、より複雑な動きも**tweener**なら手軽に設定することができます。

### 今回はここまで
ここまでで、**tweener**を使ってピースが滑らかに移動できるようになりました。
次回は**ゲーム的要素の追加**で、よりゲームらしくしていきます。
