+++
date = "2019-06-29T20:55:06+09:00"
draft = false
slug = ""
tags = ["phina.js","html5","runstant","Github"]
title = "【phina.js 】Webブラウザだけでhtml5ゲームを作成して公開する"
eyecatch = "runstantdownload.gif"
+++

### はじめに
ゲームを作っている人なら、自分の作ったゲームを誰かに遊んでもらいたい気持ちは皆持っていると思います。今では、スマホアプリを作ってプラットフォームで公開する方法がメジャーですが、初心者にとってはそう簡単にはいかないものです。    
中には、**アプリ化まではいかないけど何かゲームを作って公開してみたい**と思っている人もいるでしょう。そのようなときは、**javascript**で**html5**ゲームを作って、Webで公開することをおすすめします。（もちろんhtml5ゲームをアプリ化する方法もあります）

### Webブラウザだけでゲームを作成して公開する
今回は、特別な環境構築をせずに、Webブラウザだけを使って**html5**ゲームを公開する方法を紹介します。大まかな流れは以下のとおりです。

1. **runstant**でコーティングする
1. **runstant**プロジェクトを**html**ファイルとしてダウンロードする
1. **GitHub**にゲーム公開用のリポジトリを作る
1. **html**ファイルを**GitHub**のリポジトリにアップロードする
1. **GitHub**のリポジトリの設定で**GitHub Pages**を有効にする

それでは、順番に説明していきます。

### runstantでコーティングする
[runstant](https://runstant.com/)は、**phina.js**の生みの親である[phi](https://twitter.com/phi_jp)さんが開発したWebブラウザ上で動くオンラインエディタで、ユーザー登録すれば誰でも利用することができます。 **phina.js**の公式エディタでもあり、実際に様々なプログラムが作られて公開されています。

#### phina.jsのひな形からゲームを作成
これまでも触れていますが、普段私が使っている[phina.jsのひな形](https://runstant.com/alkn203/projects/8f0388a4)から作ることで、簡単に始められます。

#### 今回作ったゲーム
サンプルとして、制限時間内にどれだけ円をタッチできるかを競う単純なゲームを作りました。

[Touch The Circle(runstant)](https://runstant.com/alkn203/projects/21370e4b)

### runstantプロジェクトをhtmlファイルとしてダウンロードする
ゲームを作ったら、Webに公開するファイルの準備をする必要がありますが、**runstant**の便利な機能として、プロジェクトのダウンロードがあります。
ダウンロードされたファイルは**html**形式で**javascript**のコード部分もパッケージ化されているので、このファイル１つあればゲームとして動作します。

#### プロジェクトのダウンロード方法
![runstantdownload.gif](runstantdownload.gif)

* **runstant**の画面下部にあるボタンをクリックすると、プロジェクトのダウンロードができます。ダウンロード先は任意の場所にして、ファイル名を**index.html**に変更して下さい。

### GitHubにゲーム公開用のリポジトリを作る
[GitHub](https://github.co.jp/)は、プログラマなら誰もが知るようになりつつある、主にソースコードのバージョン管理を目的としたサービスです。
今回は、この**GitHub**をゲームの公開用サーバとして利用します。

ユーザー登録を行ったら、ゲーム公開用のリポジトリを作成します。

* ユーザーホーム画面で「New」ボタンをクリックします。

![newrepository.gif](newrepository.gif)

1. 作成画面でリポジトリ名を入力します。
1.  公開範囲が「Public」になっているのを確認します。
1. 「リポジトリをREAD ME で初期化」にチェックを入れます。
1. 「Create repository」ボタンをクリックします。

### htmlファイルをGitHubのリポジトリにアップロードする
リポジトリの用意が出来たら、**runstant**からダウンロードした**html**ファイルをリポジトリにアップロードします。**GitHub**は、ローカル環境から**git**のコマンドを駆使して使うイメージがありますが、**GitHub**上のGUI操作でもファイルのアップロード程度はできます。

![uploadfile.gif](uploadfile.gif)

* 「Upload files」ボタンをクリックします。

![uploadfile2.gif](uploadfile2.gif)

1. 上の領域にダウンロードした**html**ファイルをドラッグするか、ファイル選択ダイアログでファイルを選択します。
2.  「Commit Changes」ボタンをクリックします。

![uploaded.gif](uploaded.gif)

* ファイルがアップロードされたのを確認します。

### GitHubのリポジトリの設定でGitHub Pagesを有効にする

![setting.gif](setting.gif)

*  上部メニューから「Setting」をクリックします。

![ghpages.gif](ghpages.gif)

* 「github pages」を有効にします。以前は、**git** で**gh-pages**名でブランチを切って、**push**する必要がありましたが、現在は不要になっています。

![ghpagescheck.gif](ghpagescheck.gif)

* 上部に表示される アドレスが公開先のアドレスになります。
* 反映されるまでには、少し時間がかかる場合もあるようです。

### 公開先
[Touch The Circle](https://alkn203.github.io/touchthecircle/)