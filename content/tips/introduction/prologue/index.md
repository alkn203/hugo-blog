+++
date = "2021-06-02T20:56:06+09:00"
draft = false
slug = ""
tags = ["phina.is"]
title = "はじめに"
+++

## phina.jsについて
自分なりに**phina.js**について書いたエントリーです。

[phina.js事始め【改】](https://alkn203.github.io/blog/2021/06/11/start-guide/)

## phina.jsの開発環境を構築する
* このTips集では、Webブラウザ上でコーディング・実行が可能な[runstant](https://runstant.com)を使用してますので、基本的に開発環境として必要なものはWebブラウザだけです。
* 自分の好きなエディタを使ってローカルで開発することも可能ですが、クライアントサイドで実行されるjavascriptは、セキュリティ上の理由からローカルファイルへのアクセスが制限されています。
* 対応策としては、Visual Studio Codeなどのエディタの拡張機能にあるローカルサーバーを使う方法があります。

## phina.jsを読み込む
**runstantのView(html)** タブのコードは以下のようになっています。

```js
<!doctype html>

<html>
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, user-scalable=no" />
    <meta name="apple-mobile-web-app-capable" content="yes" />

    <title>${title}</title>
    <meta name="description" content="${description}" />

    <style>${style}</style>
  </head>
  <body>
    <script src="https://cdn.jsdelivr.net/gh/phinajs/phina.js@v0.2.3/build/phina.js"></script>
    <script>${script}</script>
  </body>
</html>
```

* **script** タグで上記のように読み込みます。**rawgit** はサービスが終了したので、**CDN** として **jsdeliver** を使用しています。
* **phina.js**のバージョンは現在**0.2.3**が最新ですので、特段の事情がない限りは最新版を使用することを強くおすすめします。