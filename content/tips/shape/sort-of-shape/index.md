+++
date = "2021-06-18T21:55:06+09:00"
draft = false
slug = ""
tags = ["shape"]
title = "Shapeの種類について知る"
eyecatch = "sort-of-shape.png"
+++

![sort-of-shape](sort-of-shape.png)

## Shapeの種類
**phina.js**には、以下の種類の**Shape**が予め用意されています。

## RectangleShape
矩形の**Shape**です。

| プロパティ | 説明 |
| ---- | ---- |
| fill | 塗りつぶし色 |
| stroke | 縁取り線色 |
| strokeWidth | 縁取り線の太さ |
| cornerRadius | 角丸め値 |

```js
// RectangleShape
RectangleShape({
  width: 128,
  height: 128,
  fill: 'red',
  stroke: 'lime',
  strokeWidth: 16,
  cornerRadius: 16
}).addChildTo(this).setPosition(320, 200);
```

## CircleShape
円形の**Shape**です。

| プロパティ | 説明 |
| ---- | ---- |
| fill | 塗りつぶし色 |
| stroke | 縁取り線色 |
| strokeWidth | 縁取り線の太さ |
| radius | 半径 |

```js
// CircleShape
CircleShape({
  fill: 'green',
  stroke: 'white',
  strokeWidth: 16,
  radius: 64
}).addChildTo(this).setPosition(320, 400);
```

## TriangleShape
三角形の**Shape**です。

| プロパティ | 説明 |
| ---- | ---- |
| fill | 塗りつぶし色 |
| stroke | 縁取り線色 |
| strokeWidth | 縁取り線の太さ |
| radius | 半径 |

```js
// TriangleShape
TriangleShape({
  fill: 'purple',
  stroke: 'white',
  strokeWidth: 16,
  radius: 64
}).addChildTo(this).setPosition(320, 600);
```

## StarShape
星形の**Shape**です。

| プロパティ | 説明 |
| ---- | ---- |
| fill | 塗りつぶし色 |
| stroke | 縁取り線色 |
| strokeWidth | 縁取り線の太さ |
| radius | 半径 |
| sides | 外側の頂点数 |
| sideIndent | 凹み間隔 |

```js
// StarShape
StarShape({
  stroke: 'white',
  strokeWidth: 16,
  radius: 64,
  sideIndent: 0.5,
}).addChildTo(this).setPosition(320, 800);
```

## PolygonShape
正多角形の**Shape**です。

| プロパティ | 説明 |
| ---- | ---- |
| fill | 塗りつぶし色 |
| stroke | 縁取り線色 |
| strokeWidth | 縁取り線の太さ |
| radius | 半径 |
| sides | 頂点数 |

```js
// PolygonShape
PolygonShape({
  stroke: 'white',
  strokeWidth: 16,
  radius: 64,
  sides: 8,
}).addChildTo(this).setPosition(100, 480);
```

## HeartShape
ハート形の**Shape**です。

| プロパティ | 説明 |
| ---- | ---- |
| fill | 塗りつぶし色 |
| stroke | 縁取り線色 |
| strokeWidth | 縁取り線の太さ |
| radius | 半径 |
| cornerAngle | 両側の辺の角度 |

```js
// HeartShape
HeartShape({
  stroke: 'white',
  strokeWidth: 16,
  radius: 64,
}).addChildTo(this).setPosition(540, 480);    
```

## PathShape
頂点配列からパスを作成する**Shape**です。

| プロパティ | 説明 |
| ---- | ---- |
| paths | 頂点配列（Vector2型） |

</br>


| メソッド | 説明 |
| ---- | ---- |
| setPaths(paths) | 頂点配列をセット |
| clear | 頂点配列をクリア |
| addPaths(paths) | 頂点配列を追加 |
| addPath(x, y) | 頂点を追加 |
| getPath(i) | 指定されたインデックスの頂点を返す |
| getPaths | 頂点配列を返す |
| changePath(i, x, y) | 指定されたインデックスの頂点を変更する |

```js
// PathShape
PathShape({
  paths: [Vector2(0, 0), Vector2(200, 200), Vector2(200, 680),
          Vector2(420, 680), Vector2(640, 960)]
}).addChildTo(this);
```

## サンプルコード
<details>
<summary>コードを見る</summary>

```js
/ グローバルに展開
phina.globalize();
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
    this.backgroundColor = 'black';
    // RectangleShape
    RectangleShape({
      width: 128,
      height: 128,
      fill: 'red',
      stroke: 'white',
      strokeWidth: 16,
      cornerRadius: 16
    }).addChildTo(this).setPosition(320, 200);
    // CircleShape
    CircleShape({
      fill: 'green',
      stroke: 'white',
      strokeWidth: 16,
      radius: 64
    }).addChildTo(this).setPosition(320, 400);
    // TriangleShape
    TriangleShape({
      fill: 'purple',
      stroke: 'white',
      strokeWidth: 16,
      radius: 64
    }).addChildTo(this).setPosition(320, 600);
    // StarShape
    StarShape({
      stroke: 'white',
      strokeWidth: 16,
      radius: 64,
      sideIndent: 0.5,
    }).addChildTo(this).setPosition(320, 800);
    // PolygonShape
    PolygonShape({
      stroke: 'white',
      strokeWidth: 16,
      radius: 64,
      sides: 8,
    }).addChildTo(this).setPosition(100, 480);
    // HeartShape
    HeartShape({
      stroke: 'white',
      strokeWidth: 16,
      radius: 64,
    }).addChildTo(this).setPosition(540, 480);
    // PathShape
    PathShape({
      paths: [Vector2(0, 0), Vector2(200, 200), Vector2(200, 680),
              Vector2(420, 680), Vector2(640, 960)]
    }).addChildTo(this);    
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
  });
  // fps表示
  //app.enableStats();
  // 実行
  app.run();
});
```

</details>

## runstantプロジェクト
{{< targetblank "https://runstant.com/alkn203/projects/23f3b015" >}}