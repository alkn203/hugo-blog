+++
date = "2021-06-19T11:58:06+09:00"
draft = false
slug = ""
tags = ["sprite"]
title = "フレームアニメーションを設定する"
eyecatch = "frameanimation.gif"
+++

![frameanimation](frameanimation.gif)

**Sprite**画像でフレームアニメーションを設定します。

## スプライトシート画像を用意する
スプライトシート画像とは、以下のようにアニメーション用のコマをシートの様に並べた画像です。

![](https://storage.googleapis.com/zenn-user-upload/0apyqr2u79w8e2n0yuim6uqlwgp2)

* 上の画像だと、横6x縦3の計18コマから成り立っています。
* ゲーム作成ではコマのことを**フレーム**と呼ぶことが多いです。

## スプライトシート画像を読み込む
通常の画像と同じように**ASSETS**として読み込みます。

```js
// アセット
var ASSETS = {
  // 画像
  image: {
    'tomapiko': 'https://cdn.jsdelivr.net/gh/phinajs/phina.js@develop/assets/images/tomapiko_ss.png',
  },
};
```

## フレームアニメーション情報を読み込む
* フレームアニメーション情報が定義された**json**形式のファイルを読み込む方法が一般的ですが、今回はファイルからではなく**ASEETS**として、コード内に定義しています。
* コード内に記述して十分に動作確認した後に、外部**json**ファイルに保存してアセットとして読み込む方が、開発する上では効率的です。

```js
// アセット
var ASSETS = {
  // 画像
  image: {
    'tomapiko': 'https://cdn.jsdelivr.net/gh/phinajs/phina.js@develop/assets/images/tomapiko_ss.png',
  },
  // スプライトシート
  spritesheet: {
    "tomapiko_ss":
    {
      // フレーム情報
      "frame": {
        "width": 64, // 1フレームの画像サイズ（横）
        "height": 64, // 1フレームの画像サイズ（縦）
        "cols": 6, // フレーム数（横）
        "rows": 3, // フレーム数（縦）
      },
      // アニメーション情報
      "animations" : {
        "walk": { // アニメーション名
          "frames": [12,13,14], // フレーム番号範囲
          "next": "walk", // 次のアニメーション
          "frequency": 6, // アニメーション間隔
        },
      }
    },
  }
};
```

* **frames**にアニメーションに使いたいフレーム番号の範囲を書きます。 **0**から始まることに注意してください。
* **next**に次のアニメーションを指定します。同じ名前にするとループします。
* **frequency**でアニメーションの間隔を指定します。小さくすれば速くなり、大きくすれば遅くなります。

## スプライトシート画像の作成
スプライトシート画像は、通常の画像と同じく**Sprite**クラスを使って作成しますが、アセット名の次の引数で**１フレームの画像サイズ**を指定します。

```js
// スプライト画像作成
var sprite = Sprite('tomapiko', 64, 64).addChildTo(this);
```

## フレームアニメーションの設定

```js
// スプライトにフレームアニメーションをアタッチ
var anim = FrameAnimation('tomapiko_ss').attachTo(sprite);
// アニメーションを指定する
anim.gotoAndPlay('walk');
```

* **FrameAnimation**クラスのコンストラクタにスプライトシートのアセット名を指定して、スプライトにアタッチします。
* **gotoAndPlay**関数にアニメーション名を指定すると、そのアニメーションが自動で再生されます。


## サンプルコード
<details>
<summary>コードを見る</summary>

```js
// グローバルに展開
phina.globalize();
// アセット
var ASSETS = {
  // 画像
  image: {
    'tomapiko': 'https://cdn.jsdelivr.net/gh/phinajs/phina.js@develop/assets/images/tomapiko_ss.png',
  },
  // スプライトシート
  spritesheet: {
    "tomapiko_ss":
    {
      // フレーム情報
      "frame": {
        "width": 64, // 1フレームの画像サイズ（横）
        "height": 64, // 1フレームの画像サイズ（縦）
        "cols": 6, // フレーム数（横）
        "rows": 3, // フレーム数（縦）
      },
      // アニメーション情報
      "animations" : {
        "walk": { // アニメーション名
          "frames": [12,13,14], // フレーム番号範囲
          "next": "walk", // 次のアニメーション
          "frequency": 6, // アニメーション間隔
        },
      }
    },
  }
};
/*
 * メインシーン
 */
phina.define("MainScene", {
  // 継承
  superClass: 'DisplayScene',
  // 初期化
  init: function() {
    // 親クラス初期化
    this.superInit();
    // 背景色
    this.backgroundColor = 'skyblue';
    // スプライト画像作成
    var sprite = Sprite('tomapiko', 64, 64).addChildTo(this);
    sprite.setPosition(320, 480);
    // スプライトにフレームアニメーションをアタッチ
    var anim = FrameAnimation('tomapiko_ss').attachTo(sprite);
    // アニメーションを指定する
    anim.gotoAndPlay('walk');
  },
});
/*
 * メイン処理
 */
phina.main(function() {
  // アプリケーションを生成
  var app = GameApp({
    // MainScene から開始
    startLabel: 'main',
    // アセット読み込み
    assets: ASSETS,
  });
  // fps表示
  //app.enableStats();
  // 実行
  app.run();
});
```

</details>

## runstantプロジェクト
https://runstant.com/alkn203/projects/2764859c