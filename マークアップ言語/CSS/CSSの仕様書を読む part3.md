前回のpart2では、ブロックレベル要素とインラインレベル要素についてを主に見てきた。

ブロックレベル要素とは、displayプロパティの値が`block`, `table`などの要素のことで、インラインレベル要素とは、displayプロパティの値が`inline`, `inline-block`などの要素のことだった。

そして、ブロックレベル要素はブロックレベルボックスを、インラインレベル要素はインラインレベルボックスをそれぞれ生成する。

さらに、その生成されるブロックレベルボックスとインラインレベルボックスは、それぞれブロックフォーマットコンテキストとインラインフォーマットコンテキストと同じ動きをするのだった。

ここでブロックフォーマットコンテキストとインラインフォーマットコンテキストとは何か？という疑問が湧いてくる。

*目次*
* [ブロックフォーマットコンテキストとは](#ブロックフォーマットコンテキストとは)
* [ブロックフォーマットコンテキストが確立されるとどうなる？](#ブロックフォーマットコンテキストが確立されると)
* [①body要素内にブロックレベル要素を配置](#普通にbody要素内にブロックレベル要素を配置)
* [②body要素内で新しくブロックフォーマットコンテキストを確立](#新しくブロックフォーマットコンテキストを確立)
* [インラインフォーマットコンテキストとは](#インラインフォーマットコンテキストとは)
* [ラインボックスの用途](#ラインボックスの用途)
* [displayプロパティのinlineblockの仕様](#displayプロパティのinlineblockの仕様)
* [inlineblock](#inlineblock)
* [非置換要素とは](#非置換要素とは)
* [あらためてプロパティinlineblockを見てみる](#あらためてプロパティinlineblockを見てみる)
* [marginの相殺](#marginの相殺)
* [widthとheightと非置換の関係](#widthとheightと非置換の関係)


### ブロックフォーマットコンテキストとは

仕様書CSS2.1には以下のように書かれている。

>Floats, absolutely positioned elements, block containers (*1) that are not block boxes, and block boxes with 'overflow' other than 'visible' (*2) establish new block formatting contexts for their contents.
>
>//文の流れをわかりやすくするため、いったん()内の記述は省略した。
>
>フロート、絶対配置要素、ブロック ボックスではないブロック コンテナ (*1)、および 'visible' 以外の 'overflow' を持つブロック ボックス (*2) は、そのcontentsに対して新しいブロック フォーマット コンテキストを確立します。
>
>//以下は省略した()内の記述
>
>*1: such as inline-blocks, table-cells, and table-captions
>
>*2: except when that value has been propagated to the viewport
>
>*1: インラインブロック、表セル、表キャプションなど
>
>*2: ただし、その値がビューポートに伝播されている場合は除きます。

つまり、以下のプロパティなどを持つ要素の生成するボックスは、その**contents**に対して**新しいブロックフォーマットコンテキストを確立する。**

||種類|
|-|-|
|①|float|
|②|absolutely positioned elements(絶対配置された要素)|
|③|ブロックボックスではないブロックコンテナ。例:inline-blocks, table-cells, and table-captionsなど。|
|④|overflowプロパティの値がvisible(初期値)以外のブロックボックス。つまり、`overflow:visible`の時はブロックフォーマットコンテキストを確立しない。|

**④の補足**

「overflowプロパティの値がvisible(初期値)以外の**ブロックボックス**。」とある。

ブロックボックスとは、ブロックコンテナボックスでもあるブロックレベルボックスのことだった。

ブロックボックスの具体例はpart2でも見てきた通り、例えば以下になる。

```
<div id="parent">
  <p id="child">子ブロック</p>
</div>
```

上記の場合のように、ブロックレベルボックスの中に、別の要素が含まれている時にそのブロックレベルボックスは**ブロックボックスとなる。**



続き。

>In a block formatting context, boxes are laid out one after the other, vertically, beginning at the top of a containing block.
>
>The vertical distance between two sibling boxes is determined by the 'margin' properties.
>
>Vertical margins between adjacent block-level boxes in a block formatting context collapse.
>
>ブロックフォーマットコンテキストの内部で、ボックスは、containing blockの上部から始めて垂直方向に次々にレイアウトされます。
>
> 2 つの兄弟ボックス間の垂直距離は、「マージン」プロパティによって決まります。
>
>ブロックフォーマットコンテキスト内の隣接するブロックレベルボックス間の垂直方向のマージンは相殺します。

**ブロックフォーマットコンテキスト内では以下の動きをする。**

・ボックスは包含ブロックの上部から初めて縦に次々に配置されていく。(包含ブロックについてはpart1-2を参照)

・2つのsibling boxes間の垂直距離、つまり共通の親要素を持つボックスの縦の距離は「マージン」によって決まる。(siblingには「共通の親を持つ兄弟」といった意味がある)

・上下に隣あっているボックス間のマージンが重なっている場合、相殺が働く。

### ブロックフォーマットコンテキストが確立されると

それでは、このブロックフォーマットコンテキストが**新しく確立されると**どうなるのか？

結論から書くと、**そのcontent領域内で新しくブロックフォーマットコンテキストの動きをする。**

これだけだとわかりにくいので具体例を見てみる。

新しくブロックフォーマットコンテキストを確立することがどういうことかをわかりやすくするために、以下の2パターンに分けて比較しながら見ていくことにする。

**①普通にbody要素内にブロックレベル要素を配置した場合**

**②body要素内で「新しくブロックフォーマットコンテキストを確立した」場合**

#### 普通にbody要素内にブロックレベル要素を配置

まず最初に「①普通にbody要素内にブロックレベル要素を配置した場合」を見ていく。

以下のコードを書いたとする。

```
<!--body-->
<body>
<p class="one"></p>
<p class="two"></p>
</body>

/*CSS*/

p.one{
 background:orange;
 height:100px;
 margin-top:32px; /*ここの動きに注目*/
}

p.two{
 background:gray;
 height:100px;
}
```
まず、前提として以下の点をおさえておきたい。

・p要素にはブラウザによってデフォルトで上下に16pxのマージンが付与されており、何もしない状態だと上下に16pxの間隔をあけながら垂直にレイアウトされていく。

・body要素のdisplayプロパティの値はデフォルトで`block`なことから、body要素の内側ではさきほど書いた「ブロックフォーマットコンテキスト内の動き」に従って要素がレイアウトされていく。(補足:`display:blok`の時、ボックスはブロックフォーマットコンテキストに従うのだった。詳しくはpart2参照)

以上をふまえて、話を戻すと、

つまりこの場合、CSS内の「p.one」で設定した`margin-top:32px;`は、**body要素の生成したボックスに対して適用されている。**

どういうことかというと、`<p class="one">`に設定したこの`margin-top:32px;`は**ウィンドウの上端から`32px`となっている**ということだ。

まとめると、この場合、要素がレイアウトされる流れは以下になる。

***
**「①普通にbody要素内にブロックレベル要素を配置した場合」のレイアウトの流れ**

①body要素内の一番最初のブロックレベル要素である`<p class="one">`の生成するボックスは、包含ブロックの概念より、body要素のcontent領域内に作られる。

② ①より、`<p class="one">`のマージンとbody要素のマージンが一致する。(※body要素のプロパティの値は何もいじっていないデフォルトの状態だとする。)

③`<p class="one">`のマージン「32px」とbody要素のマージン「8px」の相殺が起き、値が大きい方の「32px」が適用される。
***

#### 新しくブロックフォーマットコンテキストを確立

次に本題の「②body要素内で「新しくブロックフォーマットコンテキストを確立した」場合」を見ていく。

以下のコードを書いたとする。

```
<!--body-->
<body>
<div> <!--divを追加 -->
<p class="one"></p>
<p class="two"></p>
</div>
</body>

/*CSS*/

p.one{
 background:orange;
 height:100px;
 margin-top:32px; /*ここの動きに注目*/
}

p.two{
 background:gray;
 height:100px;
}

div{
 background:#a0d8ef;　/*変化わかりやすくするため背景を追加*/
}
```

上記のコードは「①普通にbody要素内にブロックレベル要素を配置した場合」で書いたコードに少し変更を加えたもので、pタグを2つとも`<div>`で囲んで、`<div>`にbackgroundプロパティで背景色を追加した。

この時点では、p要素の間のマージンの色がdivの背景色に変わる以外の変化は起きない。

それではp要素を囲んでいる`div`のCSS部分を以下のように変更してみる。

```
div{
 background:#a0d8ef;
 overflow:hidden; /*overflowプロパティの値を「visiable」以外にすると新しいブロックフォーマットコンテキストを確立する*/
}
```

すると、div要素の背景色が上側に`32px`と下側に`16px`分だけ広がる。

つまり、何が起きたのかを一言でいうと**基準が変わった**ということになる。

これまでは、body要素の生成したボックスを基準としてmarginが適用されていたのが、div要素の生成したボックスを基準としてmarginが適用されるようになったということだ。

なぜこうなったのかというと、`overflow:hidden;`を追加したことにより、div要素内で**新しくブロックフォーマットコンテキストが確立された**ためだ。

以上見てきた「新しくブロックフォーマットコンテキストを確立する」やり方は、marginが効かない場合の対処法として覚えておくと便利だ。

***
**復習**

新しくブロックフォーマットコンテキストが確立されるとどうなるのか？

**そのcontent領域内で新しくブロックフォーマットコンテキストの動きをする。**
***

***
**ブロックフォーマットコンテキスト内では以下の動きをする。**

・ボックスは包含ブロックの上部から初めて縦に次々に配置されていく。(包含ブロックについてはpart1-2を参照)

・2つのsibling boxes間の垂直距離、つまり共通の親要素を持つボックスの縦の距離は「マージン」によって決まる。(siblingには「共通の親を持つ兄弟」といった意味がある)

・上下に隣あっているボックス間のマージンが重なっている場合、相殺が働く。
***

### インラインフォーマットコンテキストとは

次はインラインフォーマットコンテキストについて見ていく。

仕様書CSS2.1には以下のように書かれている。

>9.4.2 Inline formatting contexts
>
>In an inline formatting context, boxes are laid out horizontally, one after the other, beginning at the top of a containing block.
>
>Horizontal margins, borders, and padding are respected between these boxes.
>
>The boxes may be aligned vertically in different ways: their bottoms or tops may be aligned, or the baselines of text within them may be aligned.
>
>The rectangular area that contains the boxes that form a line is called a line box.
>
>The width of a line box is determined by a containing block and the presence of floats.
>
>The height of a line box is determined by the rules given in the section on line height calculations.
>
>9.4.2 インラインフォーマットコンテキスト
>
>インラインフォーマットコンテキストでは、ボックスは、包含ブロックの先頭から順に水平方向にレイアウトされます。
>
>これらのボックス間では、水平方向のマージン、境界線、パディングが考慮されます。
>
>ボックスはさまざまな方法で垂直方向に整列できます。ボックスの底部または上部を整列させたり、ボックス内のテキストのベースラインを整列させたりできます。
>
>複数のボックスが並んでいる長方形の領域をラインボックスと呼びます。
>
>ラインボックスの幅は、包含ブロックとフロートの存在によって決まります。
>
>ラインボックスの高さは、「ラインの高さの計算」に関するセクションに記載されている規則によって決定されます。

インラインフォーマットコンテキスト内では以下の動きをする。

・ボックスは、包含ブロックの先頭から順に水平方向に配置されていく。

・ボックス間では、水平方向のマージン、ボーダー、パディングが使用できる。

・ボックスはさまざまな方法で垂直方向に配置できる。(原文にあるalignは「物を直線上に配置する」という意味)

#### ラインボックスとは

上記のインラインフォーマットコンテキストの説明文の中で新しく「ラインボックス」という言葉がでてきた。

ラインボックスの定義は **「複数のボックスが並んでいる長方形の領域」** とある。

定義だけではピンとこないので具体例を見てみる。

例えば、以下のコードがあったとする。

```
<p>いくつかの<em>強調された言葉が</em>出てくる
<strong>この</strong>文には</p>
```

上記のコードで、p要素は、5つのインラインボックスを含むブロックボックスを生成する。そのうちの3つは**匿名インラインボックス**となる。

**匿名インラインボックス**とは、この場合p要素の中にあって`<em>`や`<strong>`で**囲まれていないテキストが入っているインラインボックス**のことを指す。

具体的には以下の3つのテキストが入っているインラインボックスが該当する。

||匿名|
|-|-|
|①|いくつかの|
|②|出てくる|
|③|文には|

残りの2つの「強調された言葉が」と「この」は、それぞれ`<em>`と`<strong>`で囲まれているので、em要素とstrong要素の生成するインラインボックスに入っていることになる。

この匿名についてはpart2で詳しく書いた。

いったん、ここまでの話を整理してみる。

まず上記のコードでは、p要素の生成するボックスはその中に5つのインラインボックスを含んでいる。

この5つのうち、3つが「匿名インラインボックス」で、残り2つが「通常のインラインボックス」となる。

この話をふまえた上で仕様書の記述を見てみる。

仕様書には以下のように書かれている。

>To format the paragraph, the user agent flows the five boxes into line boxes.
>
>In this example, the box generated for the P element establishes the containing block for the line boxes.
>
>If the containing block is sufficiently wide, all the inline boxes will fit into a single line box:
>
>If not, the inline boxes will be split up and distributed across several line boxes.
>
>段落をフォーマットするために、ユーザー エージェントは 5 つのボックスをラインボックスに流し込みます。
>
>この例では、P 要素に対して生成されたボックスが、ラインボックスの包含ブロックを確立します。
>
>包含ブロックの幅が十分に広い場合、すべてのインラインボックスが 1つのラインボックスに収まります。
>
>そうでない場合、インラインボックスは分割され、複数のラインボックスに分散されます。

実は先程ラインボックスの具体例として示したコードは、仕様書のサンプルコードを形をそのままに単語を日本語訳したものとなる。

なので、今引用した上記の仕様書の説明がそのまま適用できる。

上記の引用では以下のことが述べられている。

・段落を配置するために、ブラウザは先程の5つのインラインボックスを「ラインボックス」に流し込む。

・先程のラインボックスの具体例で示したコードでは、p要素の生成するボックスが、この「ラインボックス」の包含ブロックを確立する。

・包含ブロックの幅が十分に広い場合、5つのインラインボックが1つの「ラインボックス」に収まります。

・包含ブロックの幅が十分ではない場合、インラインボックスは分割され、複数の「ラインボックス」に分散される。

#### あらためてラインボックスの意味を確認

ここまでで、ラインボックスがどういうものかを見てきた。

もう一度ラインボックスの意味を確認しておく。

ラインボックスとは「複数のボックスが並んでいる長方形の領域」のこと。

どういう時に作成されるかというと、テキストなどのインラインコンテンツの途中で`<span>`などを使うことで作成される。

テキストの途中で`<span>`を使用することで、「複数のインラインボックスが並んでいる状態」が出来上がり、ラインボックスが形成されるというわけだ。

要するにラインボックスは**複数のボックス(例えばインラインボックス)が並んでいる状態を「ひとまとめ」にしたもの**と考えてもok。

では、このラインボックスは一体何に役立つのだろうか。

#### ラインボックスの用途

結論から書くと、このラインボックスは **`vertical-align`プロパティと`line-height`プロパティの動きを決定する際の基準** となる。

`vertical-align`プロパティと`line-height`プロパティはそれぞれ以下の役割がある。

||意味|
|-|-|
|`vertical-align`|インライン レベルの要素によって生成されるボックスのラインボックス内の垂直方向の位置を変更できる。例:`vertical-align:super;`とすることで上付き文字を設定することができる。|
|`line-height`|行間の高さを指定できる。例:`line-height:1.6` font-sizeが50pxだとすると行間の高さの計算式は次になる。行間の高さの合計:50x1.6=80, 80-50=30。そして行間は文字の上と下に均等に付与されるので、80-50=30で出てきた30を2で割った`15`が実際の行間になる。|

`line-height`は「行間の高さ」を指定するプロパティと上の説明では書いた。

実際その通りなのだけど、仕様書では「ラインボックス」という言葉を使って以下のように説明がなされている。

>On a block container element whose content is composed of inline-level elements, 'line-height' specifies the minimal height of line boxes within the element.
>
>The minimum height consists of a minimum height above the baseline and a minimum depth below it, exactly as if each line box starts with a zero-width inline box with the element's font and line height properties.
>
>We call that imaginary box a "strut." (The name is inspired by TeX.).
>
>コンテンツがインラインレベルの要素で構成されるブロックコンテナー要素では、「line-height」は要素内のラインボックスの最小限の高さを指定します。
>
>最小限の高さは、各ラインボックスが要素のフォントと行の高さのプロパティを持つ幅ゼロのインラインボックスで始まるのとまったく同じように、ベースラインより上の最小限の高さとその下の最小限の深さで構成されます。
>
>この想像上の箱を「ストラット」と呼びます。 (名前は TeX からインスピレーションを受けています。)

上の説明をまとめると以下になる。

・`line-height`は要素内のラインボックスの「最小限の高さ」を指定する。

(復習:ラインボックスとは**複数のボックス(インラインボックスなど)が並んでいる状態を「ひとまとめ」にしたもの**。)

・「最小限の高さ」とは2つの部分で構成される。その内訳は「ベースラインより上の最小限の高さ」と「その下の最小限の深さ」の2つだ。

・「ベースラインより上の最小限の高さ」と「その下の最小限の深さ」を合わせた広さの架空の箱のことを「ストラット」と呼ぶ。

**ベースライン**とは、文字を揃える基準となる線のこと。イメージとしてはアルファベット練習帳に引いてある横線(罫線)、もしくは大学ノートを思い浮かべればok。

### displayプロパティのinlineblockの仕様

`inline-block`の説明に入る前に、前提としてdisplayプロパティの役割を改めて確認しておく。

仕様書CSS2.1の「第9章 ビジュアルフォーマットモデル」の9.2節「Controlling box generation(ボックス生成の制御)」に以下の記述がある。

>The 'display' property, described below, specifies a box's type.
>
>以下で説明する「display」プロパティは、ボックスのタイプを指定します。

このCSSの仕様書を読むシリーズのpart1「ボックスモデルのまとめ」の節にて、「HTMLのすべての要素はボックスを生成する」と書いた。

**つまり、HTMLのすべての要素はボックスを生成し、そのボックスのタイプはdisplayプロパティによって指定される**ことになる。

もちろん、html要素やbody要素もボックス生成し、そのボックスのタイプはdisplayプロパティによって決まる。

実際に、chromeブラウザのデベロッパーツールを使用すると、html要素とbody要素にデフォルトで`display:block`が指定されているのが確認できる。

このデフォルトのCSSは「デフォルトCSS」と呼ばれ、ブラウザによって自動的に指定される。

言い換えると、自分たちが普段見ているwebページは、**各要素によって生成されたボックスの集合体**とも見ることができる。  
そして、そのボックスを制御するのがCSSの役割というわけだ。

#### inlineblock

displayプロパティの値`inline-block`について仕様書CSS2.1では以下のように書かれている。

>inline-block
>
>This value causes an element to generate an inline-level block container. The inside of an inline-block is formatted as a block box, and the element itself is formatted as an atomic inline-level box.
>
>インライン-ブロック
>
>この値により、要素はインラインレベルのブロックコンテナーを生成します。インラインブロックの内部はブロックボックスとしてフォーマットされ、要素自体はアトミックインラインレベルボックスとしてフォーマットされます。

上記の引用の意味をよく理解するために「ブロックコンテナとインラインブロックの関係性」について書かれた以下の「9.2.1 Block-level elements and block boxes」節の記述もあわせて確認しておく。

>Not all block container boxes are block-level boxes: non-replaced inline blocks and non-replaced table cells are block containers but not block-level boxes.
>
>すべてのブロックコンテナボックスがブロックレベルボックスであるわけではありません。非置換インラインブロックと非置換テーブルセルはブロックコンテナですが、ブロックレベルボックスではありません。

ここで重要なのは、**非置換インラインブロックは、ブロックコンテナであるが、ブロックレベルボックスではない**の部分だ。

#### 非置換とは

上記の引用で登場した「非置換」という言葉の意味を確認しておく。

「非置換」を理解するためには、まず「置換要素」について理解する必要がある。

**置換要素とは**何かというと、**その要素のcontent領域内が外部オブジェクトに置き換えられる要素のことをいう。**

画像を追加する時に使う**`img要素`もこの置換要素の一つ。**

例えば、以下のコードを実行して、ブラウザのデベロッパーツールを使ってimg要素のcontent領域を確認してみると、conetnt領域内がまるごと画像に**置き換わっている**ことが確認できる。

```
<img src="https://1.bp.blogspot.com/-jlZlCg-8FAM/Xub_u8HTD1I/AAAAAAABZis/ZhUI05AZBEQpVinedZ6Xy-eIucmNuY2SQCNcBGAsYHQ/s1600/pose_pien_uruuru_man.png" alt="いらすとや「ぴえんのイラスト」">
```

上記の例の場合、img要素のcontent領域内の数値は「697x768」となっており、その画像自信の解像度になっていることが確認できた。

ちなみに、上記のコードでは「いらすとや」のサーバーから直接画像を引っ張ってきている。

このように、img要素では自分で用意した画像ではなくても、外部のサイトのアップロードされた画像を表示させることも可能だ。

「置換要素」について仕様書には以下のように書かれている。

>Replaced element
>
>An element whose content is outside the scope of the CSS formatting model, such as an image, embedded document, or applet. For example, the content of the HTML IMG element is often replaced by the image that its "src" attribute designates.
>
>Replaced elements often have intrinsic dimensions: an intrinsic width, an intrinsic height, and an intrinsic ratio.
>
>交換された要素
>
>画像、埋め込みドキュメント、アプレットなど、内容がCSS書式設定モデルの影響の外側にある要素。
>
>たとえば、HTML IMG 要素のコンテンツは、その「src」属性が指定する画像に置き換えられることがよくあります。
>
>置換された要素には、多くの場合、固有の幅、固有の高さ、固有の比率などの固有の寸法があります。

「outside the scope of the CSS formatting model(CSS書式設定モデルの影響の外側)」とあるように、**置換要素内は独立した空間**となっており、その外側にある空間の影響を受けない。

つまり、マージンの相殺などが起きなくなる。

「マージンの相殺が起きない」ことは例えば以下ようなコードを書いてみると確認できる。

```
<body>
<img src="https://1.bp.blogspot.com/-jlZlCg-8FAM/Xub_u8HTD1I/AAAAAAABZis/ZhUI05AZBEQpVinedZ6Xy-eIucmNuY2SQCNcBGAsYHQ/s1600/pose_pien_uruuru_man.png" alt="いらすとや「ぴえんのイラスト」">
</body>

/*CSS*/
img{
 margin-top:30px;
}
```

ブロックフォーマットコンテキストのところで見たように、`<body>`内の要素(上記のコードでいえばimg要素)に設定しているmarginは、`body要素`のデフォルトのマージン`8px`との相殺が起きることがある。

一方、上記の置換要素(img要素)の例の場合、body要素の`8px`のmarginに加えて、img要素の`margin-top:30px;`が適用されている。

つまり、「marginの合計=8px + 30px = 38px」となっており、相殺は起きていない。

このことはデベロッパーツールを開くと表示されるhtmlコードを使用して、body要素とimg要素の上にそれぞれカーソルをのせることで確認できる。

(※仮に相殺が起きるとすると、大きい方のmarginが優先されるためmarginは`30px`のみが適用される。しかし、確認してみると`30px`と`8px`の両方が適用されているので相殺は起きていないことがわかる。)

#### 非置換要素とは

置換要素についてわかったところで、今度は非置換要素について見ていく。

非置換要素とは、**置換要素ではない要素のこと**だ。置換要素をわかっていれば、すんなり理解できると思う。

置換要素の意味をもう一度確認しておく。

置換要素とは、**その要素のcontent領域内が外部オブジェクトに置き換えられる要素のこと**で、具体的には`img要素`などが該当するのだった。

つまり非置換要素はこの逆で、その要素のcontent領域内が外部オブジェクトに**置き換えられない**通常の要素のことだ。

要するに、非置換要素とは置換要素以外の要素のことと考えてok。

参考に、img要素もふくめた置換要素の一覧をまとめておく。
||一覧|
|-|-|
|**置換要素**|“img”要素、“audio”要素、“canvas”要素、“embed”要素、“iframe”要素、“input”要素、“object”要素、“video”要素|

#### 非置換インラインブロックとは

非置換インラインブロックとは、例えば`span要素`などの**置換要素ではない要素の生成するインラインブロックのこと**をいう。

置換要素の`img要素`はインラインレベル要素であるため、こういった置換要素の生成するインラインブロックと区別して「非置換インラインブロック」などと呼ばれる。

ちなみにimg要素がインラインレベル要素であることは、デベロッパーツールを使用することで確認できる。

#### あらためてプロパティinlineblockを見てみる

それではここまでの話をふまえた上で、もう一度`display:inline-block`の説明を見てみる。

>inline-block
>
>This value causes an element to generate an inline-level block container.
>
>The inside of an inline-block is formatted as a block box, and the element itself is formatted as an atomic inline-level box.
>
>インライン-ブロック
>
>この値により、要素はインラインレベルのブロックコンテナーを生成します。
>
>インラインブロックの内部はブロックボックスとしてフォーマットされ、要素自体はアトミックインラインレベルボックスとしてフォーマットされます。

つまり、`display:inline-block`と指定すると要素は以下の動きをする。

・要素はインラインレベルボックスを生成する。

・そのインラインレベルボックスの中身はブロックレベルボックスとなり、要素自体はアトミックインラインレベルボックスとして配置されることから、そのインラインレベルボックス自身は「独立した不透明なボックス」としてインラインフォーマットコンテキストに参加する。

つまり、ボックスの中身はブロックレベルの動きをして、ボックス自体はインラインレベルの動きをする(ただし独立しているためmarginの相殺などは起きない)。

アトミックインラインレベルボックスについてはpart2を参照。

#### marginの相殺

**`display:inline-block`の時、他のボックスとのマージンの相殺などは起きない。**

実際に、仕様書CSS2.1「8.3.1 Collapsing margins(マージンの相殺)」節に以下の記述がある。

>Margins of inline-block boxes do not collapse (not even with their in-flow children).
>
>インライン-ブロックボックスのマージンは折りたたまれません (フロー内の子であっても同様)。

つまり、`display:inline-block`の要素の生成する「inline-blockボックス」自体は、マージンの相殺は起きないことを意味している。

ただし「inline-blockボックス」の中身に関しては、普通にブロックフォーマットコンテキストに従うため、通常通りマージンの相殺は発生する。

実際に以下のコードを書いて確かめてみたけど、上記の通りの動きをした。

```
<body>
<div class="one">
<p class="one"></p>
<p class="two"></p>
</div>
</body>

/*CSS*/
div.one{
 background:#a0d8ef;
 display:inline-block;　/*displayプロパティの値をinline-blockに変更*/
 height:150px;
 width:300px;
 margin-top:30px;　/*body要素のマージンとの相殺が起きるか確認*/
}

p.one{
 background:orange;
 height:30px;
 margin-bottom:30px; /*ボックスの中身でマージンの相殺が起きるか確認*/
}

p.two{
 background:gray;
 height:30px;
}
```

#### marginの相殺ブロックフォーマットコンテキストの場合

新しいブロックフォーマットコンテキストを確立する場合においても同様の仕様がある。

仕様書CSS2.1「8.3.1 Collapsing margins」節に以下のように書かれている。

>Margins of elements that establish new block formatting contexts (such as floats and elements with 'overflow' other than 'visible') do not collapse with their in-flow children.
>
>新しいブロックフォーマットコンテキストを確立する要素のマージン (float や、「visible」以外の「overflow」を持つ要素など) は、フロー内の子と一緒に折りたたまれません。
>

#### widthとheightと非置換の関係

先程、非置換要素の意味について書いた。

ここでは、その非置換要素とプロパティの`width`と`height`にはどういう関係があるのかを見ていきたい。

結論を書くと、以下の関係が成り立つ。

**`width`と`height`は非置換インライン要素には適用できない。**

実際、仕様書CSS2.1の`width`と`height`の説明部分に以下の記述がある。

>This property does not apply to non-replaced inline elements.
>
>このプロパティは、非置換インライン要素には適用されません。

**非置換インライン要素とは、** `display:inline`の非置換要素のことだ。

非置換インライン要素には、例えば`span要素`がある。

実際に、以下のコードを書いて確かめてみても、`width`と`height`は機能しないことがわかる。

```
<body>
<span></span>
</body>

/*CSS*/
span{
 background:yellow;
 width:100px;
 height:150px;
}
```

一方、`display:inline`でも置換要素である`img要素`は、`width`と`height`が指定できる。

他にも、`display:inline-block`にしても`width`と`height`が指定可能となる。

part4へ続く。

次のpart4では、いよいよ`floatプロパティ`とそれを制御する`clearプロパティ`について見ていく。
