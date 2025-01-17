*目次*
* [概要](#概要)
* [参考記事](#参考記事)
* [canvas要素](#canvas要素)
* [図形を描画](#図形を描画)
* [独立した四角形を描画](#独立した四角形を描画)
* [円を描画](#円を描画)
* [縁にだけ色を塗る](#縁にだけ色を塗る)
* [ボールを動かす](#ボールを動かす)
* [変数のスコープ](#変数のスコープ)
* [壁への衝突を検出](#壁への衝突を検出)
* [上端も下端もマイナスになる理由](#上端も下端もマイナスになる理由)
* [画面端にぶつかる毎に色を変更](#画面端にぶつかる毎に色を変更)

### 概要

JavaScriptを使ってブロック崩しゲームを作成するのに必要な知識をまとめていく。

### 参考記事

・MDN-キャンバスを作ってその上に描画する

https://developer.mozilla.org/ja/docs/Games/Tutorials/2D_Breakout_game_pure_JavaScript/Create_the_Canvas_and_draw_on_it

・MDN-ボールを動かす

https://developer.mozilla.org/ja/docs/Games/Tutorials/2D_Breakout_game_pure_JavaScript/Move_the_ball

・MDN-ボールを壁で跳ね返させる

https://developer.mozilla.org/ja/docs/Games/Tutorials/2D_Breakout_game_pure_JavaScript/Bounce_off_the_walls


### canvas要素

`canvas要素`は画面上に**グラフィックやアニメーションを描画**する時に使われる。

**使い方**

基本的に`document.querySelector()メソッド`と`getContext()メソッド`と一緒に使用される。

具体的には以下のように書く。　

```
const canvas = document.querySelector("#myCanvas");
const ctx = canvas.getContext("2d");
```

**・解説**

まず、`document.querySelector()メソッド`で`canvas要素`を取得。

その後`getContext()メソッド`の引数を`"2d"`と設定することで、`CanvasRenderingContext2Dオブジェクト`が作成される。

この`CanvasRenderingContext2Dオブジェクト`の持つ色々なメソッドを組み合わせることで、画面にドットを描いたり、その他にも図形、文字、画像を描画したりすることができるようになる。

### 図形を描画

実際に図形を描画するには、上記のコードにたいして以下のように追記する。

```
const canvas = document.querySelector("myCanvas");
const ctx = canvas.getContext("2d");

// 図形を描画
ctx.fillStyle = "green";
ctx.fillRect(10, 10, 100, 100);
```

**解説**
||意味|
|-|-|
|`fillStyle`|図形の色を指定するプロパティ。注意点としてはあくまでも「図形」の色を指定するということ。|
|`fillRect`|` fillStyle`で塗りつぶした色をもとに四角形を作成するメソッド。書き方は`fillRect(x, y, width, height)`のように書き、`(x, y)`を始点とし、`width`と`height`でサイズを指定しで描画する。始点は左上から始まる。具体的には`(x,y)`を`(20, 40)`とした場合、`canvas要素内`の左端から`20px`、上端から`40px`の位置に長方形を作成する。|

`fill`は英語で **「塗りつぶす」** といった意味がある。

また、`Rect`は英語で**長方形**を意味するrectangleの略。(普通に正方形も作成できることから、**四角形**と捉えておいたほうが良い)

### 独立した四角形を描画

上記のコードの`// 図形を描画`の箇所を以下のコードに変更する。

```
// 独立した赤い四角形を描画
ctx.beginPath();
ctx.rect(20, 40, 50, 50);
ctx.fillStyle = "#FF0000";
ctx.fill();
ctx.closePath();
```

**解説**

||意味|
|-|-|
|`beginPath()`|独立したパスの作成するメソッド。例えば`beginPath()`を、線を引く毎に最初に記述すると色違いの線を引くことができる。|
|`closePath()`|図形の最初と最後の点を自動的に接続するメソッド。孤や三角形などの底辺を作成する時に使う。なお図形がすでに閉じていたり、1つしか点がなかったりした場合は、このメソッドは何もしない。|
|`rect()`|**ただ四角形の作成のみを行う**メソッド。そのため四角形の作成と`canvas`への描画を同時に行いたい場合は`fillRect()`を使用する。|
|`fill()`|`rect()`で作成した長方形を`canvas`に描画(追加)する時に使用する。|

### 円を描画

先ほどのコードの以下のコードを追記する。

```
// 緑の円を描画
ctx.beginPath();
ctx.arc(240, 160, 20, 0, Math.PI * 2, false);
ctx.fillStyle = "green";
ctx.fill();
ctx.closePath();
```

**解説**
||意味|
|-|-|
|`arc()`|円周を作成するメソッド。円を描くことができる。書き方は`arc(x, y, radius, startAngle, endAngle, counterclockwise)`のように記述する。`(x, y)`は円の中心の座標を表し、`radius`は半径を表す。そして`(startAngle, endAngle)`で円の開始角度と終了角度を指定する。そして最後の`counterclockwise`は省略可能の値で、円の描く方向を指定できる。デフォルトの値は`false`で、時計回りで始まりの角度から終わりの角度に向けて描く。|
|`Math.PI`|円周率3.14…(以下省略)を表す。 `Math.PI * 2`で`2π`となり`360°`を表している。|

### 縁にだけ色を塗る

以下のコードを追記。

```
// 図形の縁にだけ色を塗る
ctx.beginPath();
ctx.rect(160, 10, 100, 40);
ctx.strokeStyle = "rgba(0, 0, 255, 0.5)";
ctx.stroke();
ctx.closePath();
```

**解説**

||意味|
|-|-|
|`stroke()`|線を描くメソッド。**注意点**として、`moveTo()`と`lineTo()`を使用して線を追加した後はこの`stroke()`を記述することを忘れずに。そうしないと線が描画されない。|
|`strokeStyle()`|`stroke()`で描いた**輪郭線の色**を指定する。デフォルト値は`#000`(黒色）。|

### ボールを動かす

画面に描画したボールを動かすために必要な知識をまとめていく。

||意味|
|-|-|
|`setInterval()`|指定した秒数ごとに関数を繰り返し実行する。`setInterval(func, delay)`のように書き、`func`の部分には関数名を、`delay`には秒数を書く。秒数はミリ秒単位で表され、**1000ミリ秒が1秒に相当する**ので、1秒ごとに実行した場合は`1000`と書く。|
|`.width`|このプロパティが`canvas要素`にたいして使用される時は、`canvas要素`の`width`を取得できる。|
|`.height`|同様に`canvas要素`の`height`を取得する。|
|`clearRect()`|`canvas`の一部または全体を真っ白(厳密には透明な黒)にする。円や四角形を描画する直前に`ctx.clearRect(0, 0, canvas.width, canvas.height);`を記述することで**動いているように見せることができる。**|

**コード**
```
<body>

<canvas id="myCanvas" width="480" height="320"></canvas>

<script>
...(省略)
let x = canvas.width / 2;
let y = canvas.height - 160;

let dx = 2;
let dy = -2;

function draw() {
...(省略)
x += dx;
y += dy;
}
setInterval(draw, 10);
</script>
</body>
```

**解説**

`let x = canvas.width / 2;`は数値を代入すると、`canvas要素`の幅の半分の`480 / 2 = 240`となり、**ボールを`canvas要素の`ちょうど半分の位置に指定**している。

同様に`let y = canvas.height - 160;`は、`320-160 = 160`となり、**ボールをy軸(縦)のちょうど真ん中の位置に指定**している。

そして`x += dx;`はx軸に加算してボールを右に進め、`y += dy;`はy軸に加算しボールを上へ進める。

### 変数のスコープ

```
// 変数を関数の外側で宣言している
let x = canvas.width / 2;
let y = canvas.height - 30;
let dx = 2;
let dy = -2;

function draw() {
// 処理内容
}
```

上記のコードのように変数を関数の外側で宣言すると、**現在の文書のどのコードからも使用できる**ようになる。このことから、このような変数のことを**グローバル（大域）変数**と呼ぶ。( 参照:[MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Guide/Grammar_and_types#%E5%A4%89%E6%95%B0%E3%81%AE%E3%82%B9%E3%82%B3%E3%83%BC%E3%83%97) )

### 壁への衝突を検出

壁への衝突を検出するため条件式として**もっとも基本となるのが以下のコード。**

```
if (y + dy < 0) {
  dy = -dy;
}
```

**解説**

上記のコードは画面の4つの端のうち**上端への衝突を検出する**ためのもの。

`y`が縦の位置を表し、`dy`には`-2`が入っている。

指定したミリ秒数ごとに`y`に対して`dy`に入っている数値`-2`が足されていき、ボールがどんどん上側に移動していく仕組みになっている。

そして`y + dy`の計算結果が0未満となって`y + dy < 0`が`true`となった時、`-dy = -(-2) = 2`となって符号が反転することで今度は`y`に`+2`がされていき、ボールが下側に移動していく。

ここである疑問がわいてくる。

それは条件式は**なぜ`y < 0`ではなく`y + dy < 0`なのか**、と。

`y`が縦の位置を表しているのなら別に`y < 0`でも良くないか、と。

結論から言うと、この答えは**画面の端にめり込むのを防ぐため**となる。

どういうことか、実際のコードを見ながら見ていく。

```
// 変数yの中身を表示するための要素を作成
const yWatch = document.createElement("p");
yWatch.id = "yWatch";
document.body.appendChild(yWatch);

// 描画する円の半径
const ballRadius = 10;

// y軸に加算
y += dy;

// 上端と下端への衝突を検出
// dy = 0とすることで移動を停止
if (y + dy > canvas.height - ballRadius || y + dy  < ballRadius) {
  dy = 0;
}

// 変数yの中身に+1した結果を表示
yWatch.textContent = y + 1;
```

上記の実際のコードでは、まず変数`ballRadius`に半径`10`を代入した上で`y + dy < ballRadius`として判定に使用する。

そして上記のコードでは、この条件式の意味を分かりやすくするために、判定文の直後に`textContentプロパティ`を使用して`y`の中身を表示させている。

if文の中身の`dy = 0;`は加算をやめて移動を停止することを意味する。

ここでもし`y < ballRadius`だと、`10 < ballRadius`が`false`となって`y += dy;`が**1回分多く実行されてしまい**`y = 8`となって上端が画面にめり込んでしまう。

一方で`y + dy < ballRadius`だと、**`10 + (-2) < ballRadius`の時点で`true`となって`y += dy;`が1回分多く実行されずにすみ**`y = 10`と上端にめり込む前に止まることが可能となる。

### 上端も下端もマイナスになる理由

```
// dxとdyの初期値
let dx = 2;
let dy = -2;

// ボールの中心座標への加算処理
x += dx;
y += dy;

// 上端への衝突検出
if (y + dy  < ballRadius) {
  dy = -dy;
　// 下端への衝突検出
 } else if (y + dy > canvas.height - ballRadius ) {
  // パドルへの衝突も合わせて確認
  if (x > paddleX && x < paddleX + paddleWidth) {
    dy = -dy;
   } 
```

上端と下端の衝突検出のコードで結局どちらも`dy = -dy`になっているのはなぜなのかが気になったので見ていく。

その理由はボールの動きを順に追っていくとわかる。

まず`dy`の初期値が`-2`で、最初はボールの中心`y座標`に対して`-2`ずつ加算されていくことで上側に移動していく。

そして上端へ衝突すると`dy = -(-2) = 2`となって`+2`ずつ加算されていくことで今度は反対の下側へ移動していく。

`+2`されて下側に移動しているので今度はその動きを逆にするには`dy = -dy = -(2)`とすれば良くて、**結局上端も下端も`dy = -dy`となる。**

これは左右の衝突検出も同様。

### 画面端にぶつかる毎に色を変更

衝突検出の応用編として、画面端にぶつかる毎にボールの色が変わるようにコードを改良してみる。

以下は必要なコードを一部抜粋している。

```
// 色の情報を与えるための変数をグローバル変数として定義
let h = Math.random() * 360;
let hslcolor = `hsl(${h}, 80%, 60%)`;

// fillStyleにhsl値を代入
function colorChange(){
 ctx.beginPath();
 ctx.arc(x, y, ballRadius, 0, Math.PI * 2);
 ctx.fillStyle = hslcolor;
 ctx.fill();
 ctx.closePath();
}

// 関数の呼び出し
colorChange();

// 衝突を検出する毎に色を計算して、hsl値を更新する
if (x + dx > canvas.width - ballRadius || x + dx < ballRadius) {
  dx = -dx;
  h = Math.random() * 360;
  hslcolor = `hsl(${h}, 80%, 60%)`;

  }

if (y + dy > canvas.height - ballRadius || y + dy  < ballRadius) {
  dy = -dy;
  h = Math.random() * 360;
  hslcolor = `hsl(${h}, 80%, 60%)`;
  }
```

**解説**

**注意点**として、衝突検出時に`h = Math.random() * 360;`を実行した後に、その値をhsl値として代入することを忘れずに。

具体的には直後の変数`hslcolor`にhsl値を代入するコードを記述する必要がある。そうしないと色が変更されない。

あとポイントとしては、**ループの中にこの色を変更するコードを入れ込む**ことがあげられる。




