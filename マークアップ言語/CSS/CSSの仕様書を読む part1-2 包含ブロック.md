このページでは、Containing blocks(邦訳版では包含ブロック)について見ていく。

この「CSSの仕様書を読む」シリーズは現在part3まで書いているが、仕様書を読み解いていくにあたってどうしてもこの「包含ブロック」についての理解が必要となったので急遽追加することにした。

タイトルをpart1-2としたのは、包含ブロックを理解するためには、part1で見てきた「ボックスのエッジ」の部分の理解が必要となるためだ。

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
>9.1.2 ブロックを含む
>
>CSS 2.1 では、多くのボックスの位置とサイズは、包含ブロックと呼ばれる長方形のボックスのエッジを基準にして計算されます。
>
>一般に、生成されたボックスは、子孫ボックスのブロックを含むものとして機能します。 ボックスがその子孫を含むブロックを「確立する」と言います。 「ボックスの包含ブロック」という表現は、ボックスが生成する包含ブロックではなく、「ボックスが存在する包含ブロック」を意味します。
>
>各ボックスには、その包含ブロックに対する位置が与えられますが、この包含ブロックによって制限されることはありません。 オーバーフローする可能性があります。
>
>含まれるブロックの寸法の計算方法の詳細については、次の章で説明します。

「多くのボックスの位置とサイズは、包含ブロックと呼ばれる長方形のボックスの**エッジを基準**にして計算されます。」とある。

part1の復習になるが、**エッジとは4つの領域 (コンテンツ、パディング、ボーダー、マージン) のそれぞれの周囲(上下左右)のこと**で、以下のルールがあるのだった。

**①パディングが0の場合、パディングエッジはコンテンツエッジと一致する。**  
**②ボーダーが0の場合、ボーダーエッジはパディングエッジと一致する。**  
**③マージンが0の場合、マージンエッジはボーダーエッジと一致する。**

つまり、0がある場合には、エッジが連鎖的に狭まっていくと。
