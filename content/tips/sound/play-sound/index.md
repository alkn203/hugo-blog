+++
date = "2021-06-25T09:59:06+09:00"
draft = false
slug = ""
tags = ["sound"]
title = "音を再生する"
eyecatch = "play-sound.png"
+++

![play-sound](play-sound.png)

## 音を再生する方法
**phina.js**で音を再生するには、音源をアセットとして読み込みそれを再生します。

## アセットの読み込み
**assets**としてサウンドファイルを読み込みます。

```js
// アセット
var ASSETS = {
  // サウンド
  sound: {
    'se1': 'https://cdn.jsdelivr.net/gh/alkn203/assets_etc@master/maou-fantasy-01.mp3',
  },
};
```

## 音の再生

```js
// 一回限りタッチイベント
shape.one('pointend', function() {
  // 音再生
  SoundManager.play('se1');
});
```

* 音を再生するには**SoundManager**クラスの**play**メソッドを使います。
* **play**メソッドの場合は、1回限り再生されます。

## サンプルコード
<details><summary>コードを見る</summary>
```js
// グローバルに展開
phina.globalize();
// アセット
var ASSETS = {
  // サウンド
  sound: {
    'se1': 'https://cdn.jsdelivr.net/gh/alkn203/assets_etc@master/maou-fantasy-01.mp3',
  },
};
/*
 * メインシーン
 */
phina.define("MainScene", {
  // 継承
  superClass: 'DisplayScene',
  // コンストラクタ
  init: function() {
    // 親クラス初期化
    this.superInit();
    // 背景
    this.backgroundColor = 'black';
    // Shape作成
    var shape = Shape({
      backgroundColor: 'red',
      x: 320,
      y: 480,
    }).addChildTo(this);
    // タッチを有効に
    shape.setInteractive(true);
    // 一回限りタッチイベント
    shape.one('pointend', function() {
      // 音再生
      SoundManager.play('se1');
    });
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

## 使用素材について
音楽：魔王魂

## runstantプロジェクト
{{< targetblank "https://runstant.com/alkn203/projects/d1c1ce56" >}}