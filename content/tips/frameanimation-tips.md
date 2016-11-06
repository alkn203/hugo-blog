+++
date = "2016-06-11T23:59:06+09:00"
draft = false
slug = ""
tags = ["tips", "frameanimation", "sprite"]
title = "フレームアニメーション関係Tips"

+++

### 今回のTips
以前にも紹介したように、phina.jsには**FrameAnimation**という便利な機能があります。
今回はより便利に使うための2つのTipsを紹介します。

### スプライトのサイズがスプライトシートに定義されたサイズにフィットしないようにする
フレームアニメーションを使うためには、**SpriteSheet**を定義する必要がありますが
デフォルトだと実際のコード中でスプライトのサイズを変えてもスプライトシート(json)に書かれた**width**と**height**に
戻ってしまい、スプライトを拡大して表示したい時に意図した結果になりません。

```js
// スプライトシート
spritesheet: {
  'explosion_ss':
  {
    "frame": {
      "width": 20, // ←この値にフィットされる
      "height": 20,// ←この値にフィットされる
      "cols": 16,
      "rows": 1,
    },
```

これを回避するためには、**FrameAnimation**クラスのプロパティ**fit**を**false**にします。
これで実際に変更したサイズで正しく表示されるようになります。
サンプルでは20X20で切り出した画像を5倍で表示しています。

```js
// 親クラスの初期化
this.superInit('explosion', 20, 20); // ←実際の画像の切り取りサイズ
// SpriteSheetをスプライトにアタッチ
var anim = FrameAnimation('explosion_ss').attachTo(this);
// スプライトシートのサイズにフィットさせない
anim.fit = false; // ←ここ
//アニメーションを再生する
anim.gotoAndPlay('start');
// サイズ変更
this.setSize(20*5, 20*5); // ←サイズ変更
```

### フレームアニメーションの終了を検知する
ゲーム作成において、フレームアニメーションが終了した後に何か処理をしたい時があるかと思いまます。
この場合、**FrameAnimation**クラスの**finished**プロパティの値が**true**かどうかを調べると便利です。
サンプルでは爆発アニメーションが終了したら、自身を消去するようにしています。（consoleに結果表示）

```js
// 毎フレーム処理
update: function() {
  // アニメーションが終わったら自身を消去
  if (this.anim.finished) { // ←ここで判定
    this.remove();
    console.log('removed');
  }
},
```
#### サンプル(runstant上)
<div class='runstant'><iframe src='http://runstant.com/alkn203/projects/e18e94bb' width='100%' height='640px' style='border:0px;box-shadow:0px 0px 2px 0px #aaa'></iframe></div>