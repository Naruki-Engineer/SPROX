# Automatic-Bulid-System
こんにちは、なるきです。GitHub で開発環境の整理･紹介をしています。  
Hello! My name is Naruki.  
We introduce the development environment with GitHub.

## Introduction
前はローカルにページを作っていましたが、Git を個人導入するに当たり、公開場所を GitHub 上に移行しました。  
開発環境を Javascript に統一したいという試みも最近始めました。  
クライアントサイドで処理したほうがサーバーに負担がない、PHP は速度が遅い などの理由です。  
PHP はサーバー環境を立ち上げる必要がありますが、Javascript はその必要がありません。

# Infomation
ここでは フロントエンドで使用できる、オートマチックビルドシステムについて補足解説します。  
このデータ、及び sprox.co.jp は Git でファイル管理しています。  
作業ディレクトリは、``sprox-master`` のネットワークドライブ上にあります。  
自分の環境、ローカルディレクトリにシステムを導入したい場合、余別で開発環境が必要になるため、  
とりあえず、NAS 環境下での仮想サーバー構築を想定しています。  
誰でもアクセスでき、誰でも同じビルドツールを使用できる環境にあるということを念頭に置いて下さい。  
個々で、ビルドツールを起動すれば、個々で仮想サーバーを立ち上げることができ、各々のPCでコンパイル等行えます。

# Node.js
https://nodejs.org/ja/download/  
上記のリンクからダウンロードしてインストールして下さい。**LTS 推奨版を選択して下さい**。

# 黒い画面のやつ
Node.js はコマンドで使用します。Windows は コマンドプロンプト、Mac は ターミナル を使用します。Windows は スタートメニューから ``Windows + R`` で実行ウィンドウを呼び出し ``cmd`` と打ち込めば起動できます。Mac は Finder で ``⌘ + Shift + U`` でユーティリティフォルダに移動できます。そこからターミナルを選択してください。

```
node -v
```

上記コマンドを入力して Enter すると Node のバージョンが返ってきます。返ってこない場合は一旦コマンドラインを再起動しましょう。

```
v6.10.1
```

