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
* [displayプロパティのinlineblockの仕様](#displayプロパティのinlineblockの仕様)
* [inlineblock](#inlineblock)
* [marginの相殺](#marginの相殺)


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

結論から書くと、**そのcontent内で新しくブロックフォーマットコンテキストの動きをする。**

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

それではCSSの`div`の部分を以下のように変更してみる。

```
div{
 background:#a0d8ef;
 overflow:hidden; /*overflowプロパティの値を「visiable」以外にすると新しいブロックフォーマットコンテキストを確立する*/
}
```

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

>inline-block
>
>This value causes an element to generate an inline-level block container. The inside of an inline-block is formatted as a block box, and the element itself is formatted as an atomic inline-level box.
>
>インラインブロック
>
>この値により、要素はインラインレベルのブロックコンテナーを生成します。インラインブロックの内部はブロックボックスとしてフォーマットされ、要素自体はアトミックインラインレベルボックスとしてフォーマットされます。

「インラインブロック」という言葉が出てきたので、仕様書での意味を見てみる。

インラインブロックについては、仕様書CSS2.1の「9.2.1 Block-level elements and block boxes」節に以下の記述があった。

>Not all block container boxes are block-level boxes: non-replaced inline blocks and non-replaced table cells are block containers but not block-level boxes.
>
>すべてのブロック コンテナ ボックスがブロック レベル ボックスであるわけではありません。非置換インラインブロックと非置換テーブルセルはブロックコンテナですが、ブロックレベルボックスではありません。

ここで重要なのは、**非置換インラインブロックは、ブロックコンテナであるが、ブロックレベルボックスではない**の部分だ。

#### 非置換とは

上記の引用で登場した「非置換」という言葉の意味を確認しておく。

「非置換」を理解するためには、まず「置換要素」について理解する必要がある。

**置換要素とは**何かというと、**その中身(content)が外部オブジェクトに置き換えられる要素のことをいう。**

置換要素の代表例が、画像を追加する時に使う`img要素`になる。

仕様書には以下のように書かれている。

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

「outside the scope of the CSS formatting model(CSSフォーマットモデルのスコープの外側)」とあるように、置換要素の内部は独立した空間となっており、その外側にある空間の影響を受けない。

つまり、マージンの相殺などが起きなくなる。

そして、**インラインブロックとは、インラインレベルボックスであるブロックコンテナボックスであると解釈できる。**

つまり、「要素はインラインレベルのブロックコンテナーを生成します。」の部分は**インラインブロックのことを指している**と解釈できる。そして、非置換であるならブロックレベルボックスではないといえる。

もっとシンプルにいうと、「インラインレベルボックスの中にp要素などが配置されていく」というイメージをもてばok。

その後、仕様書の記述はこう続く。

>インラインブロックの内部はブロックボックスとしてフォーマットされ、要素自体はアトミックインラインレベルボックスとしてフォーマットされます。

まとめると、displayプロパティの値が`inline-block`の時、その要素はインラインブロックを生成し、そのインラインブロックの内部はブロックボックス(ブロックレベルボックスがブロックコンテナボックスとなる時の呼び名。)と同じ動きになる。

そして、そのインラインブロック自体は、アトミックインラインレベルボックス(独立した不透明なボックスとしてインラインフォーマットコンテキストに参加しているインラインレベルボックスのこと)の動きをするということになる。

#### marginの相殺

**`display:inline-block`の時、他のボックスとのマージンの相殺などは起きない。**

実際に、仕様書「8.3.1 Collapsing margins」節に以下の記述がある。

>Margins of inline-block boxes do not collapse (not even with their in-flow children).
>
>インラインブロックボックスのマージンは折りたたまれません (フロー内の子であっても同様)。

つまり、インラインブロックにおいて、marginの相殺は起きないことを意味している。(インラインブロックの要素自体はもちろんのこと、インラインブロックの内部においても相殺は起きない)

#### marginの相殺ブロックフォーマットコンテキストの場合

ブロックフォーマットコンテキストにおいても同様の仕様がある。

仕様書「8.3.1 Collapsing margins」節に以下のように書かれている。

>Margins of elements that establish new block formatting contexts (such as floats and elements with 'overflow' other than 'visible') do not collapse with their in-flow children.
>
>新しいブロックフォーマットコンテキストを確立する要素のマージン (float や、「visible」以外の「overflow」を持つ要素など) は、フロー内の子と一緒に折りたたまれません。






