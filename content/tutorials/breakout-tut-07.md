+++
date = "2015-11-06T20:55:06+09:00"
draft = false
slug = ""
tags = ["tutorial", "breakout"]
title = "【phina.js】ゲーム作成チュートリアル（ブロック崩し）最終回＝ゲームオーバーとクリア処理＝"

+++

### はじめに
前回は、**Tweener**を使ったアニメーション処理を追加しました。
今回は最終回として、ゲームオーバーとクリア処理を追加したいと思います。

### ゲームオーバー処理

ボールが画面下に落下したら、**GAME OVER**という文字を表示してタイトル画面に戻るようにします。
コードは以下のとおりです。

<div class='runstant'><iframe src='http://goo.gl/Gn8EzI' width='100%' height='640px' style='border:0px;box-shadow:0px 0px 2px 0px #aaa'></iframe></div>

### コード説明

##### 93から111行目

```js
      // 落下
      var self = this;

      if (ball.top > screenRect.bottom) {
        // ゲームオーバー表示
        var label = Label({
          text: 'GAME OVER',
          fill: 'yellow',
          fontSize: 64,
        }).addChildTo(this);
        label.setPosition(this.gridX.center(), this.gridY.center());
        // 少し待ってからタイトル画面へ
        label.tweener.clear()
                     .wait(2000)
                     .call(function() {
                       self.nextLabel = 'title';
                       self.exit();
                     });
      }
```

* **Label**クラスを使って表示用のラベルを追加しています。
* **tweener**を使って、**wait**で2秒ほど待ってからタイトル画面に移動するようにしています。
* **nextLabel**で移動先のシーン名を指定します。
* **exit**で現在のシーンを抜けます。

### クリア処理

全てのブロックを崩したらクリアとして、スコアを結果画面に表示してみます。
コードは以下のとおりです。

<div class='runstant'><iframe src='http://goo.gl/15M7pz' width='100%' height='640px' style='border:0px;box-shadow:0px 0px 2px 0px #aaa'></iframe></div>

### コード説明

##### 53から63行目

```js
// スコア
this.score = 0;
var scoreLabel = Label({
  text: this.score + '',
  fill: 'lime',
  fontSize: 64,
}).addChildTo(this);
scoreLabel.setPosition(this.gridX.center(), this.gridY.center());
scoreLabel.alpha = 0.6;
// 連続ヒット数
this.hitNumber = 0;
```

* **this.score**は、スコア用の変数です。
* **Label**クラスを使ってスコア表示用のラベルを追加しています。ラベルの**text**プロパティにスコア値を設定しています。
* **alpha**は透明度で、0に近づくほど透明になります。
* **this.hitNumber**ブロックを連続で崩した回数を記録します。

##### 232から240行目

```js
  // スコア加算
  this.addScore();
},
// スコア加算処理
addScore: function() {
  this.hitNumber++;
  this.score += this.hitNumber * 10;
  this.scoreLabel.text = this.score;
},
});
```
* ブロックを崩した時に**addScore**関数を呼び出して、スコアを加算します。
* **addScore**関数内では、連続ヒット数の場合、スコアを上乗せしています。
* スコア表示用ラベルの**text**プロパティに現在のスコアを反映させれば、ラベルが更新されます。

##### 209から215行目

```js
// クリアチェック
if (this.blockGroup.children.length === 0) {
  // スコアをリザルトシーンに渡す
  this.exit({
    score: this.score,  
  });
}
```
* ブロックを全て崩した時がクリアの条件ですので、**length**の値をチェックします。
* **exit**にスコアを渡して、リザルトシーンに移動します。**nextLabel**を設定していないのは、**main**の次はデフォルトで**result**が設定されているためです。
* 実際にクリアすると、リザルト画面にスコアが引き渡されているのが分かるかと思います。

### まとめ

今回のチュートリアルでは、**phina.js**を使った簡単なブロック崩しの作成について触れさせて頂きました。
**phina.js**には、今回では紹介しきれていない機能がまだまだありますので、これからこのブログなどで私なりに紹介できればと思っております。

今回の一連のエントリーに関する質問などは、私([@alkn203](https://twitter.com/alkn203))までお気軽にどうぞ。
**phina.js**に関する質問は、作者のphi氏([@phi-jp](https://twitter.com/phi_jp))に直接して頂くか、[gitter](https://gitter.im/phi-jp/phina.js)でして頂ければ、メンバーが真剣に答えてくれると思います。(微力ながら私も参加しています）

今回のチュートリアルが、**phina.js**を使ったゲーム作成の際に少しでも参考になれば幸いです。
最後までお付き合い頂きありがとうございました。
