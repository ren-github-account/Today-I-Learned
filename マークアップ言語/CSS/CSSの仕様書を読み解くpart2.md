### 第9章 ビジュアルフォーマットモデル

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

### box dimensionsとtyoe

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

