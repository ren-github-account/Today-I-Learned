前回のpart2では、ブロックレベル要素とインラインレベル要素についてを主に見てきた。

ブロックレベル要素とは、displayプロパティの値が`block`, `table`などの要素のことで、インラインレベル要素とは、displayの値が`inline`, `inline-block`などの要素のことだった。

そして、ブロックレベル要素はブロックレベルボックスを、インラインレベル要素はインラインレベルボックスをそれぞれ生成する。

さらに、その生成されるブロックレベルボックスとインラインレベルボックスは、それぞれブロックフォーマットコンテキストとインラインフォーマットコンテキストにもとづいているのだった。

ここでブロックフォーマットコンテキストとインラインフォーマットコンテキストとは何か？という疑問が湧いてくる。

### ブロックフォーマットコンテキストとは

仕様書CSS2.1には以下のように書かれている。

>Floats, absolutely positioned elements, block containers (*1) that are not block boxes, and block boxes with 'overflow' other than 'visible' (*2) establish new block formatting contexts for their contents.
>※文の流れをわかりやすくするため、いったん()内の記述は省略した。
>
>フロート、絶対配置要素、ブロック ボックスではないブロック コンテナ ()、および 'visible' 以外の 'overflow' を持つブロック ボックス () は、その内容に対して新しいブロック フォーマット コンテキストを確立します。
>
>*1: such as inline-blocks, table-cells, and table-captions
>
>*2: except when that value has been propagated to the viewport
>
>*1: インラインブロック、表セル、表キャプションなど
>
>*2: ただし、その値がビューポートに伝播されている場合は除きます。

**つまり、以下のプロパティなどを持つ要素の生成するブロックボックスは、そのcontentsに対して新しいブロックフォーマットコンテキストを確立する。**

||種類|
|-|-|
|①|float|
|②|absolutely positioned elements(絶対配置要素)|
|③|inline-blocks, table-cells, and table-captions|
|④|overflowプロパティの値がvisible(初期値)以外|
