+++
date = "2021-06-25T10:59:06+09:00"
draft = false
slug = ""
tags = ["sound"]
title = "音の再生を停止する"
eyecatch = "stop-sound.png"
+++

![stop-sound](stop-sound.png)

## 音の再生を停止する方法
**phina.js**で音の再生を停止するには、 **stopMusic**メソッドを使います。。

## playMusicメソッド

```js
// タッチイベント
playBtn.onpointend = function() {
  // 音再生
  SoundManager.playMusic('bgm');
};
```

* 音楽を再生します。
* デフォルトで音楽はループします。

## stopMusicメソッド

```js
// タッチイベント
stopBtn.onpointend = function() {
  // 音停止
  SoundManager.stopMusic();
};
```


## サンプルコード
<details><summary>コードを見る</summary>
```js
// グローバルに展開
phina.globalize();
// アセット
var ASSETS = {
  // サウンド
  sound: {
    'bgm': 'https://cdn.jsdelivr.net/gh/alkn203/assets_etc@master/maou-fantasy-01.mp3',
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
    // 再生ボタン作成
    var playBtn = Shape({
      backgroundColor: 'red',
      x: this.gridX.center(-2),
      y: this.gridY.center(),
    }).addChildTo(this);
    // タッチを有効に
    playBtn.setInteractive(true);
    // タッチイベント
    playBtn.onpointend = function() {
      // 音再生
      SoundManager.playMusic('bgm');
    };
    // 停止ボタン作成
    var stopBtn = Shape({
      backgroundColor: 'blue',
      x: this.gridX.center(2),
      y: this.gridY.center(),
    }).addChildTo(this);
    // タッチを有効に
    stopBtn.setInteractive(true);
    // タッチイベント
    stopBtn.onpointend = function() {
      // 音停止
      SoundManager.stopMusic();
    };
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
{{< targetblank "https://runstant.com/alkn203/projects/8ba7cc91" >}}