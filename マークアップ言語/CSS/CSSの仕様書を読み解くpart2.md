このページはpart1の続きとなっている。そのため、part1で既に説明した部分に関しては省略しているところがある。

そのため、わからない用語などが出てきたらpart1を参照されたし。

*目次*
* [第9章 ビジュアルフォーマットモデル](#第9章ビジュアルフォーマットモデル)
* [box dimensionsとtyoe](#boxdimensionsとtyoe)
* [ブロックレベル要素とブロックボックス](#ブロックレベル要素とブロックボックス)
* [ブロックボックスの定義](#ブロックボックスの定義)
* [ブロックボックスの具体例](#ブロックボックスの具体例)
* [3つの用語の使い分け](#使い分け)
* [インラインレベル要素とインラインボックス](#インラインレベル要素とインラインボックス)
* [匿名ブロックボックスと匿名インラインボックス](#匿名ブロックボックスと匿名インラインボックス)
* [テキストの仕様](#テキストの仕様)

### 第9章ビジュアルフォーマットモデル

>In the visual formatting model, each element in the document tree generates zero or more boxes according to the box model. The layout of these boxes is governed by:
>
>・box dimensions and type.
>
>・ positioning scheme (normal flow, float, and absolute positioning).
>
>・ relationships between elements in the document tree.
>
>・ external information (e.g., viewport size, intrinsic dimensions of images, etc.).
>
>ビジュアル フォーマット モデルでは、ドキュメント ツリー内の各要素が、ボックス モデルに従って 0 個以上のボックスを生成します。 これらのボックスのレイアウトは次によって制御されます。
>
>・box dimensionsとtyoe。
>
>・位置決めscheme（normal flow、float、absolute positioning）。
>
>・ドキュメントツリー内の要素間の関係。
>
>・ 外部情報 (例: ビューポートサイズ、画像の固有の寸法など)。

### boxdimensionsとtyoe

「box dimensions」はpart1で見た通り、ボックスの中にあるmargin, padding, boder, contentの4つの領域のこと。

typeについてはこれから詳しくみていく。

typeについて仕様書CSS2.1での記述は以下の通り。

>9.2 Controlling box generation
>
>The following sections describe the types of boxes that may be generated in CSS 2.1. A box's type affects, in part, its behavior in the visual formatting model.
>
>The 'display' property, described below, specifies a box's type.
>
>9.2 ボックス生成の制御
>
>次のセクションでは、CSS 2.1 で生成されるボックスのタイプについて説明します。 ボックスのタイプは、ビジュアルフォーマットモデルでの動作に部分的に影響します。
>
>以下で説明する「display」プロパティは、ボックスのタイプを指定します。

つまり、ボックスのタイプ(type)は、displayプロパティによって指定されることがわかる。

### ブロックレベル要素とブロックボックス

続き。

>9.2.1 Block-level elements and block boxes
>
>Block-level elements are those elements of the source document that are formatted visually as blocks (e.g., paragraphs). The following values of the 'display' property make an >element block-level: 'block', 'list-item', and 'table'.
>
>9.2.1 ブロックレベル要素とブロックボックス
>
>ブロックレベル要素は、ブロック (段落など) として視覚的にフォーマット(配置)されたソースドキュメントの要素です。
>
>「display」プロパティの次の値は、要素をブロックレベルにします:
>
>「block」、「list-item」、「table」。

つまり、displayプロパティの値が以下の3つのいずれかの場合、**その要素はブロックレベル要素となる。**

||値|
|-|-|
|①|`block`|
|②|`list-item`|
|③|`table`|

続き。

>Block-level boxes are boxes that participate in a block formatting context.
>
>Each block-level element generates a principal block-level box that contains descendant boxes and generated content and is also the box involved in any positioning scheme.
>
>Some block-level elements may generate additional boxes in addition to the principal box: 'list-item' elements. These additional boxes are placed with respect to the principal box.
>
>ブロックレベルボックスは、ブロックフォーマットコンテキストに参加するボックスです。
>
>各ブロックレベル要素は、子孫ボックスと生成されたコンテンツを含む主要なブロックレベルボックスを生成し、また、任意の位置決めスキームに関与するボックスでもあります。
>
>一部のブロックレベル要素は、主要なボックスである「list-item」要素に加えて追加のボックスを生成する場合があります。 これらの追加のボックスは、主ボックスに対して配置されます。

ブロックレベルボックスとは、**ブロックレベル要素が生成するボックスのこと。**

そして、この「ブロックレベル要素が生成したボックス」は**ブロックフォーマットコンテキストに従う**

ちなみにブロックフォーマットコンテキストは、仕様書CSS2.1の邦訳版では「ブロック整形コンテキスト」と訳されている。

#### ブロックボックスの定義

続き。

>Except for table boxes, which are described in a later chapter, and replaced elements, a block-level box is also a block container box.
>
>A block container box either contains only block-level boxes or establishes an inline formatting context and thus contains only inline-level boxes.
>
>Not all block container boxes are block-level boxes: non-replaced inline blocks and non-replaced table cells are block containers but not block-level boxes.
>
>Block-level boxes that are also block containers are called block boxes.
>
>The three terms "block-level box," "block container box," and "block box" are sometimes abbreviated as "block" where unambiguous.
>
>後の章で説明するテーブル ボックスと置換要素を除き、ブロック レベル ボックスはブロック コンテナ ボックスでもあります。
>
>ブロック コンテナ ボックスには、ブロック レベルボックスのみが含まれるか、インライン フォーマット コンテキストが確立されてインライン レベルのボックスのみが含まれます。
>
>すべてのブロック コンテナ ボックスがブロック レベル ボックスであるわけではありません。
>
>非置換インライン ブロックと非置換テーブル セルはブロック コンテナですが、ブロック レベル ボックスではありません。
>
>ブロックコンテナでもあるブロックレベルボックスは、ブロックボックスと呼ばれます。
>
>「ブロックレベルボックス」、「ブロックコンテナボックス」、および「ブロックボックス」という 3 つの用語は、明確な場合には「ブロック」と省略されることがあります。

ブロックレベル要素が生成するブロックレベルボックスは、**ブロックコンテナボックスでもある。**

ブロックコンテナボックスの中には、ブロックレベルボックスかインラインレベルボックスのみが含まれる。

**ブロックコンテナボックスは必ずしも、ブロックレベルボックスであるわけではない。**

例えば、以下の場合はブロックコンテナボックスだけど、ブロックレベルボックスではない。

||ブロックコンテナボックスだけど、ブロックレベルボックスではない例|
|-|-|
|①|non-replaced inline blocks|
|②|non-replaced table cells|

そして、ブロックレベル要素が生成するブロックレベルボックスは、ブロックコンテナボックスでもある。  

ブロックレベルボックスがブロックコンテナボックスとなる時、**「ブロックボックス」** と呼ばれる。

#### ブロックボックスの具体例

ブロックレベルボックスがブロックコンテナボックスとなる時、**ブロックボックスと呼ばれる**が、それは具体的にどんな時なのか。

仕様書では具体例として以下が挙げられていた。

```
<DIV>
  Some text
  <P>More text
</DIV>
```

**ブロックコンテナボックスの「container」には「容器」といった意味がある。**

そして、div要素の初期値が`display:block`なことから、div要素は、ブロックレベルボックスを生成する。(復習:ブロックレベルボックスとは、ブロックレベル要素が生成するボックスのこと。)

上記の例のように、ブロックレベルボックスの中にp要素などが含まれるような時に、そのブロックレベルボックスはブロックコンテナボックスとなり、**別名:ブロックボックスと呼ばれる。**

#### 使い分け

>「ブロックレベルボックス」、「ブロックコンテナボックス」、および「ブロックボックス」という 3 つの用語は、明確な場合には「ブロック」と省略されることがあります。

これまで3つの用語ブロックレベルボックス、ブロックコンテナボックス、ブロックボックスが出てきた。

この3つの用語は、文章の中で区別がはっきりしている場合は、省略されて**単に「ブロック」と呼ばれる**こともある。

ややこしいので3つの用語をもう一度整理してみる。

||意味|
|-|-|
|ブロックレベルボックス|divやpなどのブロックレベル要素が生成するボックスのこと。つまり、各ブロックレベル要素はブロックレベルボックスを生成する。|
|ブロックコンテナボックス|なかにブロックが入るコンテナ **(容器)**　のこと。例えば、ブロックレベルボックスの中にp要素などが含まれるような時、そのブロックレベルボックスはブロックコンテナボックスとなる。|
|ブロックボックス|ブロックレベルボックスがブロックコンテナボックスでもある時、別名:ブロックボックスと呼ばれる。(※ブロックレベルボックス=ブロックコンテナボックスというわけではない)|

### インラインレベル要素とインラインボックス

>9.2.2 Inline-level elements and inline boxes
>
>Inline-level elements are those elements of the source document that do not form new blocks of content;
>
>the content is distributed in lines (e.g., emphasized pieces of text >within a paragraph, inline images, etc.).
>
> The following values of the 'display' property make an element inline-level: 'inline', 'inline-table', and 'inline-block'.
>
>Inline-level elements generate inline-level boxes, which are boxes that participate in an inline formatting context.
>
>An inline box is one that is both inline-level and whose contents participate in its containing inline formatting context.
>
>A non-replaced element with a 'display' value of 'inline' generates an inline box.
>
> Inline-level boxes that are not inline boxes (such as replaced inline-level elements, inline-block elements, and inline-table elements) are called atomic inline-level boxes because they participate in their inline formatting context as a single opaque box.
>
>9.2.2 インラインレベル要素とインラインボックス
>
>インラインレベル要素は、コンテンツの新しいブロックを作成しないソースドキュメントの要素です。
> 
>コンテンツは行単位で共有されます (段落内の強調されたテキスト部分、インライン画像など)。
>
>「display」プロパティの値「inline」、「inline-table」、および「inline-block」は、要素をインラインレベルにします。
>
>インラインレベルの要素は、インラインフォーマット コンテキストに参加するボックスであるインラインレベルボックスを生成します。
>
>インラインボックスは、インラインレベルであり、その内容がそのボックスを含むインライン書式設定コンテキストに参加しているボックスです。
>
>'display' 値が 'inline' である非置換要素は、インラインボックスを生成します。
>
>インラインボックスではないインライン レベル ボックス (置換されたインラインレベル要素、インラインブロック要素、インラインテーブル要素など) は、単一の不透明なボックスとしてインラインフォーマットコンテキストに参加するため、アトミックインラインレベルボックスと呼ばれます。

インラインレベル要素は、`display:block`の時のようなブロック(ブロックレベルボックスやブロックコンテナボックスなど)を生成しない。

`content`は横並び。

インラインレベル要素は、インラインレベルボックスを生成する。  
そしてインラインレベルボックスは**インラインフォーマットコンテキストに従う。**

インラインレベルボックスは **「インラインボックス」** とも呼ばれる。

singleは「only one(唯一の)」といった意味で、opaqueはtransparent(透明)の対義語となっていることから、  
以下の記述は、**独立した透明ではない(Anonymous block boxesやAnonymous inline boxesなどではない)ボックス**とも訳せる。

>a single opaque box.

ちなみに、対義語のことを英語でEnglish opposite(反対の) wordと言う。

### 匿名ブロックボックスと匿名インラインボックス

ここで、Anonymous block boxesやAnonymous inline boxesについて見ていく。

日本語訳は「匿名ブロックボックス」「匿名インラインボックス」となる。

2つとも、**コンテンツのフォーマット(配置)を定義しやすくするためにつくられた仮のボックス**である点は同じ。

この匿名のボックスを理解するための予備知識として、まずテキストの仕様について見ていく。

#### テキストの仕様

テキストはインラインコンテンツであり、**インラインボックスの中に存在している。**

インラインコンテンツは原文では「inline content」と書かれてあり、inlineには「連続的な一連の作業」といった意味がある。  
なので、**インラインコンテンツとは「新しい行で開始されず、連続して横並びに続いていくコンテンツ」のことをいう。**  
このインラインコンテンツの具体例がテキストというわけだ。

例えば、以下のコードを書いた場合の動きについて見ていく。

```
<p>こんにちわ</p>
```

以下の順番で画面上にレンダリングが行われる。

||動き|
|-|-|
|①|まず、p要素がブロックボックスを生成する。|
|②|テキストの「こんにちわ」は、インラインコンテンツなのでインラインボックスの中に存在する。|

まとめると、p要素が生成したブロックボックスの中に、インラインボックスがあり、そのインラインボックスの中にインラインコンテンツであるテキストの「こんにちわ」が存在していることになる。

そして、テキストの間にemやspanなどがある場合、そのほかのテキストが入っているインラインボックスは**匿名インラインボックスと呼ばれる。**

例を挙げると以下になる。

```
<p>文字を <em>強調</em> するよ</p>
```










