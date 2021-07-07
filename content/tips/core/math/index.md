+++
date = "2021-06-24T23:59:06+09:00"
draft = false
slug = ""
tags = ["math"]
title = "phina.jsの拡張クラスを使ってみよう　ーMath編ー"
+++

## はじめに
今回は、**Math** の拡張クラスを紹介したいと思います。

## 拡張Mathクラス
**phina.js** では、元々の **javascript** の**Math**クラスに様々な拡張メソッドが追加されています。
以下に、使う機会が多いと思われるメソッドを紹介します。

### degToRad
度をラジアンに変換します。

```js
Math.degToRad(180); // => 3.141592653589793
```

* sinなどの三角関数ではラジアンを使うので、角度をラジアンに変換するこのメソッドは使用頻度が高いです。

### radToDeg
ラジアンを度に変換します。

```js
Math.radToDeg(Math.PI/4); // => 45
```

* **Sprite** などの回転を表すプロパティである **rotation** は度で指定するので、このメソッドが活躍します。

### clamp
指定した値を指定した範囲に収めた結果を返します。

```js
Math.clamp(120, 0, 640); // => 120
Math.clamp(980, 0, 640); // => 640
Math.clamp(-80, 0, 640); // => 0
```

* 前に紹介した拡張**Number**クラスにも同様のメソッドがあります。

### inside
指定した値が指定した値の範囲にあるかどうかを返します。

```js
Math.inside(980, 0, 640); // => false
Math.inside(120, 0, 640); // => true
```

### randint
指定された範囲内でランダムな整数値を生成します。

```js
Math.randint(-4, 4); // => -4、0、3、4 など
```

* ゲームを面白くするために、ランダム要素は必須と言えるので使用頻度が高いです。

### randfloat
指定された範囲内でランダムな数値を生成します。

```js
Math.randfloat(-4, 4); // => -2.7489193824000937 など
```

* 少数バージョンです。

### randbool
ランダムに真偽値を生成します。引数で百分率を指定する事もできます。

```js
Math.randbool();   // => true または false
Math.randbool(80); // => 80% の確率で true
```

## 使い方
htmlファイルで **phina.js** を以下のように読み込みます。

```html
<script src="https://cdn.jsdelivr.net/gh/phinajs/phina.js@v0.2.3/build/phina.js"></script>
```

## 実行サンプル
https://runstant.com/alkn203/projects/37136936

## さいごに
コアクラスの中でも**Math**クラスは、ゲーム開発に関連する実用的なメソッドが多いと言えるでしょう。