+++
date = "2015-11-17T23:59:06+09:00"
draft = false
slug = ""
tags = ["works", "calendar"]
title = "【phina.js】ゲーム以外の作成サンプル（カレンダー）"
+++

### はじめに
以前にQiitaに[tmlib.jsで作ってみた⑨＝カレンダー＝](http://qiita.com/alkn203/items/5f9b227bf13b395eb7e0)を投稿しましたが、今回、これを[phina.js](https://github.com/phi-jp/phina.js)でリファクタンリングしてみました。

### 作成したサンプルプログラム
* Windowsのカレンダーを参考に作りました。
* 日付の配置に**Grid**を使い、スライド処理に**tweener**を使っています。

<div class='runstant'><iframe src='http://goo.gl/odsvmZ' width='100%' height='640px' style='border:0px;box-shadow:0px 0px 2px 0px #aaa'></iframe></div>

### まとめ
**phina.js**を使ってゲーム以外のプログラムを作成してみました。**Electron**などと組み合わせるといわゆるガジェット系のデスクトップアプリも作ることができると思います。他にも電卓などを作ってみたいと思っていますので、その時はまたこのブログで紹介したいと思います。

---
 * *2015-11-21　一部の環境で時刻ラベルが画面からはみ出さないように修正（[@phi-jp](https://twitter.com/phi_jp)さんありがとうございました。）*
 * *月ボタンをもう少し押しやすいように変更*
