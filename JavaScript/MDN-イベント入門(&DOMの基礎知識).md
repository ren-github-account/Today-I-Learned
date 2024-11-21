*目次*
* [概要](#概要)
* [イベントとは](#イベントとは)
* [イベントハンドラーとは](#イベントハンドラーとは)
* [イベントリスナーの使い方](#イベントリスナーの使い方)
* [querySelectorメソッド](#querySelectorメソッド)
* [DOMの基礎知識](#DOMの基礎知識)
* [DOMとインターフェイスの関係](#DOMとインターフェイスの関係)
* [CSSStyleSheetとは](#CSSStyleSheetとは)
* [CSSStyleSheetを取得](#CSSStyleSheetを取得)
* [CSSRule](#CSSRule)
* [エラー対処](#エラー対処)
* [リスナーを削除する](#リスナーを削除する)
* [複数のハンドラーを追加する](#複数のハンドラーを追加する)

### 概要

本ページは以下の記事の内容を自分なりにまとめたものである。

https://developer.mozilla.org/ja/docs/Learn/JavaScript/Building_blocks/Events

### イベントとは

イベントが発生すると、システムは何らかのシグナルを発生させ(これを**発火**という)、何らかのコードが実行されるメカニズムを提供する。
```
イベントが発生すると、システムは何らかのシグナルを発生（または「発火」）させ、
イベントが発生したときに自動的にアクションが実行される（つまり、何らかのコードが実行される）メカニズムを提供します。
```

イベントの具体例として以下のものがある。

```
・ユーザーが特定の要素を選択したり、クリックしたり、カーソルを当てたりする

・ユーザーがキーボードのキーを押す

・ユーザーがブラウザーウィンドウをリサイズしたり閉じたりする

・ウェブページの読み込みが完了する

・フォームが送信される

・動画が再生される、停止される、再生が終わる

・エラーが発生する
```

### イベントハンドラーとは

イベントハンドラーとは、**イベントが発生した時に実行するコードのまとまり(通常はJavaScript関数)のこと。**

イベントハンドラーがイベントに反応して実行するように設定されていることを、**イベントハンドラーを登録**していると言う。

豆知識:ハンドラーは英語でhandlerと書き、その接尾辞の「er」は「～する人・もの」を表す。(出典:龍谷大学 接尾辞-erと-eeについて) 英語でhandleには「操作」という意味や「制御される部分」といった意味があるので、「制御されるもの、あるいは操作するもの」という意味になる。  
つまり、イベントハンドラーは「イベントを操作するもの」となる。まあ、要は「機能を付与する」ってことだね。

またイベントハンドラーは、**イベントリスナー**と呼ばれることもある。

**イベントハンドラー**と**イベントリスナー**は厳密に言えばちがうものでその違いは以下の表にまとめた。

||違い|
|-|-|
|イベントリスナー|イベントの発生に耳を傾ける。(つまりperceive(知覚する)的な意味)|
|イベントハンドラー|発生したイベントに応答して動作するコードのこと。|

Listenの意味については、[この](https://www.grammar-monster.com/easily_confused/hear_listen.htm)サイトが図解付きでわかりやすかった。

### イベントリスナーの使い方

イベントハンドラーを追加するには、`addEventListener()メソッド`を使用する。

`addEventListener()`の使い方をググると、以下のコードのように`addEventListener()`内に直接、関数の処理を記述しているパターンをよく見かける。

**直接記述パターン**
```
const btn = document.querySelector("button");

function random(number) {
  return Math.floor(Math.random() * (number + 1));
}

btn.addEventListener("click", () => {
  const rndCol = `rgb(${random(255)} ${random(255)} ${random(255)})`; /* 上で定義したrandom関数を参照してはいるが、
　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　関数の処理をaddEventListener()内に直接記述している
  document.body.style.backgroundColor = rndCol;
});
```

しかし、上記の直接記述パターンは個人的に**見づらい**と感じる。そういう時は以下のように、**関数名のみを記述するやり方でもOK。**

**関数名のみ記述パターン**
```
const btn = document.querySelector("button");

function random(number) {
  return Math.floor(Math.random() * (number + 1));
}

function changeBackground() {
  const rndCol = `rgb(${random(255)} ${random(255)} ${random(255)})`;
  document.body.style.backgroundColor = rndCol;
}

btn.addEventListener("click", changeBackground); /* 関数名のみを記述している。ここで使用する関数は上で定義している。
```

### querySelectorメソッド

`addEventListener()`と一緒によく使われるメソッド。イメージとしてはDOMツリー内をCSSセレクターで**検索**できるみたいな感じ。

英語でqueryには **「質問する」** といった意味がある。つまり、querySelectorメソッドは **「質問を投げかけ目的の情報を引っ張り出す」** といったイメージがしっくりくる。

**構文**
```
querySelector(selectors)
```

 querySelectorメソッドで指定できるselectorの部分には、CSSでプロパティを設定する時に使うセレクターが指定できるが、**結合子などと組み合わせることで複雑な条件付きの検索が可能となる。** ( 参照:[こちら](https://swfz.hatenablog.com/entry/2023/07/15/183739) )

**使い方**

以下の例は、class名に"myclass" を持つ文書内の要素の内、**最初のものを返す。**
```
const el = document.querySelector(".myclass");
```
**最初のものを返す**という点に注意。なのでHTMLに以下ように書かれていた場合、「💩でごんすごんす」とかかれた要素のみが戻り値として返される。

```
<body>
   <div class="myclass">💩でごんすごんす</div>  
   <div class="myclass">うへへ</div>
</body>
```

**・豆知識**:CSSのセレクターの部分を以下のように`.myclass`のみ記述しても反映される。( 参照:[ドットでつなげるやつ](https://github.com/ren-github-account/Today-I-Learned/blob/main/%E3%83%9E%E3%83%BC%E3%82%AF%E3%82%A2%E3%83%83%E3%83%97%E8%A8%80%E8%AA%9E/CSS/%E3%83%89%E3%83%83%E3%83%88%E3%81%A7%E3%81%A4%E3%81%AA%E3%81%92%E3%82%8B%E3%82%84%E3%81%A4.md) )

```
.myclass {
 background:#fffaf0;
}
```
**戻り値**

querySelectorメソッドの**戻り値**は、「指定された**CSSセレクター**のset(まとまり)に一致するオブジェクトのうち、ドキュメント内の最初にある(要素を描画している)**Elementオブジェクト**を返す。一致するものがない場合は`null`を返す」。  

この`null`を返すという特性から、**if文と組み合わせて判定に使うことができる。**( 参照:[こちら](https://coding-memo.work/javascript/988/) )

### DOMの基礎知識
上記の説明文でわからない単語が2つ出てきたので以下の表にまとめる。(2つの単語以外にもその前提となる単語をまとめている)

**追記** (2024.11.19) 

以下で、DOMの細かな仕様についてあれこれ見ていく。それぞれの関係性がややこしく理解に手間取ったwが、やっと自分なりにまとめることができたので、最初に大枠をつかむ意味においても、以下にそのまとめを書いておく。

**【最終まとめ:DOMのインターフェイスとオブジェクトの関係】**

まずDOMでは、**HTML文書内の要素やテキスト**をそれぞれ**オブジェクト**として扱うが、**このオブジェクト自体にオブジェクトを操作するために必要なプロパティやメソッドそしてイベントが組み込まれている。**

そして、このプロパティやメソッド、イベントは、**インターフェイスと呼ばれる仕様によって定められている。**

||意味|
|-|-|
|Element|Elementオブジェクトを理解するには、そもそもElementとは何かを理解する必要がある。Elementとは、Document内のすべての要素オブジェクト (つまり、要素を描画するオブジェクト) がinherit(継承する)最も一般的で**ベース(基礎)となるクラスのこと。**(クラスについて詳しくは[こちら](https://github.com/ren-github-account/Today-I-Learned/blob/main/JavaScript/MDN-%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88%E6%8C%87%E5%90%91%E3%83%97%E3%83%AD%E3%82%B0%E3%83%A9%E3%83%9F%E3%83%B3%E3%82%B0(%E3%82%AF%E3%83%A9%E3%82%B9%E3%81%A8%E3%82%A4%E3%83%B3%E3%82%B9%E3%82%BF%E3%83%B3%E3%82%B9%E3%81%AA%E3%81%A9).md#%E3%82%AF%E3%83%A9%E3%82%B9%E3%81%A8%E3%82%A4%E3%83%B3%E3%82%B9%E3%82%BF%E3%83%B3%E3%82%B9%E3%81%A8%E3%81%AF)と[こちら](https://github.com/ren-github-account/Today-I-Learned/blob/main/JavaScript/MDN-%E3%82%AF%E3%83%A9%E3%82%B9(%E5%8F%A4%E5%85%B8%E7%9A%84OOP%E9%A2%A8).md)を参照) ただ、通常Elementといった場合、Elementクラスのインスタンスプロパティである**子要素やclassnameやidのことを指す**場合も多い。(参照:[こちら](https://qiita.com/tomokichi_ruby/items/c3ed6f6edbd5078ddf70) )   **Elementインターフェイスは、Nodeインターフェイスから継承される。** ElementインターフェイスとNodeインターフェイスの2つを組み合わせることで、個々の要素で使用するメソッドとプロパティの多くを提供する。参照:[DOMの紹介](https://developer.mozilla.org/ja/docs/Web/API/Document_Object_Model/Introduction)|
|Document|Documentは**ブラウザで読み込まれたwebページを表す。** そしてDocumentはDOMツリーであるウェブページのコンテンツへの**入口**としての役割を果たす。**DOMツリーとは**ブラウザがHTML文書を解釈する時に構築するものだが、**DocumentはこのDOMツリーの一番上に位置する。**(参照:[DOMツリーのイメージ図](https://developer.mozilla.org/ja/docs/Web/API/Document_Object_Model/Using_the_Document_Object_Model#dom_%e3%83%84%e3%83%aa%e3%83%bc%e3%81%a8%e3%81%af%ef%bc%9f))つまり、平たくいえば、**DocumentはDOMツリーへの入り口を意味する。**|
|Elementオブジェクト|上述のElementクラスを使って作成されたオブジェクトのこと。Elementオブジェクトのプロパティの具体例には**innerHTMLプロパティ**などがある。その他にも**id**や**classname**や**子要素**などがある。詳しくは[こちら](https://developer.mozilla.org/ja/docs/Web/API/Element/innerHTML)|
|DOM| DOMとは、HTMLまたはXMLなどのマークアップ言語の文書を表現・操作するための**API**のこと。**HTMLなどは、一旦ブラウザでノードツリー(DOMツリー)として読み込まれ、その読み込まれたそれぞれのノードがブラウザ上で描画され、僕たちが普段目にするwebページの形を成す。** |
|HTML DOM|まずDOMという大きなくくりがあって、さらにその中に**HTMLを操作・描画するための「HTML DOM」が存在する。** HTMLが書かれた文書は、DOMの中の**Documentインターフェイス**を使用して表現される。より具体的には、**Documentインターフェイスのインスタンス(具体例:bodyなどがある)** で表現される。HTML DOMで定義されているインターフェイスとその継承関係について詳しくは[こちら(図解付き)](https://developer.mozilla.org/ja/docs/Web/API/HTML_DOM_API)、DOMとHTML DOMの関係については[こちら](https://developer.mozilla.org/ja/docs/Web/API/Document_Object_Model#html_dom)。|
|オブジェクト|「HTML DOM」関係のMDNの解説ページに、よく**オブジェクト**という言葉が登場する。この文脈におけるオブジェクトは、**HTML文書内の要素やテキストのこと**を表している。ノードとの関係性でいうと、この**オブジェクト(HTML文書内の要素やテキスト)を、その内部に保存している**のが**ノード**ということになる。各ノードは、その**ノードに関する情報を取得するためのプロパティ**と、DOM内で**ノードを作成、削除、整理するためのメソッド**を提供する**Nodeインターフェイスに基づいている。** つまり、DOMを使って、ノード内の要素やテキストを操作できるのはこの**Nodeインターフェイスのおかげ**ということ。参照1:[HTML文書の構造](https://developer.mozilla.org/ja/docs/Web/API/HTML_DOM_API)。参照2:[オブジェクトとノード](https://www.javadrive.jp/javascript/dom/index1.html)。もっといえば、MDNのページに　**「DOMは文書をノードとオブジェクトで表現します。」** ( 参照:[DOM の紹介](https://developer.mozilla.org/ja/docs/Web/API/Document_Object_Model/Introduction) )とあるように、HTMLの要素達がブラウザで解釈される時まずDOMツリーに変換される。DOMツリー内では、各要素や、その要素の中に含まれるテキストデータは、**それぞれ、要素ノードやテキストノードと呼ばれるノード内にそれぞれ入れられる。** 要するに、DOMではHTML文書内の**要素やテキストをオブジェクトとして扱うが、その各オブジェクトはDOMツリー内ではノード**と呼ばれる。|
|Documentオブジェクト|**その文書自身を表すオブジェクト。** HTML文書でいえば、今扱っているそのHTML文書自体を表す。そして、**Webページの操作と作成に使用できるすべてのプロパティ、メソッド、イベントは、それぞれのオブジェクトに組み込まれている。** ( 参照:[DOMの紹介](https://developer.mozilla.org/ja/docs/Web/API/Document_Object_Model/Introduction) ) 例えば今説明した**Documentオブジェクトがどのようなプロパティやメソッド、イベントを持っているか**はMDNのDocumentについての解説ページで見ることができる。参照:[Document](https://developer.mozilla.org/ja/docs/Web/API/Document)。まとめると、DOMではHTML文書内の要素やテキストをそれぞれオブジェクトとして扱う。そして、このそれぞれのオブジェクトをDOMで操作できるのは、オブジェクト自体に操作するためのプロパティ、メソッド、イベントが組み込まれているからということになる。|
|Node(ノード)|Nodeには大きく2つの意味があって、1つは**DOMとしてのNode**でもう1つは**クラスとしてのNode**になる。**DOMとしてのNodeはノードツリー内のある一点を表す。(具体的には、HTMLの要素やテキストなどが該当する)** **このことからDOMとしてのNodeは、ブラウザがHTMLを解釈する時に作られるものと言える。** |
|クラスとしてのNode|**DOMNodeインターフェイス**と呼ばれ、他の多くの**DOM APIオブジェクトのベースとなる抽象的な基本クラス。** このNodeインターフェイスがあるおかげで、このNodeインターフェイスをベースに持つオブジェクト達を同じように扱うことが可能となる。**Nodeインターフェイスの子インターフェイスには、Documentインターフェイス, Elementインターフェイス, DocumentFragmentインターフェイスがある。** 今述べた3つのインターフェイスは、**Nodeインターフェイスを親クラスとするサブクラスでもあるため、これらのインターフェイスに属するオブジェクトは、Nodeインターフェイスの機能も継承している。** その他の重要なインターフェースとして**Windowインターフェイス**がある|
|Windowインターフェイス| DOM文書を収める**ウィンドウ**を表す。 documentプロパティは、そのウィンドウに読み込まれたDOMのdocumentオブジェクト(例:bodyなど)を指す。Windowインターフェイスは、厳密に言えば**ユーザーインターフェイスのウィンドウの概念とは必ずしも直接関連づかない**点に注意。|
|EventTargetインターフェース|EventTargetインターフェースは、**イベントを受信でき、イベントのリスナーを持つことができるオブジェクトによって実装される。** (**具体的には、Elementやその子、Documentオブジェクトなど**) つまり、イベントのターゲットとなるものはすべて、このインターフェースに関連付けられた3つのメソッド(`addEventListener()`,`removeEventListener()`,`dispatchEvent()`)を持つ。**イベントのターゲットとなるものの具体例としては、Elementやその子要素、そしてDocumentオブジェクト と Windowオブジェクトなどがある。** documentオブジェクトとwindowオブジェクトはそれぞれ、DocumentインターフェイスとWindowインターフェイスを実装する。つまりもっと具体的に言えば、**Elementやその子要素、加えてDocumentオブジェクトやWindowオブジェクトなどに対して`addEventListener()`を使用することができる**ってことだな。|
|innerHTMLプロパティ|HTML内にあるテキストや`<p></p>`などのタグそれ自体などを置き換えたり取得したりできる機能を持つ。(参照:[innerHTMLの使い方](http://javascriptmania.blog111.fc2.com/blog-entry-18.html)) **innerHTMLで値を代入すると、その値として与えられたHTMLを解釈して構築されたノードに置き換える。** **注意点**としてinnerHTMLはXSS防止の観点から非推奨で、**代わりに`textContentプロパティ`の使用が推奨される。**|
|Serialization (シリアライズ)|オブジェクトやデータ構造が転送に適したフォーマットに変換されること。(参照:MDN用語集より)|
|CSSセレクター|CSSを使って**どのHTML要素に変更を加えるかをブラウザーに伝えるもの。** ここで選択された要素にはCSSプロパティ値 (property value)が適用される。参照:[CSSセレクター](https://developer.mozilla.org/ja/docs/Learn/CSS/Building_blocks/Selectors)|

### DOMとインターフェイスの関係

上記でDOMとそのインターフェイスについて細かな仕様を見てきた。

ここでDOMとインターフェイスについての自分なりの理解をまとめてみる。

まずDOMという大きなくくりがあって、その中にインターフェイスと呼ばれる仕様があれこれ定められている。DOMはAPIであるため、そのインターフェイスで定められている中に、HTMLのbodyだとかその他の各要素を**操作・描画**するための色々なAPIの機能が存在する。その**操作・描画**するための具体的な機能がDocumentオブジェクトやWindowオブジェクトに**組み込まれているプロパティ、メソッド、イベントである。**(詳しくは上述の表のDocumentオブジェクトの部分を参照)

図式にすると以下になる。

**DOM->インターフェイス->DocumentオブジェクトやWindowオブジェクトに組み込まれているプロパティ、メソッド、イベント**

つまり、各インターフェイスによって定められているのが、プロパティやメソッドやイベントということになる。

### CSSStyleSheetとは

まず前提として**CSSを構成する要素はDOMでは全てオブジェクト**で表される。

その**DOM持つオブジェクトの一つがCSSStyleSheet**となる。

CSSStyleSheetは、CSSそのもので最も大きなまとまりを表す。具体的には、**CSSファイルに記述されたCSS全体(HTMLに直接記述している場合は、`<style>～</style>`の間の全て)** を表している。

もし、**`<style>〜</style>`が2つ以上あった場合、それぞれのstyle要素ごとにCSSStyleSheetが存在する。** 

つまり、CSSファイルの数だけ、もしくは`<style>`タグの数の分だけCSSStyleSheetは存在するってことだな。

### CSSStyleSheetを取得

それでは、このCSSが入っているCSSStyleSheetを取得するにはどうすればよいか。

それには、**documentオブジェクトが持つstyleSheetsプロパティ**を使用することで取得できる。

**使い方**は以下のように書く。**注意点**として、**末尾の`[0]`は一番最初の最初のCSSStyleSheetのことを表しており、付けないとエラーになる。**

```
document.styleSheets[0]

/* 一方、以下のように末尾に添字を書かないで実行しようとしたらエラーが出た  */
document.stylesheets
```

### CSSRule

上述のCSSStyleSheetは、**CSSファイルに記述されたCSS全体(HTMLに直接記述している場合は、`<style>～</style>`の間の全て)のこと**を表すのだった。

そのCSSStyleSheetはさらに、**CSS内のセレクタごとに細かく分けることができる。**

例えば、以下のCSSがあったとする。

```
.myclass {
 background:#fffaf0;
}

div.change-color {
 background:#7fffd4;
 width:200px;
 height:200px;
}
```

上記のコード内でセレクタを数えてみると、`.myclass `と`div.change-color`という2つのセレクタが存在する。

今述べた**それぞれのセレクタは、CSSStyleSheetの持つCSSRulesというプロパティによって管理されており、各セレクタごとに添字(例:`[0]`)が付与されている。** ( 参照:[CSSの構造](https://uhyohyo.net/javascript/5_2.html) )

例えば上記のコードでいえば、一番目のセレクタ`.myclass`は、`[0]`の添字を持つCSSRulesプロパティに格納されており、`div.change-color`は`[1]`といった具合になる。

**使い方**

CSSRulesを実際に使うには、**変数と組み合わせて以下のように使う。** つまづきポイントしては、CSSRulesを直接使うのではなく、変数と組み合わせて **変数[添字]** という形で使うことに最初は違和感を覚えるかもしれないが使っていくうちに慣れてくる。

あと解説では**CSSRules**と大文字が使われているが、実際にコードを書く時は**cssRules**と「CSS」の部分は小文字で書いてok。というか今試してみたら**小文字で書かないとエラーになる。**

```
let stylesheetInfo = document.styleSheets[0].cssRules;
stylesheetInfo[1].style.backgroundColor = "#00bfff";　/* 変数stylesheetInfo[1]とすることで、
　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　上述のCSSコードでいえばセレクタ`div.change-color`のCSSを呼び出している
```

上記のコードのうち **`style.backgroundColor`** となっている部分の解説をしておくと、この部分は、**DOMのstyleオブジェクトのbackgroundColorプロパティを指定している。**

**styleオブジェクトとは、CSSを扱うことができるオブジェクトのこと。**

**backgroundColorプロパティ**は、そのstyleオブジェクトが持つ**背景色を指定することができるプロパティ**となる。参照:[styleオブジェクトのプロパティ一覧](http://alphasis.info/javascript/dom/styleobject/)

### エラー対処

ローカル環境(何もせずにPC内のhtmlファイルをダブルクリックするやり方)で、以下のCSSStyleSheetオブジェクトのCSSRulesプロパティを含んだコードを実行しようとしたら出た。
```
 const stylesheetInfo = document.styleSheets[0].cssRules; /* エラーメッセージによると末尾のcssRulesの部分で引っかかっていた
 stylesheetInfo[1].style.backgroundColor = "#00bfff";
```

エラーメッセージを読むと、**セキュリティエラー**と書いてある。

**対処法**

XAMPPでサーバーを立ててそのサーバー上で実行するとエラーが回避できた。

**エラーメッセージ**
```
Uncaught SecurityError: Failed to read the 'cssRules' property from 'CSSStyleSheet': Cannot access rules

【翻訳】
キャッチされないセキュリティ エラー: 'CSSStyleSheet' から 'cssRules' プロパティを読み取ることができませんでした: ルールにアクセスできません
```

参考
[Google Chromeはローカル環境でlinkタグのスタイルシートのcssRulesにアクセスできなかった](https://qiita.com/querykuma/items/930b20758b06c31d2af5)


### リスナーを削除する

`addEventListener()`を使ってイベントハンドラーを追加した場合、`removeEventListener()メソッド`を使って再び除去することができる。

`removeEventListener()メソッド`で除去するには以下のコードを書く。

```
要素名.removeEventListener("イベントハンドラープロパティ名(例:clickなど)", 関数名);
```

このリスナーの除去は、**大規模で複雑なプログラムを書く時に役立つ。**

```
単純で小さなプログラムでは、古くて使われていないイベントハンドラーをクリーンアップする必要はありませんが、
大規模で複雑なプログラムでは、効率を向上させることができます。
また、イベントハンドラーを除去する機能により、同じボタンが異なる状況で異なるアクションをするようなことも可能です。
ハンドラーを追加したり除去されたりするだけです。
```

### 複数のハンドラーを追加する

以下のように書くことで、一つのイベントにたいして複数のハンドラーを持たせることができる。

```
myElement.addEventListener("click", functionA);
myElement.addEventListener("click", functionB);
```

長くなったのでその2へ続く。
