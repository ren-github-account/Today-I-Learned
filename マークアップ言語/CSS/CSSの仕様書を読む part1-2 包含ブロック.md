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
>要素のボックスの位置とサイズは、要素を含むブロックと呼ばれる特定の四角形を基準にして計算されることがあります。 要素を含むブロックは次のように定義されます。
>
>ルート要素が存在する包含ブロックは、初期包含ブロックと呼ばれる四角形です。
>
>連続メディアの場合、ビューポートの寸法を持ち、キャンバスの原点に固定されます。 これは、ページ化されたメディアのページ領域です。
>
>最初の包含ブロックの「方向」プロパティは、ルート要素の場合と同じです。
>
>他の要素の場合、要素のpositionが「`relative`」または「`static`」の場合、包含ブロックは最も近いブロックコンテナ祖先ボックスのコンテンツエッジによって形成されます。
