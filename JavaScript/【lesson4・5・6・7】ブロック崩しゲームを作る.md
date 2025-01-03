*目次*
* [概要](#概要)
* [参考記事](#参考記事)
* [パドルを描画](#パドルを描画)
* [操作キーが押されているかどうかを判定](#操作キーが押されているかどうかを判定)
* [パドルの移動](#パドルの移動)
* [ゲームオーバーを実装](#ゲームオーバーを実装)
* [パドルをボールに当てる](#パドルをボールに当てる)
* [ブロックを記録する二次元配列を作成](#ブロックを記録する二次元配列を作成)
* [ブロックを描画](#ブロックを描画)
* [ブロックへの衝突を検出](#ブロックへの衝突を検出)
* [本当に4番目の条件を満たしているか](#本当に4番目の条件を満たしているか)
* [ブロックが消えるようにする](#ブロックが消えるようにする)
* [なぜ0か1かで消えるのか](#なぜ0か1かで消えるのか)

## 概要

lesson1・2・3の続き

## 参考記事

・MDN-パドルとキーボード操作

https://developer.mozilla.org/ja/docs/Games/Tutorials/2D_Breakout_game_pure_JavaScript/Paddle_and_keyboard_controls

・MDN-ゲームオーバーを実装する

https://developer.mozilla.org/ja/docs/Games/Tutorials/2D_Breakout_game_pure_JavaScript/Game_over

・MDN-ブロックのかたまりを作る

https://developer.mozilla.org/ja/docs/Games/Tutorials/2D_Breakout_game_pure_JavaScript/Build_the_brick_field

・MDN-衝突検出

https://developer.mozilla.org/ja/docs/Games/Tutorials/2D_Breakout_game_pure_JavaScript/Collision_detection

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

### ブロックを描画

```
function drawBricks() {
  for (let c = 0; c < brickColumnCount; c++) {
    for (let r = 0; r < brickRowCount; r++) {
      const brickX = c * (brickWidth + brickPadding) + brickOffsetLeft;
      const brickY = r * (brickHeight + brickPadding) + brickOffsetTop;
      bricks[c][r].x = brickX;
      bricks[c][r].y = brickY;
      ctx.beginPath();
      ctx.rect(brickX, brickY, brickWidth, brickHeight);
      ctx.fillStyle = "#0095DD";
      ctx.fill();
      ctx.closePath();
    }
  }
}
```

**解説**

縦の列を表す`for (let c = 0; c < brickColumnCount; c++)`の中に横の行を表す`for (let r = 0; r < brickRowCount; r++)`が入っている。

この実行の順番はどうなっているかというと、まず縦列の`for (let c = 0; c < brickColumnCount; c++)`が実行されて直下の横の行を表す`for (let r = 0; r < brickRowCount; r++)`が実行される。

そして**横の行を表す**変数`brickRowCount`の値は`3`なので`for (let r = 0; r < brickRowCount; r++)`の処理が3回実行されてからやっとループから抜けて、再び外側にある`for (let c = 0; c < brickColumnCount; c++)`(**縦列を表す**)が実行され、再び直下の`for (let r = 0; r < brickRowCount; r++)`(**横の行を表す**)の処理が3回実行…(以下同じ)となる。

なので描画のイメージは縦の列が全部で5列あって、その1列ごとに3つのブロックが描かれていくという風になる。

そして、ブロックの描画開始位置である`x座標`と`y座標`を指定するための以下の式がぱっと見理解しにくいので解説していく。

```
const brickX = c * (brickWidth + brickPadding) + brickOffsetLeft;
const brickY = r * (brickHeight + brickPadding) + brickOffsetTop;
```

まず、画面の左端から何ピクセルの位置にあるかを算出する以下の1番目の式から見ていく。

```
// 画面の左端から何ピクセルの位置かを算出
const brickX = c * (brickWidth + brickPadding) + brickOffsetLeft;

// 式に具体的に数値を当てはめてみる
0 * (75 + 10) + 30
1 * (75 + 10) + 30
2 * (75 + 10) + 30
```
式に具体的な数値を当てはめた結果を表にまとめてみた。

||描画位置|
|-|-|
|`c = 0`|画面の左端から`30ピクセル`の位置から描画開始|
|`c = 1`|画面の左端から`30ピクセル`の位置にブロックの幅`75ピクセル`とブロック同士の隙間を表す`10ピクセル`を加えた位置から描画開始|
|`c = 2`|画面の左端から`30ピクセル`の位置に、今度は2つ分のブロックの幅`75ピクセル`とそのブロックの隙間`10ピクセル`を加えた位置から描画開始|

次は、画面の上端から何ピクセルの位置にあるかを表す2番目の式について見ていく。

```
// 画面の上端から何ピクセルの位置かを算出
const brickY = r * (brickHeight + brickPadding) + brickOffsetTop;

// 式に具体的に数値を当てはめてみる
0 * (20 + 10) + 30
1 * (20 + 10) + 30
2 * (20 + 10) + 30
```

仕組みは1番目の式と同じで、今度は基準が上端に変わっただけだ。

左端の時とやってることは同じなので説明は省略。

ここまで出来たら後は算出した`x座標`と`y座標`をオブジェクトに追加する。

その追加を行っているのが以下のコード。

```
bricks[c][r].x = brickX;
bricks[c][r].y = brickY;
```

上記のコードは二次元配列となっていてイメージしづらい。

そこで以下のイメージを持つと良い。( 参照:[javadrive](https://www.javadrive.jp/javascript/array/index5.html) )

```
// ブロックの描画開始位置をそれぞれ保存したオブジェクトを収納する配列
bricks = [ 
 [{x:0, y:0}, {x:0, y:0}, {x:0, y:0}]
 [{x:0, y:0}, {x:0, y:0}, {x:0, y:0}]
 [{x:0, y:0}, {x:0, y:0}, {x:0, y:0}]
　　...(省略)
 ];

// 二次元配列にアクセス
bricks[0][0] = {x:0, y:0};
bricks[0][1] = {x:0, y:0};
bricks[0][2] = {x:0, y:0};
```

### ブロックへの衝突を検出

ブロックへの衝突を検出するには以下の4つ条件が全て`true`である必要がある。

```
・ボールの x 座標がブロックの x 座標より大きい

・ボールの x 座標がブロックの x 座標とその幅の和より小さい

・ボールの y 座標がブロックの y 座標より大きい

・ボールの y 座標がブロックの y 座標とその高さの和より小さい
```

こうして並べて書くと何やらややこしく見えるが、言っていることは単純で**要はボールがブロックの左端と右端そして上端と下端の間の範囲内にめり込んでいるかどうか**を判定すれば良い。

注意点としては、4番目の条件文の`ボールの y 座標がブロックの y 座標とその高さの和より小さい`は、壁への衝突判定の時と同じイメージで考えていると**ハマる**ということ。

そもそもブロックへの衝突を検出をしなければ、ボールはブロックを素通りして動くことを考えるとわかりやすいが、上記の4つの条件での判定ではボールはブロックへの衝突時、**想像よりだいぶめり込んでいる。** (ブロックに当たると跳ね返るというと、ついブロックの端にちょっと触れたら跳ね返るイメージを想像しがちだがこの場合はそうではない)

このことは実際にMDNのページにある「ブロック崩しゲーム」の完成版をよく観察してみると確認できる。( 参照:[完成版のリンク](https://breakout.enclavegames.com/lesson10.html) )

### 本当に4番目の条件を満たしているか

本当に4番目の条件を満たしているかどうか確認するには、以下のように衝突判定のコードを変更すれば良い。

```
// ボールのy座標、ブロックのy座標とその高さを表示するための要素を作成
const yWatch = document.createElement("p");
yWatch.id = "yWatch";
document.body.appendChild(yWatch);

const byWatch = document.createElement("p");
byWatch.id = "byWatch";
document.body.appendChild(byWatch);

const heightWatch = document.createElement("p");
heightWatch.id = "heightWatch";
document.body.appendChild(heightWatch);

// ブロックへの衝突を検出
 if (x > b.x && x < b.x + brickWidth && y > b.y && y < b.y + brickHeight) {
　　　　　// 衝突の瞬間にボールの動きを止める
         clearInterval(interval);
　　　　　// ボールのy座標、ブロックのy座標とその高さを表示
         yWatch.textContent = "y:" + y;
         byWatch.textContent = "b.y:" + b.y;
         heightWatch.textContent = "brickHeight:" + brickHeight;

        /* 
        dy = -dy;
　　　　 */
      }
```

上記のようにコードを変更して実行すると、見事4番目の条件を満たしていることが確認できる。

**比較用**に**上端の壁への衝突検出をブロックへの衝突検出に応用したやり方**も載せておく。

```
// ブロックの数を1つだけにして幅と高さを大きくした
const brickRowCount = 1;
const brickColumnCount = 1;
const brickWidth = 450;
const brickHeight = 85;
const brickOffsetTop = 30;

// y + dy < brickOffsetTop + brickHeightでブロックへの衝突を検出
if (y + dy  < ballRadius || y + dy < brickOffsetTop + brickHeight) {
  /* 
  dy = -dy;
　 */

   // 衝突の瞬間にボールの動きを止める
   clearInterval(interval);
   // ボールのy座標、ブロックのy座標と高さを表示
   yWatch.textContent = "y:" + y;
   byWatch.textContent = "brickOffsetTop:" + brickOffsetTop;
   heightWatch.textContent = "brickHeight:" + brickHeight;
   

 } else if (y + dy > canvas.height - ballRadius ) {
  if (x > paddleX && x < paddleX + paddleWidth) {
    dy = -dy;
   } else {
   alert("GAME OVER");
   clearInterval(interval);
   document.location.reload();
    }
 }
```

上記のコードは実行すると今度は、y座標の方が大きくなって**4番目の条件を満たさない**ことがわかる。

これはブロックへのめり込み具合がこちらの方が少ないために起こる。

あと`brickHeight`の高さを変更すると**奇妙なことが起きる。**

`brickOffsetTop + brickHeight`の和より`y座標`の値の方が`1`大きくなったり、あるいは和と`y座標`の値が同数になったりする。

この現象が起こる理由は以下のコードで検証したように、**偶数か奇数の違い**だと考えられる。

```
// 和が奇数の場合
y:116
brickOffsetTop:30
brickHeight:85

// 和が偶数の場合
y:120
brickOffsetTop:30
brickHeight:90

// 和が奇数の場合
y:128
brickOffsetTop:30
brickHeight:97
```

### ブロックが消えるようにする

まず、ブロックを画面に描画するかどうかを判定するためのプロパティを追加する。

具体的には、以下のコードのようにオブジェクト内にプロパティとして`status:1`を新しく追加する。
```
const bricks = [];
for (let c = 0; c < brickColumnCount; c++) {
  bricks[c] = [];
  for (let r = 0; r < brickRowCount; r++) {
    bricks[c][r] = { x: 0, y: 0, status: 1 }; // status:1の部分を新しく追加
  }
}
```

そして、ブロックを描画する関数内の処理に以下のような追記を行う。(**注意:`}`を忘れないように** )

すなわち`statusプロパティ`の値が`1`ならブロックの描画するが、そうでないなら描画しないとなる。

```
function drawBricks() {
  for (let c = 0; c < brickColumnCount; c++) {
    for (let r = 0; r < brickRowCount; r++) {
      if (bricks[c][r].status === 1) { // 追記箇所
        const brickX = c * (brickWidth + brickPadding) + brickOffsetLeft;
        const brickY = r * (brickHeight + brickPadding) + brickOffsetTop;
        bricks[c][r].x = brickX;
        bricks[c][r].y = brickY;
        ctx.beginPath();
        ctx.rect(brickX, brickY, brickWidth, brickHeight);
        ctx.fillStyle = "#0095DD";
        ctx.fill();
        ctx.closePath();
      }
    }
  }
}

```

ここまで完了したらあとは、衝突検出の処理にいくつかの追記を行う。

まず`b.status === 1`でブロックが描画されている状態かどうかを確認してそれが`true`ならば衝突検出を行う。

そして衝突検出も`true`ならば`b.status = 0;`と設定し直すことで描画処理を停止する。

```
function collisionDetection() {
  for (let c = 0; c < brickColumnCount; c++) {
    for (let r = 0; r < brickRowCount; r++) {
      const b = bricks[c][r];
      if (b.status === 1) { // 追記1
        if (
          x > b.x &&
          x < b.x + brickWidth &&
          y > b.y &&
          y < b.y + brickHeight
        ) {
          dy = -dy;
          b.status = 0;　// 追記2
        }
      }
    }
  }
}
```

### なぜ0か1かで消えるのか

ところで、今見てきたコードを見てこう思った人もいるだろう。

**なぜ、`status:1`としたり`status:0`としたりするだけでブロックが消えるのかと。**

この答えは、**ブロックが同じ位置に永遠と上塗りされながら生成されているから**、となる。

このことはゲーム画面を眺めているだけでは分からない。

そこで、ブロックが描画される毎にランダムに色を変えるようにコードを変更してみた。

もし、**ブロックが同じ位置に永遠と上塗りされながら生成されている**という説が正しければ、色は指定したミリ秒ごとに変わるはずである。

```
// ブロックの数を1個にしてwidthとheightを大きくした
const brickRowCount = 1;
const brickColumnCount = 1;
const brickWidth = 400;
const brickHeight = 90;

// ブロックを描画
function drawBricks() {
  for (let c = 0; c < brickColumnCount; c++) {
    for (let r = 0; r < brickRowCount; r++) {
      const brickX = c * (brickWidth + brickPadding) + brickOffsetLeft;
      const brickY = r * (brickHeight + brickPadding) + brickOffsetTop;
      bricks[c][r].x = brickX;
      bricks[c][r].y = brickY;
      ctx.beginPath();
      ctx.rect(brickX, brickY, brickWidth, brickHeight);
      let h = Math.random() * 360; // ここからが変更箇所
      let hslcolor = `hsl(${h}, 80%, 60%)`;
      ctx.fillStyle = hslcolor;
      ctx.fill();
      ctx.closePath();
    }
  }
}

// 色の変化を分かりやすくためため再描画の間隔を0.5秒に
const interval = setInterval(draw, 500);
```

上記のコードを実行すると仮説通り、**ブロックの色が`0.5秒`ごとに上塗りされて変わる**ことが確認できる。
