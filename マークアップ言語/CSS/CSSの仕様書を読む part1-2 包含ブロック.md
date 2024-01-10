このページでは、Containing blocks(邦訳版では包含ブロック)について見ていく。

この「CSSの仕様書を読む」シリーズは現在part3まで書いているが、仕様書を読み解いていくにあたって「包含ブロック」についての理解が必要となったので追加することにした。

タイトルをpart1-2としたのは、包含ブロックを理解するためには、part1で見てきた「ボックスのエッジ」の部分の理解が必要となるためだ。

*目次*
* [まとめ](#まとめ)
* [包含ブロックの分類](#包含ブロックの分類)
* [・1](#その1)
* [・2](#その2)

### Containing blocks

仕様書CSS2.1の包含ブロックについての記述は以下のように書かれている。

>9.1.2 Containing blocks
>
>In CSS 2.1, many box positions and sizes are calculated with respect to the edges of a rectangular box called a containing block.
>
> In general, generated boxes act as containing blocks for descendant boxes; we say that a box "establishes" the containing block for its descendants. The phrase "a box's containing block" means "the containing block in which the box lives," not the one it generates.
>
>Each box is given a position with respect to its containing block, but it is not confined by this containing block; it may overflow.
>
>The details of how a containing block's dimensions are calculated are described in the next chapter. 
>
>9.1.2 包含ブロック
>
>CSS 2.1 では、多くのボックスの位置とサイズは、包含ブロックと呼ばれる長方形のボックスのエッジを基準にして計算されます。
>
>一般に、生成されたボックスは、子孫ボックスの包含ブロックとして機能します。 ボックスがその子孫のための包含ブロックを「確立する」と言います。 「ボックスの包含ブロック」という表現は、ボックスが生成する包含ブロックではなく、「ボックスが存在する包含ブロック」を意味します。
>
>各ボックスには、その包含ブロックに対する位置が与えられますが、この包含ブロックによって制限されることはありません。 オーバーフローする可能性があります。
>
>含まれるブロックの寸法の計算方法の詳細については、次の章で説明します。

「多くのボックスの位置とサイズは、包含ブロックと呼ばれる長方形のボックスの**エッジを基準**にして計算されます。」とある。

part1の復習になるが、**エッジとは4つの領域 (コンテンツ、パディング、ボーダー、マージン) のそれぞれの周囲(上下左右)のこと**で、以下のルールがあるのだった。

**①パディングが0の場合、パディングエッジはコンテンツエッジと一致する。**  
**②ボーダーが0の場合、ボーダーエッジはパディングエッジと一致する。**  
**③マージンが0の場合、マージンエッジはボーダーエッジと一致する。**

つまり、0がある場合には、エッジが連鎖的に狭まっていく。

さらに「一般に、生成されたボックスは、子孫ボックスの包含ブロックとして機能します。 ボックスがその子孫のための包含ブロックを「確立する」と言います。」とある。

**つまり、html要素も含めたすべての要素はボックスを生成するが、この生成されたボックスは同時に、子要素の生成するボックスの「包含ブロック」として機能するということ。**

この「すべての要素はボックスを生成する」については、part1のボックスモデルの部分で詳しく書いたので、よくわからなかった人はそちらを参照されると理解できるかもしれない。

もう一つ、以下の部分も頭に入れておこう。

>「ボックスの包含ブロック」という表現は、ボックスが生成する包含ブロックではなく、「ボックスが存在する包含ブロック」を意味します。

つまり、「包含ブロック」は生成されるのではなく、**「ボックスを中に内包している空間」** というイメージを持っておけばok。

これは**英単語のContainingに「（誰かまたは何か）を中に持っている」という意味がある**ことからもイメージすることができる。

#### Containingblockの定義

Containing block(包含ブロック)の定義は以下のように書かれている。

>10.1 Definition of "containing block"
>
>The position and size of an element's box(es) are sometimes calculated relative to a certain rectangle, called the containing block of the element.
>
>The containing block of an element is defined as follows:
>
>The containing block in which the root element lives is a rectangle called the initial containing block.
>
>For continuous media, it has the dimensions of the viewport and is anchored at the canvas origin; it is the page area for paged media.
>
>The 'direction' property of the initial containing block is the same as for the root element.
>
>For other elements, if the element's position is 'relative' or 'static', the containing block is formed by the content edge of the nearest block container ancestor box.
>
>10.1 「包含ブロック」の定義
>
>要素のボックスの位置とサイズは、要素の「包含ブロック」と呼ばれる特定の長方形を基準にして計算されることがあります。
>
>要素の「包含ブロック」は次のように定義されます。
>
>ルート要素が存在する包含ブロックは、初期包含ブロックと呼ばれる長方形です。
>
>連続メディアの場合、ビューポートの寸法を持ち、キャンバスの原点に固定されます。 これは、ページ化されたメディアのページ領域です。
>
>初期包含ブロックの「direction」プロパティは、ルート要素の場合と同じです。
>
>他の要素の場合、要素のpositionプロパティの値が「`relative`」または「`static`」の場合、包含ブロックは最も近いブロックコンテナ祖先ボックスのコンテンツエッジによって形成されます。

#### positionプロパティ

包含ブロックの定義の最後の部分で、positionプロパティの話が出てきたので、まずpositionプロパティとは何かを見ていくことにする。

仕様書CSS2.1には以下のように書かれている。

>9.3.1 Choosing a positioning scheme: 'position' property
>
>The 'position' and 'float' properties determine which of the CSS 2.1 positioning algorithms is used to calculate the position of a box.
>
>9.3.1 位置決めスキームを選ぶ: 'position' プロパティ
>
>「position」プロパティと「float」プロパティは、ボックスの位置の計算に CSS 2.1 位置決めアルゴリズムのどれを使用するかを決定します。

schemeには「計画」といった意味がある。

つまり、**positionプロパティとは、ボックスの位置を決めるアルゴリズムを決定するためのプロパティ。**(floatプロパティについても同様。)

#### positionプロパティの値

では、positionプロパティの値にはどんなものがあるのか。

ここでは、先ほどの文章を読み解くために必要な「`static`」「`relative`」について見ていく。

#### static
`static`は、positionプロパティの初期値(つまり、何も設定しない場合はこの`static`になっている)で、この値の時は、通常の動きで画面に配置される。

通常の動きなので、シンプルに普通の動きをすると考えてok。

#### relative

この`relative`を理解するためには、その反対の動きをする`absolute`を理解するのが近道だ。

そこで、この`relative`については`absolute`と対比しながら見ていく。

この`relative`と`absolute`の違いについては実際にコードを書いたほうがわかりやすいので、以下にテストコードを書いてみた。

```
<!--HTMLのbody内に書く-->
<div class="parent">
 <div class="child">
 </div>
</div>

/*CSS内に書く*/
div.parent{
 background:#a0d8ef; /*そら色*/
 width:500px;
 height:300px;
 margin-top:80px;
 margin-left:80px;
}

div.child{
 background:#83ccd2; /*そらみどり*/
 width:150px;
 height:150px;
 position:relative; top:20px; left:150px;　/*ここをrelativeにしたり、abosoluteにしたりすると位置が変化することがわかる*/
}
```

**absoluteにした場合**

上記のコードで、positionプロパティの値を`absolute`にした場合は、表示される画面(ウィンドウ)の左上を起点として、そこからtop:20px, left:150pxの位置に緑のボックスが配置される。

**relativeにした場合**

一方、`relative`にすると、直近の親要素であるdiv.parentを基準に緑のボックスが配置された。

### まとめ

長くなった上に色々とっちらかってわかりずらくなったため、これまでの内容を整理してみる。

#### 包含ブロックの意味

まず、そもそも包含ブロックとは何か。

仕様書CSS2.1の10.1節には以下のように書かれている。

>The position and size of an element's box(es) are sometimes calculated relative to a certain rectangle, called the containing block of the element.
>
>要素のボックスの位置とサイズは、要素の「包含ブロック」と呼ばれる特定の長方形を基準にして計算されることがあります。

**前提として、すべての要素はボックスを生成するのだった。** そして、その生成されたボックスのには4つの領域 **(margin, border, padding, content)** がある。(part1参照)

我々は、その4つの領域に色をつけたり太さを変えたりすることでデザインを作っていく。

今述べた前提と、先ほどの10.1節の記述を合わせると以下の結論が導ける。

**要素によって生成されたボックスの位置とサイズは、包含ブロックによって決まる。**

原文には「sometimes」が付いているため、例外もあるのだろうが、ひとまずは「包含ブロック」によって決まると覚えておこう。

以上をふまえて、包含ブロックとは何かの答えは、

**包含ブロックとは、ボックスの位置とサイズを決めるもの**となる。

もっと言うと、**各要素の生成するボックスは包含ブロックというブロックの中に作られる**ということになる。(ブロックの意味についてはpart2を参照)

ただし、以下の記述にある通り、包含ブロックによってボックスの位置とサイズが決まるが、**ボックスは包含ブロックをはみ出す(オーバーフロー)することもある。**

>各ボックスには、その包含ブロックに対する位置が与えられますが、この包含ブロックによって制限されることはありません。 オーバーフローする可能性があります。

### 包含ブロックの分類

包含ブロックは大きく分けて以下の2つに分けられる。

**①ルート要素が存在する包含ブロック**  
**②その他の要素が存在する包含ブロック**

順番に見ていく。

### その1
**①ルート要素が存在する包含ブロック**

ルート要素とはhtml要素ツリーの中で一番上にある要素のことで、具体的にはhtml要素のことをいう。

そして、**ルート要素の生成するボックスは初期包含ブロックの中に存在する。**

また、**初期包含ブロックは以下の記述にあるように、ビューポート、つまりウィンドウ(要素が生成するボックスが表示される画面)と同じ大きさを持ち、キャンバスの原点(左上の角)に固定される。**

> For continuous media, it has the dimensions of the viewport and is anchored at the canvas origin;
>
> 連続メディアの場合、ビューポートの寸法を持ち、キャンバスの原点に固定されます。

もう少し噛み砕いていうと、初期包含ブロックは「表示される画面」と同じ大きさを持っている。  
これは言い換えると、**ウィンドウと同じ大きさを持つ真っ白な画用紙** とも言える。

そして、このウィンドウと同じ大きさを持つ真っ白な画用紙の中に、ルート要素であるhtml要素の生成するボックスが置かれる。

くわえて、先ほどの記述には「初期包含ブロックは、キャンバスの原点に固定される」ともある。

これは真っ白な画用紙がキャンバスの左上の隅に角を合わせてどっこいしょと置かれているイメージを持てばok。

連続メディアとは、普段目にする連続したwebページのこと。

反対に連続していないメディアとは、例えばwebページをA4の紙などで印刷する時、用紙内に収まりきらないときは、2ページ目に分割されて印刷される。このようなページ分けされたメディアのことをいう。

#### ビューポートとキャンバスの違い

ビューポートとキャンバスの違いをまとめてみる。

||意味|
|-|-|
|ビューポート|表示される画面のこと。具体的には1280x720などの大きさが決まっており有限。|
|キャンバス|要素によって生成されるボックスなどが配置される空間のことで、ビューポートより大きい場合はスクロール機能などを使用して表示する必要がある。ビューポートが有限なのに対し、キャンバスは無限に広がっている(※厳密にはUA(ブラウザ)によって幅と高さの制約が課される)。|

**日常のものに例えると、ビューポートは「窓」で、キャンバスは「外の世界」。**

「外の世界」は実際には最大で地球の幅分だけ広がっているけど、「窓」から見えるのは「窓の大きさ分」の一部だけ。

**具体例**

ルート要素が存在する包含ブロックの具体例を見てみる。

例えば、以下のようなコードを書いたとする。

```
/*CSSコード*/
html{
 background:gray;
 width:300px;
 height:150px;
}

<!--htmlコード-->
<!DOCTYPE html>
<head>
<link rel="stylesheet" href="style.css">
<title>てすと</title>
</head>
<body>
</body>
</html>
```

まず、html要素のbackgroundプロパティによってキャンバス全体がグレーになる。

この挙動については仕様書CSS2.1の14.2節に以下の記述がある。

>The background of the root element becomes the background of the canvas and covers the entire canvas,
>
>ルート要素の背景がキャンバスの背景となり、キャンバス全体を覆い、

つまり、ルート要素の背景はキャンバス全体に適用され、キャンバス全体に覆いかぶさっている。

キャンバスは、ボックスも含むことから、上記のコードのように`width`と`height`を指定しても画面全体がグレーのままで変化は起きない。  
変化は起きないけど、デベロッパーツールを使ってhtml要素によって生成されたボックスを確認すると、コンテンツ領域に反映されている。(上記の例だと、コンテンツ領域が300x150となっている。widthとheightプロパティはコンテンツ領域の幅と高さを指定するプロパティなのだった(part1参照))

上記のコードに以下のようにborderプロパティを追加してあげると、widthとheightで指定した300x150の領域がきちんと生成されているのが確認できる。
```
/*CSSコード borderプロパティを追加*/
html{
 background:gray;
 width:300px;
 height:150px;
 border:solid orange;
}
```

上記のコードを例にとって、画面に表示されるまでの流れを整理すると以下になる。

①ビューポートと同じ大きさを持つ初期包含ブロックが、html要素によって生成されるボックスに対して与えられる。

②html要素のボックスは、その初期包含ブロックの中に生成される。

③html要素のbackgroundプロパティはキャンバス全体を覆うため、表示画面全体はグレーとなり、html要素のコンテンツ領域もグレーとなる。

④borderプロパティを指定してあげることで、widthとheightで指定した大きさのコンテンツ領域が実際に作られていることが確認できる。

### その2
**②その他の要素が存在する包含ブロック**

**ルート要素以外の包含ブロックは、positionプロパティの値が何であるかによって決まる。**

先ほど引用した仕様書の記述をもう一度見てみる。

>For other elements, if the element's position is 'relative' or 'static', the containing block is formed by the content edge of the nearest block container ancestor box.
>
>他の要素の場合、要素のpositionプロパティの値が「`relative`」または「`static`」の場合、包含ブロックは最も近いブロックコンテナ祖先ボックスのコンテンツエッジによって形成されます。

ルート要素以外の要素が存在する包含ブロックを調べるには、**まずpositionプロパティの値が何であるかを確認する。**

プロパティの値が「`relative`」もしくは「`static`」**(staticは初期値なので何も設定していない場合はこれ)** の場合は、包含ブロックは直近の親要素のコンテンツエッジによって形成される。

**そして、次に、包含ブロックの形成されるコンテンツエッジがどこかを調べる**ために、以下のルールに従って絞り込んでいく。

**①マージンが0の場合、マージンエッジはボーダーエッジと一致する。**(質問:marginは0か？)

**②ボーダーが0の場合、ボーダーエッジはパディングエッジと一致する。**  (boderは0か？)

**③パディングが0の場合、パディングエッジはコンテンツエッジと一致する。**(paddingは0？)

例えば、marginもboderもpaddingもすべて0なら、結局マージンエッジがコンテンツエッジと一致することになり、そこに包含ブロックが形成される。












