+++
date = "2019-07-02T20:55:06+09:00"
draft = true
slug = ""
tags = ["phina.js","tips","collision"]
title = "【phina.js】当たり判定パターンについて"
eyecatch = "tweenercolor.gif"
+++

![tweenercolor.gif](tweenercolor.gif)

### はじめに
ゲーム作りにおいて、当たり判定ほど奥深いものはないといっても過言ではありません。   
私も**phina.js**を使っている時、毎回のように当たり判定には悩まされています。   
今回は、**phina.js**における当たり判定パターンについて、これまで使ってきた経験から考えてみます。

### hitTestElementを使う
- **phina.app.Object2d**を継承したクラスであれば、**hitTestElement**を使えば手軽に当たり判定ができます。
- 矩形同士での判定になりますので、手軽な一方でグラフィックが矩形より小さいと大雑把な判定になってしまうという欠点があります。

> 参考記事   
> [[phina.js-Tips-41] 当たり判定を行う（矩形判定）](https://qiita.com/alkn203/items/2e567ee47a4a1a8b8ee4)

### Collisionを自前で用意する
- [	【phina.js】2Dシューティングチュートリアル =第6回 当たり判定とアニメーションイベントとグループ=](https://alkn203.github.io/blog/posts/tutorials/phina-shooting-06/)で作ったような**Collision**クラスを使う方法です。
- 任意のサイズに設定することができるので柔軟性がある一方、相対座標を意識して気を付けないと正しい当たり判定ができないという欠点があります。

> 参考記事   
> [[phina.js]Colliderアクセサリを作ってみた話](https://qiita.com/alkn203/items/8c24d433e9eb8ea0f4bc)

### 当たり判定用のCollisionを親にしてスプライトなどを子として追加する
- **RectangleShape**や**CircleShape**を当たり判定の対象として、スプライト画像をその子要素として追加する方法です。
- 絶対座標で当たり判定ができるというメリットがあります。
- 