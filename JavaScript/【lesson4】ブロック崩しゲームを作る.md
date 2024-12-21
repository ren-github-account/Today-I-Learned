*目次*
* [概要](#概要)
* [参考記事](#参考記事)
* [パドルを描画](#パドルを描画)
* [操作キーが押されているかどうかを判定](#操作キーが押されているかどうかを判定)

## 概要

lesson1・2・3の続き

## 参考記事

・MDN-パドルとキーボード操作

https://developer.mozilla.org/ja/docs/Games/Tutorials/2D_Breakout_game_pure_JavaScript/Paddle_and_keyboard_controls

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

使用する時は上記のコードのように`e.key`のように引数を組み合わせて使う。

左右の矢印キーを押すとそれぞれ`ArrowLeft`と`ArrowRight`が`e.key`に渡される。

`Left`と`Right`に関してはブラウザ`IE/Edge`に対応するために指定している。

`keydownイベント`は、キーが押されたときに発生する。






