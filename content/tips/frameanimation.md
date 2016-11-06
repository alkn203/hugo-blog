+++
date = "2015-12-20T23:59:06+09:00"
draft = false
slug = ""
tags = ["tips", "frameanimation", "sprite"]
title = "フレームアニメーションのスプライトシートをコード内で定義する方法"

+++

### はじめに
現在、Qiitaで[phina.js Advent Calendar 2015](http://qiita.com/advent-calendar/2015/phinajs)が盛り上がってます。（私も微力ながら3つ登録しました。）
そこで、個人的に側面からも盛り上げるべく、[phina.js](http://phinajs.com/)に関するTipsなどをこの時期に合わせて紹介したいと思います。

### 今回のネタ
今回は、画像を使ったゲーム作りには欠かせないフレームアニメーションの小ネタを紹介します。
フレームアニメーションについては、以下のエントリーがとても参考になります。

[[phina.js]SpriteSheetを使ってみよう！](http://qiita.com/minimo/items/0aad94d94478845d3ce2) by [minimo](https://twitter.com/minimo)氏

上の記事に書いてあるように、フレームアニメーションで使用するスプライトシートは、基本的には外部のファイルに**JSON**形式で定義して、**phina.js**から**ASSET**として読み込みます。これは、複数のキャラ毎にフレームアニメーションを設定する際には必須と言えます。

一方、**phina.js**のソースを読むと分かりますが、スプライトシートの定義は**js**のコード内に直接書くこともできます。
方法は、以下のように**ASSET**のスプライトシートの該当箇所に**JSON**ファイルの中身を書くだけです。
この手法は、取り急ぎ１キャラずつフレームアニメーションの確認や調整をしたい場合に便利かと思います。また、**js**のコード内なので、コメントを付すこともできます。

```js
var ASSETS = {
  image: {
    'explosion': 'https://rawgit.com/alkn203/radar_touch/gh-pages/assets/explosion.png',
  },
  spritesheet: {
    'explosion_ss':
    {
      "frame": {
        "width": 80,
        "height": 80,
        "cols": 8,
        "rows": 1,
      },
      // アニメーション
      "animations" : {
        "start": {
          "frames": [0,1,2,3,4,5,6,7],
          "next": "end",
          "frequency": 2,
        },
        "end": {
          "frames": [5,6,7],
          "next": "end",
          "frequency": 5,
        }
      }
    },
  }
};
```
### 動作サンプル

<div class='runstant'><iframe src='http://goo.gl/c8NG6s' width='100%' height='640px' style='border:0px;box-shadow:0px 0px 2px 0px #aaa'></iframe></div>

[[runstantで開く]](http://goo.gl/c8NG6s)

### まとめ
今回は、フレームアニメーションの小ネタについて書かせて頂きました。使い勝手が良いかは微妙なところですが、こういう手法もあるということで参考程度に受け止めて頂ければ幸いです。
