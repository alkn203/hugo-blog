+++
date = "2019-06-14T20:55:06+09:00"
draft = false
slug = ""
tags = ["phina.js","tweener","tips"]
title = "【phina.js】パスに沿ったオブジェクト移動"
eyecatch = "/images/movealongpath.gif"
+++

![movealongpath.gif](/images/movealongpath.gif)

### パスに沿ったオブジェクトの移動
ゲームを作成していると、動く床などのように一定のルートに従ってオブジェクトを移動させたい時があると思います。 **phina.js**を使って、自分なりにその処理を実装してみました。

### 動作サンプル
まずは以下のサンプルをご覧下さい。線に沿ってオブジェクトが移動します。

<div class='runstant'><iframe src='https://runstant.com/alkn203/projects/10bfc984/full' width='100%' height='320px' style='border:0px;box-shadow:0px 0px 2px 0px #aaa'></iframe></div>

[runstantで確認](https://runstant.com/alkn203/projects/10bfc984)

- オブジェクトの移動には、非同期処理が行える**tweener**を利用します。
- **tweener**の内部処理で使われている**_add**関数で処理をスタックさせています。
- **tweener**は、基本的にスタックされた順に非同期処理されるので、次の移動先である各頂点の位置を順番に与えることで、結果としてパスに沿った移動が可能になります。

### 課題
- 今回のサンプルでは、移動速度が一定になるように各頂点の距離が同一になるように配置しています。
- 一定の時間で移動させるのが**tweener**の処理ですので、距離が変わると移動速度も変わることになります。
- 各頂点の距離に応じて**duration**を変えると速度を一定にすることが可能になると思われますが、その辺は次回の課題にしたいと思います。
