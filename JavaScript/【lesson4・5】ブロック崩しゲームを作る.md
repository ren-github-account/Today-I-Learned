*目次*
* [概要](#概要)
* [参考記事](#参考記事)
* [パドルを描画](#パドルを描画)
* [操作キーが押されているかどうかを判定](#操作キーが押されているかどうかを判定)
* [パドルの移動](#パドルの移動)
* [ゲームオーバーを実装](#ゲームオーバーを実装)
* [パドルをボールに当てる](#パドルをボールに当てる)
* [ブロックを記録する二次元配列を作成](#ブロックを記録する二次元配列を作成)

## 概要

lesson1・2・3の続き

## 参考記事

・MDN-パドルとキーボード操作

https://developer.mozilla.org/ja/docs/Games/Tutorials/2D_Breakout_game_pure_JavaScript/Paddle_and_keyboard_controls

・MDN-ゲームオーバーを実装する

https://developer.mozilla.org/ja/docs/Games/Tutorials/2D_Breakout_game_pure_JavaScript/Game_over

### パドルを描画

ボールにぶつかるパドルを描画するには以下のコードを書く。

```
<body>

<canvas id="myCanvas" width="480" height="320"></canvas>

<script>
// パドルの高さと幅と位置を定義
const paddleHeight = 10;
const paddleWidth = 150;
let paddleX = (canvas.width - paddleWidth) / 2;

// パドルの描画
function drawPaddle() {
  ctx.beginPath();
  ctx.rect(paddleX, canvas.height - paddleHeight, paddleWidth, paddleHeight);
  ctx.fillStyle = "#0095DD";
  ctx.fill();
  ctx.closePath();
}
</script>

</body>
```

**解説**

`let paddleX = (canvas.width - paddleWidth) / 2;`では、パドルが**画面の左端から何ピクセルの位置にあるか**を指定している。

なぜこの式になるのかというと、まず全体の幅が`canvas要素`で指定した`480`である点が重要。

パドルの幅は必ず確保されるので、全体の幅`480`からパドルの幅`150`を引く。

そして、残りの幅を2で割った値を画面の左端から何ピクセルの位置にあるかに指定することで、**右端からも同じ幅が確保されてパドルの位置がちょうど真ん中となる。**

あとは`rect()`の`canvas.height - paddleHeight`の部分は、前回壁への衝突の節でやった考え方と同じ。**画面の上端から何ピクセルの位置にあるか**を指定している。

### 操作キーが押されているかどうかを判定

以下のコードを書く。

```
// キーが指で押された時と離れた時のイベントハンドラーを設定
document.addEventListener("keydown", keyDownHandler, false);
document.addEventListener("keyup", keyUpHandler, false);

function keyDownHandler(e) {
  if (e.key === "Right" || e.key === "ArrowRight") {
    rightPressed = true;
  } else if (e.key === "Left" || e.key === "ArrowLeft") {
    leftPressed = true;
  }
}

function keyUpHandler(e) {
  if (e.key === "Right" || e.key === "ArrowRight") {
    rightPressed = false;
  } else if (e.key === "Left" || e.key === "ArrowLeft") {
    leftPressed = false;
  }
}
```

**解説**

`keyプロパティ`は**ユーザーが押したキーの情報を返す。** 

使用する時は上記のコードのように`e.key`のように引数と組み合わせて使う。

左右の矢印キーを押すとそれぞれ **`ArrowLeft`と`ArrowRight`** が`e.key`に渡される。

`Left`と`Right`に関してはブラウザ`IE/Edge`に対応するために指定している。

`keydownイベント`は**キーが押されたときに発生**し、`keyup イベント`は**キーが離された時に発生**する。

### パドルの移動

以下のコードを書く。(※以下は説明に必要な箇所のみを抜粋している)

```
<body>

<canvas id="myCanvas" width="480" height="320"></canvas>

<script>

// 変数の中身を表示するための要素を作成
const paddlexWatch = document.createElement("p");
paddlexWatch.id = "padlexWatch";
document.body.appendChild(paddlexWatch);

const paddlexWatch2 = document.createElement("p");
paddlexWatch2.id = "padlexWatch2";
document.body.appendChild(paddlexWatch2);

//パドルの高さと幅と位置を指定
const paddleHeight = 10;
const paddleWidth = 150;
let paddleX = (canvas.width - paddleWidth) / 2;

...(省略)

function draw(){
 ...(省略)

 // パドルを左右に移動させる
 if (rightPressed) {
  paddleX += 7;
　paddlexWatch2.textContent = "Watch2:" + paddleX;　/* 変数paddleXの中身を画面に表示 */
 } else if (leftPressed) {
  paddleX -= 7;
 }

// パドルがそれぞれ画面の右端と左端で止まるようにする
 if (rightPressed) {
    paddleX = Math.min(paddleX + 7, canvas.width - paddleWidth);
    paddlexWatch.textContent = "Watch1:" + paddleX; /* 右端で止まった時の変数paddleXの中身を表示 */

   } else if (leftPressed) {
  paddleX = Math.max(paddleX - 7, 0);
 }

}
</body>
```

**解説**

|||
|-|-|
|`Math.min()`|引数に渡された値の中から**最小の値を返す**メソッド。|
|`Math.max()`|引数に渡された値の中から**最大の値を返す**。|

`Math.min`の引数のうち`canvas.width - paddleWidth`は、具体的な数値を当てはめると`480 - 150 = 330`となって、**パドルが画面の左端から`330ピクセル`位置にある**ことを意味する。

ここはパドルの最初の位置を指定する時に使用した`let paddleX = (canvas.width - paddleWidth) / 2;`の部分を理解していれば難しくない。

つまり、`(canvas.width - paddleWidth) / 2`の計算結果は`330 / 2 = 165`となるので、パドル以外の空白の部分の幅は`165 x 2 = 330`が限界値となる。

 `if (rightPressed)`の部分は条件が同じif文が2つあって同時に動いているように見えるが、これは実行速度か速すぎるためにそう見えるだけだと思う。

 なにしろ`setInterval(draw, 10);`でなんと1秒の100分の1の速さで関数を実行させているのだから。

画面の右端にパドルが到達した時の変数`paddleX`の中身を見ると、`330`となっていて`Math.min`がしっかりと機能していることがわかる。

あとは、`Math.min`の`paddleX + 7`と、`Math.max`の`paddleX - 7`はどちらも **`paddleX`だけにしても問題なく動く。** なぜ`paddleX + 7`や`-7`としているのかはちょっとよく分からない。


### ゲームオーバーを実装

ここの部分のコードは以下の箇所以外とくに疑問点はなかった。

**用語**
||意味|
|-|-|
|`location.reload()`|現在のURLを再読み込みするためのメソッド。|
|`clearInterval()`|`setInterval()`の繰り返し動作を停止する。使い方はまず`const interval = setInterval(draw, 10);`のようにして`setInterval()`の戻り値(識別子)を変数に代入した後に、その変数を引数に設定する。今あげた例だと`clearInterval(interval);`とすればok。|

以下の部分のコードは、MDNの解説ページでは`document.location.reload();`が`clearInterval(interval);`より先に記述されているが、**逆にした方が順番的何が起こっているか分かりやすい。**

```

if (y + dy  < ballRadius) {
  dy = -dy;
 } else if (y + dy > canvas.height - ballRadius ) {
   alert("GAME OVER");
   clearInterval(interval); /* MDNの解説ではここの順番が逆になっている */
   document.location.reload();
    }
```

### パドルをボールに当てる

```
if (y + dy < ballRadius) {
  dy = -dy;
} else if (y + dy > canvas.height - ballRadius) {
  if (x > paddleX && x < paddleX + paddleWidth) {
    dy = -dy;
  } else {
    alert("GAME OVER");
    document.location.reload();
    clearInterval(interval);
  }
}
```

`canvas`画面の下端にボールが当たった時、`if (x > paddleX && x < paddleX + paddleWidth)`でパドルの左端から右端の間にボールがあるかどうかをチェックしている。

もしそれが`true`であったなら符号を反転させボールが跳ね返る。

しかし、もし`false`であったなら`GAME OVER`となる。

if文の`paddleX + paddleWidth`の部分を理解するには以下のイメージを思い浮かべると良い。

```
| paddleX | paddlewidth | paddleX |
```

変数`paddleX`の値は最初に`let paddleX = (canvas.width - paddleWidth) / 2;`と定義しているため画面の幅のうち **`paddlewidth`以外の左右の2領域を表している。**

ということは、`paddleX`でパドルの左端を表し、`paddleX + paddleWidth`でパドルの右端を表すことになる。

### ブロックを記録する二次元配列を作成

```
// ブロックを描画するための変数を設定
const brickRowCount = 3;
const brickColumnCount = 5;
const brickWidth = 75;
const brickHeight = 20;
const brickPadding = 10;
const brickOffsetTop = 30;
const brickOffsetLeft = 30;
const bricks = [];

// ブロックを記録する二次元配列を作成
for (let c = 0; c < brickColumnCount; c++) {
  bricks[c] = [];
  for (let r = 0; r < brickRowCount; r++) {
    bricks[c][r] = { x: 0, y: 0 };
  }
}
```

**解説**

多次元配列は**配列の中に配列が格納されている状態のこと**を指し、**二次元配列はその多次元配列の一種。** ( 参照:[javadrive](https://www.javadrive.jp/javascript/array/index5.html) )

上記のコードは、変数`brickColumnCount`で**縦の列**を設定し、変数`brickRowCount`で**横の行**を設定している。

`{ x: 0, y: 0 }`の部分では、オブジェクトリテラル`{}`を使用している。

そして、ブロックを描画位置を指定するための`x座標`と`y座標`を作成している。