バージョンは定期的に安定版の更新が来ます。アップデートしてもしなくてもいいですが、不具合が起きた場合はもとに戻せるようにして下さい。Mac は [nodebrew](//github.com/hokaccha/nodebrew) , Windows は [nodist](//github.com/marcelklehr/nodist) を使うとバージョン管理が簡単です。インストールや使用方法は割愛致します。  

※ nodebrew 等インストールしておくとコマンドで簡単にバージョンの切り替えが可能です。

```
npm -v
```

つづいて、npm のコマンドを入力して Enter

```
v4.1.2
```

バージョン情報が返ってくれば問題なし。  

グローバル(自分のPC環境)に Gulp をインストールする必要があります。以下のコマンドを実行

```
npm install -g gulp       // Windows
sudo npm install -g gulp  // Mac
```

# これだけです
自動化ツールはこれで基本的には動きます。Ruby や Git などインストールしておきましょう。  

※ Mac は 開発環境がプリインストール済なので基本インストール不要。[Homebrew](//brew.sh) をインストールしておくと管理も状況確認も簡単。Windows はインストールされていないので [Chocolatey](//chocolatey.org) でインストール管理すると簡単。

# 動かし方
コマンドラインは起動したまま、作業したいディレクトリに移動します。今回はこのページの階層に移動する手段を説明します。

```
sprox-master
  │
  └─ SPROX
      │
      └─ 11_制作
          │
          └─ 小長谷
              │
              └─ Development
                  │
                  └─ Sprox
```

上記のディレクトリに作業フォルダがあります。コマンドライン cd のコマンドで作業ディレクトリに移動します。

```
cd /Volumes/SPROX/11_制作/小長谷/Development/Sprox     // Mac のみ
```

cd のコマンドを打ち込んだあと、階層フォルダをドラッグ&ドロップするとディレクトリまでを自動で挿入してくれます。

```
PC-name:src username$
```

目的の階層に移動できました。  

Windows の場合、ネットワーク上のディレクトリにアクセスするには、

```
pushd ¥Sprox-master¥SPROX¥11_制作¥小長谷¥Development¥Sprox     // Winows のみ
```

``pushd`` のコマンドで仮想ドライブとして、対象のディレクトリにアクセスする必要があります。

```
Z:¥¥Sprox     // Winows のみ
```

仮想ドライブとして対象のディレクトリにアクセスできました。  

作業ディレクトリにアクセスできたら最後に1つコマンドを打つだけ！

```
gulp
```

ネットワーク経由ですのでアクセスに時間がかかります。  

```
[**:**:**] Using gulpfile ..gulpfile.js
[**:**:**] Starting 'server'...
[**:**:**] Finished 'server' after 606 ms
[**:**:**] Starting 'default'...
[**:**:**] Finished 'default' after 26 s
[BS] Proxying: http://192.168.0.**
[BS] Access URLs:
 -------------------------------------
       Local: http://localhost:3000
    External: http://192.168.0.**:3000
 -------------------------------------
          UI: http://localhost:3001
 UI External: http://192.168.0.**:3001
 -------------------------------------
```

起動が終わるとブラウザが勝手に対象ローカルサーバーにアクセスします。  
先に XAMPP とか MAMP でサーバー建ててると競合します。気をつけて下さい。  
ですので、先に Gulp で ``localhost:3000`` を確保してから XAMPP なり起動するといいでしょう。

以上です。あとはこちらで設定したスクリプトで自動更新や SASS や Pug を自動コンパイルをしてくれます。  設定することは一切ありません。ファイルを監視しているので対象のファイルに変更があった場合、即座に自動で処理されます。

# The Files Editing
上記の動作を行い Gulp が起動すると、自分の ローカルホスト に Sprox のダミーサイトが立ち上がります。  
このページは データを監視しているので、データに変更があった場合、即座に自動でブラウザが更新され、  
なおかつ、ローカルエリアネットワーク内なら、どんな端末からアクセスしても同期して表示します。  

複数端末での確認が超ラク。

今回 sprox.co.jp の再コーティングに当たり、全て Javasript で構築しました。フォームは PHP のままです。  
使用した テンプレートエンジンは Pug、プリプロセッサーは SASS、全てフロントサイドで動作します。  
共通データは Json で管理できるようにしてあります。Pug 内の config で管理できない部分の補完をしています。  
なお、サーバー側で動作する PHP とは違うので、サーバーにソースデータ上げても意味ありません。  
自分でコンパイルして出来上がったデータをサーバーに上げて下さい。

フロントサイドでプログラムを組むことで サーバー側で PHP の動作をさせないのが目的です。軽い。

一応 ``src`` と ``dist`` で開発と出力ディレクトリは分けてます。

つまり ``src`` 内のコンパイル言語のデータを更新して下さい。  
コンパイルされたデータは ``dist`` に書き出されます。  
サーバーアップするのは ``dist`` 内のデータです。

ちなみにローカルサーバーで見ているのデータは ``dist`` のデータです。

PHP も動かしたい、、、そんなときは自分で ローカルに PHP のパスを通して立ち上げれば OK  
やり方がわからない？ XAMPP とか MAMP とかで localhost に PHP 起動しておけば OK  
なんなら 対象のディレクトリ宛てで XAMPP とか MAMP 立ち上げちゃえば Gulp を起動しなくても OK

## [Pug](Pug)
こちらでテンプレ作成とコンパイル設定を行っています。なにもしなくても大丈夫なように設定してあります。  
更新する際は、できるだけ Python 形式の記述でして欲しいです。  
直接コードを書くときは 特殊な場合に限って下さい。

``Slim`` もありますが、Pug は Javascript エンジン で開発されてます。  
変数とか if for が Javascript と同じ用に書けます。特に Json でデータ管理もできるので ◯ です。

Slim は拡張性とテンプレ作成に何があり、ちょっと面倒だったのでパス

閉じタグが必要ない インデント で階層が視認管理できるので最初はとっつきにくいかもしれませんが  
なれると超ラクです。閉めなくていい、``div`` は省略出来る、クラスや ID は 値だけ書けばいい。  
いろいろメリットあります。PHP でインクルードするより遥かに柔軟性があり、テンプレ化も用意。

## [SASS](SASS)
SASS です。``SCSS`` でも ``LESS`` でもないので注意して下さい。変数定義の方法が少し違います。  
Ruby 開発のメタ言語で LESS が Javascript 開発です。

ただし、LESS は変数の接頭が ``@`` で 通常の CSS ``@Media`` などのメソッドと被るのでパスしました。  
node-sass は Javascript で動いていて、なおかつ高速でコンパイルできます。  
Javascript のように記述ができますし、Json とも連携できます。

こちらもある程度、テンプレ化しているので簡単に記述できます。  
特にメディアクエリと各ブラウザ(特にIE)別に if文 と Include で簡単に記述できるようにしてあります

CSS から移行する人に わかりやすいように SASS記法ではなく、SCSS記法で記述しています。  
個人的にも慣れたら Python 形式の記述をしようと思っています。  
SCSS 記法なので ``{}`` は省略してません。また命名規則は BEM を元にしています。  
もし編集する場合は、規則を守って命名記述して下さい。Lint を今後追加で挿入する予定です。  

Google Chrome の開発だと Sourcemap がずれます。Google の対応を待ちましょう。

## Git
ファイルは ``Git`` で管理しています。ですので Git でファイルを書き換えて下さい。  
ローカルリポジトリとしてネットワークサーバー上に上げてます。  
ですので既存のリポジトリとして SourceTree など Git ソフトで追加してあげて下さい。  
管理が楽です。データを変更したら、コミットして下さい。  

# 今後の課題として、命名規則
特に画像の命名規則は必須かもしれません。

# 色々なガイドラインを読んでみる。
[Google HTML/CSS Styles Guide 日本語訳](http://buchineko.website/google_styleguide_html/)  
いずれは記述ルールとして厳格に遵守するものを作らないといけないかとは思います。

## これだけは守って守って欲しい。
CSS プロパティの書き方は自由だと思います。  
プロパティをABC順で書きましょうとかありますが、プロパティ種類でまとめて頂けたらそれでいいと思います。  
ただ、同類プロパティがバラバラになるのは避けましょう。  
margin-top と margin-bottom が離れて記述されていたりしたらそれは迷惑ですよね、  
一応、順番をある程度定義するなら外側のスタイルから記述しましょう。  
有名な mozilla.org Base Styles を参考

```css
.foo { /* Suggested order:
 * display
 * list-style
 * position
 * float
 * clear
 * width
 * height
 * margin
 * padding
 * border
 * background
 * color
 * font
 * text-decoration
 * text-align
 * vertical-align
 * white-space
 * other text
 * content
 */
}
```

ある程度プロパティでまとまりがあって順番が決まっていれば解読性の向上につながると思います。

## インデントはきれいに揃ろ!
スペース2つ分で決着が付きそうです。インデントは揃えてなおかつ スペース2つ分！

## クラスの"値"は小文字統一 ID や 変数 などは Camel Snake で統一
これは単に CSS のスタイルと JS などの ID と区別しやすくするため。  
JS などの場合、スネークケースやキャメルケールでの命名が多く、  
というか、ルール上、ハイフンが使えないなどの記述制約があるため、そうせざる負えない。  
また、解読性の向上にもつながる。

class の "値" は小文字統一、Chain ケース で統一する。または BEM を採用する。

```css
.main-contents /* Chain ケース */

/* Pascal Camel Snake で記述しない。 */
.MainContents /* Pascal ケース */
.mainContents /* Camel ケース */
.main_contents /* Snake ケース */
```

## 意味不明な略はやめる
統一された略字なら理解できるが、個別で新しい略字を作らない。

```
/* NG */
.contentL ← .contents-left
.MinCntWrp ← .main-contents-wrap

/* OK */
.nav ← .navigation
.info ← .infomation
.col ← .column
```

## なるべくクラスは使いまわす
記述が減ってみんなで使いまわせるものは使い回したほうが効率的。

CSS は基本的にルート表記が望ましい。

```css
/* NG */
main > article > section > h1 /* 子孫指定は深すぎないこと */
.main.content.title_01 /* こんなに限定した値を持たないといけない場合、設計が間違っている */

/* OK */
section > h1 /* 基本的に使いまわしができそうな指定 */
.title--foo /* いくつかタイトル装飾に候補があるなら マルチクラスを作成しておく */
```