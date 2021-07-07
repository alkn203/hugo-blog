+++
date = "2021-06-24T22:59:06+09:00"
draft = false
slug = ""
tags = ["number"]
title = "phina.jsの拡張クラスを使ってみよう　ーString編ー"
+++

## はじめに
今回は、**String** の拡張クラスを紹介したいと思います。

## 拡張Stringクラス
**phina.js** では、元々の **javascript** の**String**クラスに様々な拡張メソッドが追加されています。
以下に、使う機会が多いと思われるメソッドを紹介します。

### format
定義したフォーマットに引数を適用した文字列を返します。

```js
// 引数がオブジェクトの場合
var obj = {r: 128, g: 0, b: 255};
"color: rgb({r}, {g}, {b});".format(obj); // => "color: rgb(128, 0, 255);"
// 引数がオブジェクトでない場合
var count = 100;
"time: {0} / {1}".format(count, 1000); // => "time: 100 / 1000"
```

* 文字列の連結（+）を使用しなくても良いので、より直感的に書くことができます。
* 引数がオブジェクトでない場合、引数の数に応じて、{0}、{1}、{2}・・・のように定義した文字列内で参照することができます。

### trim
文字列両サイドの空白文字を全て取り除いた文字列を返します。

```js
"  Hello, world!  ".trim(); // => "Hello, world!"
```

### capitalize
すべての単語の先頭を大文字にした文字列を返します。単語の先頭以外は小文字化されます。

```js
"i aM a pen.".capitalize(); // => "I Am A Pen."
```

### capitalizeFirstLetter
先頭の文字を大文字にして、それ以外を小文字にした文字列を返します。

```js
"i aM a pen.".capitalizeFirstLetter(); // => "I am a pen."
```

* 上の３つはノベルゲームなどで使えるかもしれません。

### padding
左に文字を埋めて指定した桁にした文字列を返します。

```js
"1234".padding(10);      // => "      1234"
"1234".padding(10, '0'); // => "0000001234"
```

拡張**Number**クラスにもある**padding**と同等の処理を行います。

### paddingRight
右に文字を埋めて指定した桁にした文字列を返します。

```js
"1234".paddingRight(10);      // => "1234      "
"1234".paddingRight(10, '0'); // => "1234000000"
```

**paddding**の逆パターンで、右に文字を詰めます。

### repeat
自分自身を指定した回数だけ繰り返した文字列を返します。

```js
"Abc".repeat(4); // => "AbcAbcAbcAbc"
```

### count
指定した文字列が何個入っているかをカウントして返します。

```js
"This is a string. Isn't it?".count("is"); // => 2
```

### include
指定した文字列が含まれているかどうかを返します。

```js
"This is a string.".include("is"); // => true
"This is a string.".include("was"); // => false
```

### each
各文字を順番に渡しながら関数を繰り返し実行します。

```js
str = 'abc';
str.each(function(ch) {
  console.log(ch);
});
// => 'a'
//    'b'
//    'c'
```

### toArray
1文字ずつ分解した配列を返します。

```js
"12345".toArray(); // => ["1", "2", "3", "4", "5"]
"あいうえお".toArray(); // => "あ", "い", "う", "え", "お"]
```

他にも詳しく見たいという方は、以下をご確認下さい。
> Stringクラスのソース
> https://github.com/phinajs/phina.js/blob/develop/src/core/string.js

## 使い方
htmlファイルで **phina.js** を以下のように読み込みます。

```html
<script src="https://cdn.jsdelivr.net/gh/phinajs/phina.js@v0.2.3/build/phina.js"></script>
```

## 実行サンプル
https://runstant.com/alkn203/projects/8cde885d

## さいごに
* 文字列操作もコーディングにおいては欠かせないものですので、これらのメソッドが活躍できる場面があるかと思います。
* 次回は、**Math** クラスを紹介したいと思います。