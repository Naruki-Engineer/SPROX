# Pug
ここでは ``Pug`` について解説します。  
解説するのは今回使用したテンプレに関わる部分のみです。  
Javascript を理解できているなら拡張性はかなりあります。

今回は Json からデータを引っ張ってくるようにこちらで設定してあります。

詳しくはこのページより、公式チュートリアルを読んだり、ググると多少出てきます。  
情報は英語のほうが圧倒的に多いので、日本語で探すのは多少難しいかもです。

# Pug の書き方
Python 形式というか インデント を判断して記述していく テンプレートエンジンです。  
ですので、きれいに記述できるし、階層の書き間違えがあればエラーで知らせてくれます。  
必然的にコードの解読性と保守性が維持できます。

.pug
```pug
doctype html
html(lang="ja")
  head
    title タイトル
  body
    header
      nav
        ul: li ホーム
            li コンテンツ
            li その他
    main
      article
        h1 記事タイトル
        p.
          記事内容
          タグの末尾に . をつけると、
          それ以降は Pug の記述制約の影響を受けなくなります。
        section
          h2 見出し
          p 中身
      aside
        nav
          | サイドメニュー
          | このように | をつかうと Pug の制約を受けずに記述できます。
    footer コピーライト
```
.html
```html
<!DOCTYPE html>
<html lang="ja">
  <head>
    <title>タイトル</title>
  </head>
  <body>
    <header>
      <nav>
        <ul>
          <li>ホーム</li>
          <li>コンテンツ</li>
          <li>その他</li>
        </ul>
      </nav>
    </header>
    <main>
      <article>
        <h1>記事タイトル</h1>
        <p>
          記事内容
          タグの末尾に . をつけると、
          それ以降は Pug の記述制約の影響を受けなくなります。
        </p>
        <section>
          <h2>見出し</h2>
          <p>中身</p>
        </section>
      </article>
      <aside>
        <nav>
          サイドメニュー
          このように | をつかうと Pug の制約を受けずに記述できます。
        </nav>
      </aside>
    </main>
    <footer>コピーライト</footer>
  </body>
</html>
```

このように 入れ子 スタイルで記述します。閉めタグが必要ない、インデントで階層が即座にわかる、など  
コードの見やすさと、コンパイル時、整形するようにしてあるので誰でも同じような記述ができます。

## テンプレートファイル
ここには共通のページで使うテンプレートの設定がしてあります。  
``include`` は PHP などと同じものです。  

多少理解しづらいのは ``block`` という Pug 固有の機能です。  
これは、各ページ個別のものを読み込む場合のもので Block の中身は各々のページで設定します。

layout.pug
```pug
include ../_config
block vars

doctype html
html(lang="ja")
  head
    include ../include/_head
    block script

  body

    .layout

      .layout__container
        include ../include/_header-nav

        main.layout__content
          block main

          include ../include/_footer

    include ../include/_script

```

## テンプレートを呼び出す
テンプレートがある程度出来上がったらそれを呼び出して使用しなければ意味がありません。  
``extends`` で呼び出しができます。

index.pug
```pug
extends template/_layout

block vars
  - var key = "index"
  - var page = page_data[key]

block script
  script.
    window.onload=function(){
      bodyChange()
    }
    function bodyChange() {
      dd = new Date();
      hh = dd.getHours();
      var tag=document.getElementById("time__style");
      if (6 < hh && hh < 18) {
        tag.className="layout__container day-time"
      }
      else {
        tag.className="layout__container night-time"
      }
    }
block main
  //ここからコンテンツ
```

基本的に ``extends`` と ``block`` しか書いてないのがわかると思います。  
block 名前 と書けば、テンプレと同じ場所にその中身が反映されるということです。  

つまり、extends でテンプレを呼び出せば、Block の中身を書いてあげればいいだけ！  
各ページに無駄なものを書かなくてすむのは最高に見通しがよくメンテナンス性があります。  
PHP になんて戻れない開放感があります。いちいち 各ページにインクルード書いていく必要もなし！

最初は ？ かもしれませんが、Pug を動かしなれると、本当に汎用性が高いテンプレが作れることに気づくはずです。

## 変数で管理
上記でもあった ``block vars`` というもののなかで ``- var key = "index"`` とか変数を宛ててます。  
これは テンプレファイルで呼んでいる config.pug の中身を呼び出したり、ページ固有の変数に書き換えてます。

詳しくは説明しません。Javascript の変数と全く一緒で呼び出しているにすぎません。

ちなみに Json も呼び出しています。その場合、``#{data.jsonFileName}`` で指定の Json を呼び出せます。
