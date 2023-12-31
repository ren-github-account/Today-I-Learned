### displayとは

プロパティの1つで、要素の表示形式を決めるもの。

### 代表的な値

displayで使用できる値は主に以下のものがある。

||特徴|
|-|-|
|block|p、divタグなどの初期値。要素は縦に並ぶ。|
|inline(インライン)|a、imgタグなどの初期値。主に文章の一部として用いられ、要素の間に改行は入らず横に並ぶ。※幅と高さは指定できない。|
|none|この値を指定された要素はブラウザ上で非表示になる。あくまで非表示になるだけで読み込みは行われているため、読み込み速度が速くなるわけではない。|

**補足**

`none`はレスポンシブデザインを作る時に使われている。例えば「パソコンで見たときには表示させている要素を、画面の小さいスマホで見たときにだけdisplay:noneで非表示にする」といった使い方ができる。

### 文字の長さに応じてborderの長さを変更

`inline-block`を使用するとできる。

`inline`との違いは、幅と高さや上下左右の余白も指定できる点にある。

