# SASS
ここでは ``SASS`` について詳しく解説します。

拡張子は ``.scss`` にしています。SCSS 記法のためです。

解説するのは今回使用したテンプレに関わる部分のみです。  
SASS も Javascript の記法が使えるのである程度理解していると  
if文 for文 使っていろいろ捗ります。

SASS は CSS って面倒で難しいところを保管してくれるメタ言語です。  
親、子、孫、依存関係、継承･･･、メディアクエリ、ベンダープレフィックス･･･  
そんな面倒なことは気にせずシステムに任してしまおう！

[とほほのSass入門](//www.tohoho-web.com/ex/sass.html) ここが一番丁寧でわかりやすいです。

現在の SASS コンパイル指定で メディクリ と ベンダープレフィックス は自動で処理される設定になっています。

```scss
$base-width: 1024px;
$pad-width: 768px;
$tab-width: 767px;
$sp-width: 599px;

@mixin mq($breakpoint) {
  @if $breakpoint == base {
    @media screen and (max-width: $base-width) {
       @content;
    }
  }
  @else if $breakpoint == pad {
    @media screen and (max-width: $pad-width) {
      @content;
    }
  }
  @else if $breakpoint == tab {
    @media screen and (max-width: $tab-width) {
      @content;
    }
  }
  @else if $breakpoint == sp {
    @media screen and (max-width: $sp-width) {
      @content;
    }
  }
  @else if $breakpoint == ie {
    @media screen and (-ms-high-contrast: active), (-ms-high-contrast: none) {
      @content;
    }
  }
  @else if $breakpoint == firefox {
    @-moz-document url-prefix() {
      @content;
    }
  }
  @else if $breakpoint == chrome {
    @media screen and (-webkit-min-device-pixel-ratio:0) {
      @content;
    }
  }
  @else if $breakpoint == safari {
    @media screen and (-webkit-min-device-pixel-ratio:0) {
      ::i-block-chrome,
      _::-webkit-full-page-media, _:future,
      :root {
        @content;
      }
    }
  }
}

@include mq(各widthサイズ名) {
  // 変数よりブラウザが小さければ、適応
}

@include mq(各ブラウザ名) {
  // 各ブラウザ固有のメディアクエリで各ブラウザハック
}
```

これは <span style="color:BlueViolet">``@include``</span> で呼び出しができる <span style="color:BlueViolet">``@Mixin``</span> です。  
面倒なレスポンシブ化や各ブラウザ間の差異、バグの回避に大変役立ちます。

ちなみに整形するようにしているので、書き出された CSS の中身は メディアクエリできれいに並び替えられます。  
ですので、無駄な <span style="color:Magenta">``!important``</span> の回避につながります。

# Autoprefixer
自動で ペンダープレフィックス 付けてくれます。設定上 ラストバージョンより 2つ前まで対応させてます。  
オンラインでも動作してるものがあります。この機能を コンパイル時に埋め込みました。

[Autoprefixer CSS online](https://autoprefixer.github.io/)

example.scss
```scss
.example {
    display: flex;
    transition: all .5s;
    user-select: none;
    background: linear-gradient(to bottom, white, black);
}
```
example.css
```css
.example {
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-transition: all .5s;
  transition: all .5s;
  -webkit-user-select: none;
     -moz-user-select: none;
      -ms-user-select: none;
          user-select: none;
  background: -webkit-gradient(linear, left top, left bottom, from(white), to(black));
  background: linear-gradient(to bottom, white, black);
}
```

## 詳しいことは書きながら学んで欲しい
ネストして記述できるので親子関係がわかりやすくなりますが、  
多用すると、マルチクラス設計に難が出てきてしまうので、個人でそのあたりは感覚を掴んで欲しい。  

BEM や 命名方式の統一を図るためにも SASS はとっても有用です。

## BEM を導入しよう
CSS の クラス指定 の "値" についてかなり考察をしました。  

現在、[BEM](//github.com/juno/bem-methodology-ja/blob/master/definitions.md) (Block Element Modifier) が現在主流になっている。日本語訳があったので出来れば熟読していただきたい。  
これは WEB と 開発 の間で相互理解を深めた命名方式であり、親と要素と状態の区別が簡単であるという理由からなる命名規則。  
確かに、親クラスの名前が子の名前の先頭にくると階層が理解しやすい。

なお、BEM を少し拡張した MindBEMding がある。  
こちらは柔軟性に飛んでいて、命名規則の柔軟性と シングルクラス マルチクラス の柔軟性を説明している。

具体的には翻訳を読んで欲しい。CSS コーダーの視点だけでなく Javascript や JSON、XML のやり取りにもかなり配慮されていることが理解できると思う。