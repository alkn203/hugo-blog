+++
date = "2015-11-02T20:55:06+09:00"
draft = false
slug = ""
tags = ["tutorial", "breakout"]
title = "【phina.js】ゲーム作成チュートリアル（ブロック崩し）第5回＝ボールとブロックの反射処理＝"

+++

## はじめに

前回は、ボールと画面端、ボールとパドルの反射処理を実装しました。
今回は、ボールとブロックの反射処理、そしてブロックの消去処理を追加したいと思います。

## ボールとブロックの反射処理

ボールがブロックに当たると反射するようにします。まず、反射のパターンを整理したいと思います。
そこで、ボールとブロックとの当たり判定を以下のパターンに分けてみました。

* **ブロックの左上角** ・・・左上に跳ね返す
* **ブロックの右上角** ・・・右上に跳ね返す
* **ブロックの左下角** ・・・左下に跳ね返す
* **ブロックの右下角** ・・・右下に跳ね返す
* **ブロックの上面と下面** ・・・縦移動を反転させる
* **ブロックの左側面と右側面** ・・・横移動を反転させる

<img src="http://alkn203.github.io/blog/img/balltoblock.png" />

コードは以下のとおりです。

<div class='runstant'><iframe src='http://goo.gl/tCHV5X' width='100%' height='640px' style='border:0px;box-shadow:0px 0px 2px 0px #aaa'></iframe></div>

## コード説明
当たり判定はボールの移動中常時行う必要があるので、**update**関数内に追加します。

### ブロックとの反射

```js
// ブロックとの反射
this.blockGroup.children.some(function(block) {
  // ヒットなら
  if (ball.hitTestElement(block)) {
```

**blockGroup**の子要素配列をループして、ボールとの当たり判定を行います。
**each**を使いたいところですが、現状**forEach**には**break**に当たる処理がないとのことですので、打開策として**some**関数を使って、当たりと判定されたらループを抜けるようにしています。
この手法については、[本記事](http://qiita.com/phi/items/f998763d2d52bdd57a7c)を参考にしています。

### 角のあたり判定

角の当たり判定については、左上角を例に説明します。

```js
// 左上かど
if (ball.top < block.top && ball.left < block.left) {
  // 位置補正
  ball.right = block.left;
  ball.bottom = block.top;
  // 移動方向設定
  ball.vx = -ball.speed;
  ball.vy = -ball.speed;
  return true;
}
```

左上角の場合は、ボールがブロックに当たっていて、**ボールの左側がブロックの左側より左、かつボールの上側がブロックの上側より上**という位置関係にある時です。
<img src="http://alkn203.github.io/blog/img/lefttop.png" />
画面上で**vx**は負、**vy**も負の方向、つまり左斜め上45度方向に返します。

### 側面との当たり判定

```js
// 左側
if (ball.left < block.left) {
  ball.right = block.left;
  ball.vx = -ball.vx;
  return true;
}
// 右側
if (block.right < ball.right) {
  ball.left = block.right;
  ball.vx = -ball.vx;
  return true;
}
```
<img src="http://alkn203.github.io/blog/img/left.png" />
側面の場合は、**vy**はそのままで**vx**を反転させます。

### 上下面との当たり判定

```js
// 上側
if (ball.top < block.top) {
  ball.bottom = block.top;
  ball.vy = -ball.vy;
  return true;
}
// 下側
if (block.bottom < ball.bottom) {
  ball.top = block.bottom;
  ball.vy = -ball.vy;
  return true;
}
```
<img src="http://alkn203.github.io/blog/img/top.png" />
上下面の場合は、**vx**はそのままで**vy**を反転させます。

## ボールが当たったブロックを消去する

次はブロックの消去処理を追加します。コードは以下のとおりです。

<div class='runstant'><iframe src='http://goo.gl/xF7tNc' width='100%' height='640px' style='border:0px;box-shadow:0px 0px 2px 0px #aaa'></iframe></div>

## コード説明

### 消去処理

```js
// ブロック消去
block.remove();
```

当たり判定で当たったブロックについて、**block.remove**として自身を消去しています。
この処理を各判定毎に行います。

### 補足
お気づきの方もいらっしゃるかもしれませんが、このままの処理だと当たり具合によって内部のブロックまで消去されることがあります。
これを防ぐためには、当たったブロックが一番外側にあるかどうかの判定を追加する必要があるかと思いますが、今回は割愛したいと思います。

## 今回はここまで

ここまでで、ボールを跳ね返してブロックが消去できるようになりました。
少しはブロック崩しらしくなってきたのではないかと思います。
次回は、**phina.js**の目玉機能の１つとも言える**Tweener**を使って、ブロックの消去アニメーションを行いと思います。
