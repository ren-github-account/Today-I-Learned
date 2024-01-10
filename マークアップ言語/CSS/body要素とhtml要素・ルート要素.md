*目次*
* [そもそもbody要素とは](#そもそもbody要素とは)
* [html要素とbody要素の両方にbackgroundプロパティを指定した場合](#html要素とbody要素の両方にbackgroundプロパティを指定した場合)
* [とはいえ](#とはいえ)
* [body要素のみにbackgroundを指定した場合](#body要素のみにbackgroundを指定した場合)
* [関連事項](#関連事項)
* [ルート要素とは](#ルート要素とは)
* [displayプロパティとは](#displayプロパティとは)
* [ブロック要素](#ブロック要素)
* [参考](#参考)

### そもそもbody要素とは

実際にブラウザの画面上に表示される内容を指定するための要素。

body要素のコンテンツ領域の大きさは、その表示される量によって可変(大きくなったり小さくなったり)する。

例えば以下のコードのように、p要素を二つ`<body>`内に記述すると、body要素のコンテンツ領域の大きさは、そのp要素二つ分の大きさとなる。

(具体的には、以下のコードの場合、コンテンツ領域の高さは、**p要素のコンテンツ領域の高さ + 間に挟まるp要素のデフォルトCSSのマージン1個分 + p要素のコンテンツ領域の高さ**となる。chromブラウザで確認したところ、コンテンツ領域の高さは24+16+24=64となった。)

```
<body>
<p class="blue">うんちだよほんと</p>
<p>ほんとうんち</p>
</body>
```

### html要素とbody要素の両方にbackgroundプロパティを指定した場合

上記のbody要素の意味をふまえた上で、html要素とbody要素の両方にbackgroundプロパティを指定すると面白いことが起きる。

```
<style>

html{
 background:gray;
}

body{
 background:orange;
}

</style>

<body>
<p>うんちだよほんと</p>
<p>ほんとうんち</p>
</body>
```

上記のコードを実行してみると、p要素二行分の大きさのbody要素がオレンジになり、その背景がグレーとなる。

このようにhtml要素にも背景を設定すると、背景が二色存在するというあまり見られない装飾を行うことができる。

### とはいえ

ここまででbody要素だけではなく、html要素にもbackgroundプロパティを使うことができることがわかった。

しかし仕様書によると、この**html要素にbackgroundプロパティを指定するやり方は非推奨**のようだ。

実際に仕様書CSS2.1の14.2節に以下のように書かれている。

>For HTML documents, however, we recommend that authors specify the background for the BODY element rather than the HTML element.
>
>ただし、HTML文書の場合は、作成者が HTML要素ではなく BODY要素のbackgroundを指定することをお勧めします。

やはり、**backgroundプロパティは、body要素に指定する**のがよさそうだ。

### body要素のみにbackgroundを指定した場合

通常、背景を指定するときはbody要素に指定するが、この場合html要素のbackgroundプロパティはどうなっているのだろうか。

この場合は、**body要素のbackgroundプロパティの設定が、html要素にも適用されることになる。**  
CSS2.1の仕様にこのように定められているようだ。

具体的には仕様書「CSS Backgrounds and Borders Module Level 3」の2.11.2節に以下のように書かれている。

>2.11.2. The Canvas Background and the HTML <body> Element
>
>For documents whose root element is an HTML HTML element or an XHTML html element [HTML]:
>
>if the computed value of background-image on the root element is none and its background-color is transparent,
>
>user agents must instead propagate the computed values of the background properties from that element’s first HTML BODY or XHTML body child element.
>
>The used values of that BODY element’s background properties are their initial values,
>
>and the propagated values are treated as if they were specified on the root element.
>
>2.11.2. Canvas の背景と HTML の <body> 要素
>
>ルート要素が HTML HTML 要素または XHTML html 要素であるドキュメントの場合 [HTML]:
>
>ルート要素のbackground-imageの計算値が none で、そのbackground-colorがtransparentの場合、
>
>ユーザー エージェントは代わりに、その要素の最初の HTML BODY または XHTML body 子要素から背景プロパティの計算値を伝播する必要があります。
>
>その BODY 要素の背景プロパティで使用される値は初期値であり、伝播された値はルート要素で指定されたものとして扱われます。

`background-image:none;`と`background-color:transparent`はともに初期値。

ともに初期値なので「ルート要素のbackground-imageの計算値が none で、そのbackground-colorがtransparentの場合、」の意味は、背景画像と背景色に何も設定されていないデフォルトの状態という意味。

つまり、**html要素の背景色と背景画像に何も設定されていない場合、body要素のbackgroundプロパティの値がルート要素で指定されたものとして扱われることになる。**(ルート要素の意味については関連事項を参照)

### 関連事項
#### ルート要素とは

HTML文書はHTML要素を頂点とするツリー状の構造になっている。

頂点に位置する要素のことを **ルート要素**と言い、HTML文書の場合の**ルート要素はHTML要素となる。**

#### displayプロパティとは

要素の特徴を変更するプロパティのこと。種類によってwidthやheight、marginやpaddingなどの指定が無効になったり、要素の折り返しに影響がでる。 

#### ブロック要素

displayプロパティの値が`block`の要素群の昔の呼び方。ブロックレベル要素やブロックレベルの要素とも呼ばれる。

具体的には、以下がブロック要素の代表例。

```
・p要素
・div要素
・h1などの見出し要素
```

### 参考

body要素とhtml要素の違いを理解するにあたって以下のページが参考になった。

(https://qiita.com/kazhashimoto/items/9a097d07626adc9eccb8)

(https://qiita.com/nekotadon/items/f5b11d6a8e13ec1c6e53)

(https://detail.chiebukuro.yahoo.co.jp/qa/question_detail/q12161330442)




