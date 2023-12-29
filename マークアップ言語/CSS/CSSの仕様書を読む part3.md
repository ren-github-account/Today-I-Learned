前回のpart2では、ブロックレベル要素とインラインレベル要素についてを主に見てきた。

ブロックレベル要素とは、displayプロパティの値が`block`, `table`などの要素のことで、インラインレベル要素とは、displayの値が`inline`, `inline-block`などの要素のことだった。

そして、ブロックレベル要素はブロックレベルボックスを、インラインレベル要素はインラインレベルボックスをそれぞれ生成する。

さらに、その生成されるブロックレベルボックスとインラインレベルボックスは、それぞれブロックフォーマットコンテキストとインラインフォーマットコンテキストにもとづいているのだった。

ここでブロックフォーマットコンテキストとインラインフォーマットコンテキストとは何か？という疑問が湧いてくる。

### ブロックフォーマットコンテキストとは

仕様書CSS2.1には以下のように書かれている。

>Floats, absolutely positioned elements, block containers () that are not block boxes, and block boxes with 'overflow' other than 'visible' () establish new block formatting contexts for their contents.
>※文の流れをわかりやすくするため、いったん()内の記述は省略した。
>
>フロート、絶対配置要素、ブロック ボックスではないブロック コンテナ ()、および 'visible' 以外の 'overflow' を持つブロック ボックス () は、その内容に対して新しいブロック フォーマット コンテキストを確立します。


