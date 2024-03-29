このページはCSSの仕様書をひたすら読み解いていくシリーズのpart1になる。

CSSはちゃんと理解しようとすると、どうしても仕様書にあたることは避けられない。

そこで、このページでは、仕様書の中でも特に重要な部分を中心に読み解いてみた。

内容は、ボックスモデル(margin, boderなど)と、ボックスモデルに密接な関係のあるビジュアルフォーマットモデル(displayやfloatなど)の2つが中心だ。

*目次*
* [要素とは](#要素とは)
* [文書言語とは](#文書言語とは)
* [要素と文書言語のまとめ](#要素と文書言語のまとめ)
* [ボックスモデルとは](#ボックスモデルとは)
* [ボックスモデルの説明の用語](#ボックスモデルの説明の用語)
* [ビジュアルフォーマットモデル](#ビジュアルフォーマットモデル)
* [ドキュメントツリー](#ドキュメントツリー)
* [ボックスモデルのまとめ](#ボックスモデルのまとめ)
* [ビジュアルフォーマットモデルの詳細](#ビジュアルフォーマットモデルの詳細)
* [スタイルシート](#スタイルシート)
* [ビジュアルフォーマットモデルとボックスモデルの関係](#ビジュアルフォーマットモデルとボックスモデルの関係)
* [ボックスモデルの詳細](#ボックスモデルの詳細)
* [エッジの種類](#エッジの種類)

### 要素とは

要素について仕様書CSS2.1の記述は以下の通り。

>Element
    (An SGML term, see [ISO8879].) The primary syntactic constructs of the document language. Most CSS style sheet rules use the names of these elements (such as P, TABLE, and OL in HTML) to specify how the elements should be rendered.
>
>訳:（SGML用語。[ISO8879]を参照。）文書言語の最も重要な構文上の構成物。ほとんどのCSSスタイルシートの規則は、要素のレンダリングされるべき方法を指定するために、（HTMLにおけるP、TABLE、OLなどの）要素の名前を使用する。 

要素とは、**文書言語の構成物。** つまり、文書言語を構成しているパーツのこと。

要素は具体的には、p要素,table要素,ol要素などがある。

### 文書言語とは

文書言語について仕様書CSS2.1の記述は以下の通り。

>Document language
    The encoding language of the source document (e.g., HTML, XHTML, or SVG). CSS is used to describe the presentation of document languages and CSS does not change the underlying semantics(意味) of the document languages.
>
>訳:文書言語
     ソースドキュメントのエンコード言語 (HTML、XHTML、SVG など)。 CSS は文書言語の表現を記述するために使用され、CSS は文書言語の基礎となるセマンティクスを変更しません。


文書言語とは、ソースドキュメントのエンコード言語のことで、**具体的にはHTMLなどのこと。**

`エンコード（encode）とは、情報を一定の規則に従ってデータに置き換えて記録すること。`なので、ソースドキュメントのエンコード言語とは、つまり、元となる文書(ソースドキュメント)を一定の規則に従って置き換えて記録するための言語と解釈できる。その具体例がHTML。

### 要素と文書言語のまとめ

要素とは、文書言語(HTMLなど)の構成物のこと。

### ボックスモデルとは

ボックスモデルについて仕様書CSS2.1の記述は以下の通り。

>The CSS box model describes the rectangular boxes that are generated for elements in the document tree and laid out according to the visual formatting model.
>
>CSS ボックスモデルは、ドキュメントツリー内の要素に対して生成され、ビジュアルフォーマットモデルに従ってレイアウトされる長方形のボックスを説明します。

### ボックスモデルの説明の用語
#### ビジュアルフォーマットモデル

ビジュアルフォーマットモデルについて仕様書CSS2.1の記述は以下の通り。

>This chapter and the next describe the visual formatting model: how user agents process the document tree for visual media.
>
>この章と次の章では、ビジュアルフォーマットモデル、つまりユーザーエージェントがビジュアルメディアのドキュメントツリーを処理する方法について説明します。

ビジュアルフォーマットモデルとは、UA(ブラウザ)がビジュアルメディアのドキュメントツリーを処理する方法のこと。

つまり、ビジュアルフォーマットモデルは、**ドキュメントツリーのビジュアルメディア(ディスプレイに表示される見た目)に関わる部分を処理する方法のこと**と解釈できる。

ちなみに、ビジュアルメディアの詳しい意味については、以下で説明しているので参考にされたし。

#### ビジュアルメディア

visualの意味をgoogle翻訳の辞書で引くと以下の記載がある。

>a picture, piece of film, or display used to illustrate or accompany something.
>
>何かを説明したり付随したりするために使用される写真、フィルム、またはディスプレイ。

つまり、ディスプレイのことと解釈できる。

もうひとつ mediaの意味を同じように引いてみると、以下の記載がある。

>plural form of medium.
>
>媒体の複数形。

媒体は、コトバンクによると「人々に情報を伝達するための手段」といった意味がある。

以上の意味を合わせると、visual mediaとは、「ディスプレイを通してユーザーに情報を伝えるもの」と解釈できる。

#### ドキュメントツリー

ドキュメントツリーについて仕様書CSS2.1の記述は以下の通り。

> Document tree
    The tree of elements encoded in the source document. Each element in this tree has exactly one parent, with the exception of the root element, which has none.
>
> ドキュメントツリー
     ソースドキュメント内でエンコードされた要素のツリー。 このツリー内の各要素には、親がまったくないルート要素を除き、親が1 つだけあります。

 ソースドキュメント内でエンコードされた要素のツリー。つまり、HTMLによって記述された要素のツリー。

### ボックスモデルのまとめ

HTMLの要素はツリー構造をとり、そのツリー内の各要素ごとに、長方形のボックスが生成される。

この長方形のボックスは、ビジュアルフォーマットモデルに従ってレイアウトされる。

そして、CSSボックスモデルではこの長方形のボックスの説明を行う。

つまり、**ボックスモデルとは、各要素ごとに生成される長方形ボックスについての説明を行うもののこと。**

**ここで重要なのが、HTMLのすべての要素はボックスを生成する**ということだ。  
この部分に関しては後の「ビジュアルフォーマットモデルとボックスモデルの関係」の節でもう一度解説している。

さらに、この長方形のボックスは、ビジュアルフォーマットモデルに従ってレイアウトされることから、**ボックスモデルを理解するためには、ビジュアルフォーマットモデルについて理解しておく必要がある。**

ビジュアルフォーマットモデルの具体例として、 Containing blocks(邦訳では、包含ブロックと訳されている)がある。

### ビジュアルフォーマットモデルの詳細

さきほど書いたように、ビジュアルフォーマットモデルとは、**ドキュメントツリーのビジュアルメディア(ディスプレイに表示される見た目)に関わる部分を処理する方法のこと**。

そして、下のスタイルシートの節で書いたように、スタイルシートとは、**「文書の見た目を明確に表現したもの」** 。つまり、CSS(Cascading Style Sheets)のことを指す。

つまり、ディスプレイに表示される見た目に関わる部分を処理する方法を定義しているビジュアルフォーマットモデルは、CSSの根幹を成すものだと考えられる。

実際に、仕様書CSS2.1の目次を見ても、ビジュアルフォーマットモデルの概要、そして詳細と、2章にも分かれて解説されていることから、その重要性がうかがえる。

#### スタイルシート
>Style sheet
    A set of statements that specify presentation of a document.
>
>スタイルシート
ドキュメントのプレゼンテーションを指定する一連のステートメント。

statementは「スピーチや文章で何かを明確に表現すること。」、presentationは「新しい製品、アイデア、作品を聴衆に見せて説明するスピーチやトーク。」といった意味がある。

つまり、スタイルシートとは、**「文書の見た目を明確に表現したもの」** と解釈できる。

### ビジュアルフォーマットモデルとボックスモデルの関係

>In the visual formatting model, each element in the document tree generates zero or more boxes according to the box model.
>
>ビジュアル フォーマット モデルでは、ドキュメント ツリー内の各要素が、ボックス モデルに従って 0 個以上のボックスを生成します。

**つまり、HTML要素のツリーは、各要素ごとにボックスを生成し、そのボックスはボックスモデルに従って生成される。**

ここでボックスモデルの話がでてきたので、ここからはボックスモデルとは何かについて詳しくみていくことにする。

### ボックスモデルの詳細

仕様書CSS2.1の中にのボックスモデルの章の中に、「Box dimensions」という節がある(dimensionsは「測定可能な範囲」といった意味)。そこには以下の記述がある。

>Each box has a content area (e.g., text, an image, etc.) and optional surrounding padding, border, and margin areas; the size of each area is specified by properties defined below.
>
>各ボックスにはコンテンツ領域 (テキスト、画像など) と、オプションで周囲のパディング、境界線、およびマージン領域があります。各領域のサイズは、以下に定義されるプロパティによって指定されます。

「以下に定義されるプロパティ」とは、おなじみの`margin`,`padding`,`border`のこと。

**つまり、各要素ごとにボックスが生成され、そのボックスには4つの領域(`content`, `padding`, `border`, `margin`)がある。**

#### セグメントとは

そしてさらに以下の記述がある。

>The margin, border, and padding can be broken down into top, right, bottom, and left segments (e.g., in the diagram, "LM" for left margin, "RP" for right padding, "TB" for top border, etc.).
>
>マージン、ボーダー、およびパディングは、上、右、下、左のセグメントに分類できます (たとえば、図では、左マージンは「LM」、右パディングは「RP」、上ボーダーは「TB」など) 。）。

つまり、ボックスの中にある4つの領域(content, magin, border, padding)のうち、**margin, border, paddingの3つ**(※contentは除く)は、**上下左右のセグメント**に分解できる。

セグメントとは「分割される各部分」といった意味。

例えば、margin領域は`margin-top, margin-bottom, margin-left, margin-right`の4つのプロパティに分解することができる。これは、**content領域を除いた**他の領域においても同様。

#### エッジの種類

続き。

>The perimeter of each of the four areas (content, padding, border, and margin) is called an "edge", so each box has four edges:
>
>content edge or inner edge
>
>The content edge surrounds the rectangle given by the width and height of the box, which often depend on the element's rendered content. The four content edges define the box's content box. 
>
>padding edge
>
>The padding edge surrounds the box padding. If the padding has 0 width, the padding edge is the same as the content edge. The four padding edges define the box's padding box. 
>
>border edge
>
>The border edge surrounds the box's border. If the border has 0 width, the border edge is the same as the padding edge. The four border edges define the box's border box. 
>
>margin edge or outer edge
>
>The margin edge surrounds the box margin. If the margin has 0 width, the margin edge is the same as the border edge. The four margin edges define the box's margin box. 

>4 つの領域 (コンテンツ、パディング、ボーダー、マージン) のそれぞれの周囲は「エッジ」と呼ばれるため、各ボックスには 4 つのエッジがあります。
>
>コンテンツエッジまたは内側エッジ
>
>コンテンツのエッジは、ボックスのwidthとheightによって指定される四角形を囲みます。これらの四角形は、多くの場合、要素のレンダリングされたコンテンツに依存します。 4 つのコンテンツ エッジは、ボックスのコンテンツ ボックスを定義します。
>
>パディングエッジ
>
>パディング エッジはボックス パディングを囲みます。 パディングの幅が 0 の場合、パディングのエッジはコンテンツのエッジと同じになります。 4 つのパディング エッジは、ボックスのパディング ボックスを定義します。
>
>境界エッジ
>
>境界エッジはボックスの境界を囲みます。 境界線の幅が 0 の場合、境界線のエッジはパディング エッジと同じになります。 4 つの境界エッジがボックスの境界ボックスを定義します。
>
>マージンエッジまたは外側エッジ
>
>マージン エッジはボックス マージンを囲みます。 マージンの幅が 0 の場合、マージン エッジは境界エッジと同じになります。 4 つのマージン エッジは、ボックスのマージン ボックスを定義します。

上記の引用のなかで「各ボックスには4つのエッジがある」とあるが、これに関して**1つ注意点がある。**

それは、パディング、ボーダー、マージンの3つには上下左右のセグメント(例:top, bottomなど)があるが、**コンテンツだけは別で上下左右のセグメント(例:top,bottomなど)のプロパティを指定できない**ということだ。

セグメントの意味について詳しくは、さきほどの説明に書いた。

そのため、コンテンツ領域だけは、上記の引用にもあるように、その大きさは`width`と`height`によって決まる。

さらに上記の引用から、以下のことがわかる。

**①パディングが0の場合、パディングエッジはコンテンツエッジと一致する。**  
**②ボーダーが0の場合、ボーダーエッジはパディングエッジと一致する。**  
**③マージンが0の場合、マージンエッジはボーダーエッジと一致する。**

続きを引用。

>Each edge may be broken down into a top, right, bottom, and left edge.
>
>The dimensions of the content area of a box — the content width and content height — depend on several factors: whether the element generating the box has the 'width' or >'height' property set, whether the box contains text or other boxes, whether the box is a table, etc. Box widths and heights are discussed in the chapter on visual formatting >model details.
>
>The background style of the content, padding, and border areas of a box is specified by the 'background' property of the generating element. Margin backgrounds are always >transparent.
>
>各エッジは、上、右、下、左のエッジに分類できます。
>
>ボックスのコンテンツ領域の寸法 (コンテンツの幅とコンテンツの高さ) は、ボックスを生成する要素に「width」プロパティが設定されているか「height」プロパティが設定されているか、
>ボックスにテキストまたは他のボックスが含まれているかどうか、などのいくつかの要因によって決まります。 ボックスはテーブルなどです。
>
>ボックスの幅と高さについては、ビジュアル フォーマットモデルの詳細に関する章で説明します。
>
>ボックスのコンテンツ、パディング、ボーダー領域の背景スタイルは、生成要素の 'background' プロパティによって指定されます。 マージンの背景は常に透明です。

上記の記述から、**コンテンツ領域の大きさは、以下の要因によって決まる**ことがわかる。
||コンテンツ領域の大きさが決まる要因|
|-|-|
|①|要素にwidthプロパティやheightプロパティが設定されているかどうか。つまり、**widthとheightはコンテンツ領域の幅と高さを指定する**プロパティということ。|
|②|ボックスの中にテキスト、もしくは他のボックスが含まれているかどうか。この「他のボックス」とはtable要素などのことを指す。|


さらに、4つの領域のうち、コンテンツ、パディング、ボーダーの背景は`background`プロパティによって決まる。**マージンの背景に関しては常に透明な点に注意。**

長くなったので、続きはpart2に書く。











