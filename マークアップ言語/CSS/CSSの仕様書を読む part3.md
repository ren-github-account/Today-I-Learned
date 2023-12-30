前回のpart2では、ブロックレベル要素とインラインレベル要素についてを主に見てきた。

ブロックレベル要素とは、displayプロパティの値が`block`, `table`などの要素のことで、インラインレベル要素とは、displayの値が`inline`, `inline-block`などの要素のことだった。

そして、ブロックレベル要素はブロックレベルボックスを、インラインレベル要素はインラインレベルボックスをそれぞれ生成する。

さらに、その生成されるブロックレベルボックスとインラインレベルボックスは、それぞれブロックフォーマットコンテキストとインラインフォーマットコンテキストにもとづいているのだった。

ここでブロックフォーマットコンテキストとインラインフォーマットコンテキストとは何か？という疑問が湧いてくる。

### ブロックフォーマットコンテキストとは

仕様書CSS2.1には以下のように書かれている。

>Floats, absolutely positioned elements, block containers (*1) that are not block boxes, and block boxes with 'overflow' other than 'visible' (*2) establish new block formatting contexts for their contents.
>
>//文の流れをわかりやすくするため、いったん()内の記述は省略した。
>
>フロート、絶対配置要素、ブロック ボックスではないブロック コンテナ (*1)、および 'visible' 以外の 'overflow' を持つブロック ボックス (*2) は、その内容に対して新しいブロック フォーマット コンテキストを確立します。
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

**つまり、以下のプロパティなどを持つ要素の生成するボックスは、そのcontentsに対して新しいブロックフォーマットコンテキストを確立する。**

||種類|
|-|-|
|①|float|
|②|absolutely positioned elements(絶対配置要素)|
|③|ブロックボックスではないブロックコンテナ。例:inline-blocks, table-cells, and table-captionsなど。|
|④|overflowプロパティの値がvisible(初期値)以外のブロックボックス。つまり、`overflow:visible`の時はブロックフォーマットコンテキストを確立しない。|

**④の補足**

ブロックレベル要素はブロックレベルボックスを生成し、それはブロックフォーマットコンテキストを確立するが、  
例外として`overflow:visible`の時はブロックフォーマットコンテキストを確立しない。

続き。

>In a block formatting context, boxes are laid out one after the other, vertically, beginning at the top of a containing block.
>
>The vertical distance between two sibling boxes is determined by the 'margin' properties. Vertical margins between adjacent block-level boxes in a block formatting context collapse.
>
>ブロックフォーマットコンテキストの内部で、ボックスは、containing blockの上部から始めて垂直方向に次々にレイアウトされます。
>
> 2 つの兄弟ボックス間の垂直距離は、「マージン」プロパティによって決まります。
>
>ブロックフォーマットコンテキスト内の隣接するブロックレベルボックス間の垂直方向のマージンは相殺します。

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

displayプロパティの値が`inline-block`の時、その要素はインラインレベルボックスを生成する。インラインレベルボックスとはインラインレベル要素によって生成されるボックスのこと(HTMLのすべての要素はボックスを生成するのだった)。そしてその時、そのインラインレベルボックスは、ブロックコンテナーとなる。この文脈においてのブロックコンテナーの意味は、「中にブロックレベルボックスが入る容器のこと」とと考えられる。

さらに、そのinline-block要素の内部は、ブロックボックス(ブロックレベルボックスの中にp要素などが含まれているボックス)となる。そしてその要素自体はアトミックインラインレベルボックスとなるため水平方向にレイアウトされていき、他のボックスとのマージンの相殺などは起きない(独立した空間となるので)。

