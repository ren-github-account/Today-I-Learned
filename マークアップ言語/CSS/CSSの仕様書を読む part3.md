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

ブロックレベル要素はブロックレベルボックスを生成し、それはブロックフォーマットコンテキストにもとづくが、例外として`overflow:visible`の時はブロックフォーマットコンテキストにもとづかない(ブロックフォーマットコンテキストの影響下にない状態になる)。

続き。

>In a block formatting context, boxes are laid out one after the other, vertically, beginning at the top of a containing block.
>
>The vertical distance between two sibling boxes is determined by the 'margin' properties. Vertical margins between adjacent block-level boxes in a block formatting context collapse.
>
>ブロックフォーマットコンテキストの内部で、ボックスは、含まれるブロックの上部から始めて垂直方向に次々にレイアウトされます。
>
> 2 つの兄弟ボックス間の垂直距離は、「マージン」プロパティによって決まります。
>
>ブロックフォーマットコンテキスト内の隣接するブロックレベルボックス間の垂直方向のマージンは相殺します。

### displayプロパティのinlineblockの仕様

仕様書CSS2.1の「第9章 ビジュアルフォーマットモデル」の9.2節「Controlling box generation(ボックス生成の制御)」に以下の記述がある。

>The 'display' property, described below, specifies a box's type.
>
>以下で説明する「display」プロパティは、ボックスのタイプを指定します。

このCSSの仕様書を読むシリーズのpart1「ボックスモデルのまとめ」の節にて、「HTMLのすべての要素はボックスを生成する」と書いた。

**つまり、HTMLのすべての要素はボックスを生成し、そのボックスのタイプはdisplayプロパティによって指定される**ことになる。
