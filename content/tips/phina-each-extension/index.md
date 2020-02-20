+++
date = "2019-12-31T20:55:06+09:00"
draft = false
slug = ""
tags = ["array"]
title = "【phina.js】Arrayクラスのeach関数を拡張する"
eyecatch = "each.png"
+++

## はじめに
**phina.js**には、通常の**for**文の代替で使える便利なメソッドがあります。
**Number**の拡張クラスのメソッド**times**が代表的な例です。

```javascript
  (10).times(function(i) {
    console.log('hello');
  });
```

- コールバック関数内に書かれた処理を10回繰り返します。
- 結果は hello が10回表示されます。

## コールバック関数内でthisを使う
コールバック関数内で**this**を使った場合、この**this**はコールバック関数自身を指すので、第2引数に明示的に**this**が指定できるようになっています。

```javascript
  this.count = 0;
  
  (10).times(function(i) {
    this.count += i;
  }, this);
```

## Arrayクラスのメソッドeach
**phina.js**の**Array**クラスには、**each**というメソッドがあり、配列の各要素に対して共通の処理を行うことができます。実体は**forEach**です。

```javascript
var spriteGroup = DisplayElement.addChildTo(this);
// 各スプライトの移動
spriteGroup.children.each(function(sprite) {
  sprite.x += 1;
});
```

## eachを拡張してthisが指定できるようにする
* 便利な**each**メソッドですが、現在の仕様では上で触れた**times**メソッドのように第２引数に**this**を代入することができません。
* **each**メソッドを以下のように拡張して、**this**が指定できるようにしてみました。

```javascript
Array.prototype.$method("each", function(fn, self) {
  self = self || this;
  for (var i=0; i<this.length; ++i) {
    fn.call(self, this[i], i, this);
  }
  return this;
});
```

[サンプルコード(runstant)](https://runstant.com/alkn203/projects/da45048c)

## まとめ
今回の拡張は、新しい**javascript**にあるアロー関数を使えば解決できることではありますが、参考までにこのような方法もあると知ってもらえば幸いです。