+++
date = "2019-06-12T20:55:06+09:00"
draft = false
slug = ""
tags = ["phina.js","game"]
title = "【phina.js】シンプルなタイピングゲームを作ってみた"
eyecatch = "/images/keywordshot.gif"
+++

![keywordshot](/images/keywordshot.gif)

### はじめに
**phina.js**でシンプルなタイピングゲームを作ってみました。
私はあまりタイピングは得意な方ではないので、自分で作っておきながら途中でゲームオーバーになりますが、皆さんはいかがでしょうか。

<div class='runstant'><iframe src='https://alkn203.github.io/phina-games/keyword-shot/' width='100%' height='380px' style='border:0px;box-shadow:0px 0px 2px 0px #aaa'></iframe></div>

[別画面でプレイする](https://alkn203.github.io/phina-games/keyword-shot/)

* 画面左から飛んでくるhtmlやjavascriptに関するキーワードをタイプするだけのゲームです。
* 大文字小文字は区別しません。

### 改定
2019/5/18 ライフ制導入

### カスタマイズ
キーワード一覧は、以下のように改行区切りのテキストファイルからアセットとして読み込んでいますので、中身を変えれば
別言語のキーワードなどもイケると思います。

```javascript
// アセット
var ASSETS = {
  // キーワード一覧
  text: {
    'keywords': 'https://cdn.jsdelivr.net/gh/alkn203/phina-games@master/keyword-shot/assets/keywords',
  },
};
```

### リファレンス
キーワード一覧は、以下のサイトから引用させて頂きました。ありがとうございます。
[JavaScriptのキーワードとか予約語をかき集めてみた - Born Neet](http://blog.bornneet.com/Entry/262/)