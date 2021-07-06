+++
date = "2021-06-24T20:59:06+09:00"
draft = false
slug = ""
tags = ["array"]
title = "phina.jsの拡張クラスを使ってみよう　ーArray編ー"
+++

## はじめに
[phina.js](https://phinajs.com/)は、国産の **javascript** ゲームライブラリです。
ゲーム製作向けのライブラリですので、ゲーム製作を前提とした機能が充実していますが、**javascript** でのコーディングをより快適にするための細かな配慮がされているライブラリでもあります。その例として、今回は、**javascript** の基本クラスである **Array** の拡張メソッドを紹介したいと思います。

## 拡張Arrayクラス
**phina.js** では、元々の **javascript** の**Array**クラスに様々な拡張メソッドが追加されています。
以下に、使う機会が多いだろうと思われるメソッドを紹介します。

### first
配列の最初の要素を返します。

### last
配列の最後の要素を返します。

```js
var arr = [1,2,3];
arr.first; // => 1
arr.last; // => 3
```

* これらは、メソッドというよりもプロパティです。　　
* インデックス番号を意識しなくて良いので、参照エラーなどを回避することができます。

### contains
指定された要素が配列に含まれているかどうかを調べます。

```js
var arr = [1,2,3];
arr.contains(1); // => true
```

### swap
指定されたインデックスの要素を入れ替えます。

```js
var arr = ['a','b','c'];
arr.swap(0,1); // => arr = ['b','a','c']
```

* 前後の要素を入れ替える時などに便利です。

### eraseIf
引数で与えられた関数の条件に一致する最初の要素を削除します。

```js
var arr = [1,2,3];
arr.eraseIf(function(elem) {
  if (elem > 1) {
    return true;
  } 
}); //=> arr = [1,3]
```

* 関数の中に任意の条件を設定できるので、用途の幅が広がります。

### eraseIfAll
引数で与えられた関数の条件に一致する要素を全て削除します。

```js
var arr = [1,2,3];
arr.eraseIfAll(function(elem) {
  if (elem > 1) {
    return true;
  }
}); // => arr = [1]
```

* 通常このような処理をするためには、配列の要素を逆順でループで回す必要がありますが、条件だけを気にすれば良いので楽です。

### random
配列からランダムな要素を１つ返します。

```js
var arr [1,2,3];
arr.random(); // => random value in 1,2,3
```

* ランダム要素を加えるために、ゲーム開発で多用される処理に使えます。

### uniq
配列から重複した値を除いた新しい配列を返します。

```js
var arr = [1,2,2,3,4,4,5];
arr.uniq(); // => [1,2,3,4,5]
```

### clear
配列の要素を全て削除します。

```js
var arr = [1,2,3];
arr.clear(); // => arr = []
```

### fill
要素を指定した値で埋めます。

```js
var arr = [1,2,3];
arr.fill(0); // => arr = [0,0,0]
```

### range
一定間隔の整数の列としての配列を返します。

```js
var arr = [];
arr.range(4); // => arr = [0,1,2,3]
arr.range(2,5); // => arr = [2,3,4]
arr.range(2,10,2); // => arr = [2,4,6,8]
```

### shuffle
配列の要素をシャッフルします。

```js
var arr = [1,2,3,4,5];
arr.shuffle(); // => [2,3,5,4,1] など
```

他にも、例えば以下のようなメソッドが用意されています。
詳しく見たいという方は、以下をご確認下さい。
> Arrayクラスのソース
> https://github.com/phinajs/phina.js/blob/develop/src/core/array.js 

### equals
配列が等しいかどうかをチェックします。

### at
指定したインデックスの要素を返します。

### erase
指定したオブジェクトと一致した最初の要素を削除します。

### clone
自身のコピーを生成して返します。

### sum
要素の合計値を返します。

### average
要素の平均値を返します。

## 使い方
htmlファイルで **phina.js** を以下のように読み込みます。

```html
<script src="https://cdn.jsdelivr.net/gh/phinajs/phina.js@v0.2.3/build/phina.js"></script>
```

## 実行サンプル
https://runstant.com/alkn203/projects/830fd098

## さいごに
* 配列操作は、ゲームプログラミングに限らず、かなり使用頻度が高い処理です。それ故に、今回紹介したような機能を活用して、可能な限りコーディングを快適にしたいものです。
* **phina.js** では、他にも **Number** や **String** といったコアなクラスに拡張がされていますので、次はその辺りを紹介したいと思います。