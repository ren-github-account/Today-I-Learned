*目次*
* [概要](#概要)
* [参考記事](#参考記事)
* [スコアをカウントする](#スコアをカウントする)
* [canvas要素とmargin](#canvas要素とmargin)
* [マウス操作](#マウス操作)
* [ライフを実装する](#ライフを実装する)

## 概要

lesson4・5・6・7の続き

## 参考記事

・MDN-スコアと勝ち負けを記録する

https://developer.mozilla.org/ja/docs/Games/Tutorials/2D_Breakout_game_pure_JavaScript/Track_the_score_and_win

### スコアをカウントする

**用語**
||意味|
|-|-|
|`font`|テキストの大きさや太さ、フォントの種類などのテキストスタイルを指定できるプロパティ。実際に書く時は`ctx.font = "bold 48px serif";`のように書く。`serif`は **「総称フォントファミリー」** と呼ばれるもので、同じ種類の書体をひとつにまとめたもののこと。フォントを指定する時はこの **「総称フォントファミリー」を少なくとも1つは指定しておくこと**が推奨される。こうすることで利用者が指定したフォントをどれも利用できなかった場合にこちらの意図したフォントに近い形のフォントを表示させることが可能となる。( 参照:[MDN](https://developer.mozilla.org/ja/docs/Web/CSS/font-family) )|
|`fillText()`|指定した座標にテキスト文字列を描画し、その文字を現在の`fillStyle`で塗りつぶすメソッド。使う時は`fillText(text, x, y)`のように書く。`text`で描画したいテキストを指定し、`x`と`y`で描画開始位置の`x座標`と`y座標`を指定する。|
|`++`|インクリメント演算子といって、オペランド(演算対象)を`1`加算して返す。`a++`のように`++`を後ろに付ける形を**後置インクリメント**と呼び、注意すべきなのは**戻り値はインクリメント前の値を返す**ということ。具体例:`let x = 3; y = x++; // 実行結果 y = 3 x = 4 `|

インクリメント演算子が通常の加算とどう違うのかが気になったので、以下のコードで比較してみた。

```
// 通常の加算
let xIncr = 3;
xIncr += 1

console.log(xIncr); // 実行結果 4

// 後置インクリメント演算子
let yIncr = 3;
yIncr++;

console.log(yIncr); // 実行結果 4
```

上記のコードで検証した結果、どちらも実行結果は同じになるが**インクリメント演算子の方がシンプルに記述できる**。

### canvas要素とmargin

・`canvas要素`の直下に以下のコードの`box`を作成し`margin-top:10px;`を設定しても`canvas要素`の左側の`margin`に変化は無い

```
// HTML
<div id="test"></div>

// CSS
#test {
 width:600px;
 height:160px;
 background:blue;
}
```
・上記の`box`の直下に変数の中身を表示させる以下の三行を **`button要素`は無し**で`p要素`で追加しても同様に変化は無い。

```
// 変数の中身を表示
　yWatch.textContent = "relativeX:" + relativeX;
  byWatch.textContent = "e.clientX:" + e.clientX;
  heightWatch.textContent = "canvas.offsetLeft:" + canvas.offsetLeft;
```
・今度は上記の三行を **`button要素`あり**で`追加すると、**とうとう変化があった!**

```
// 変化内容
// canvas要素の左側のmargin 変化前
274.667

// 変化後
266.333
```
この原因はブラウザ画面の左側に**スクロールバーが表示されていたことが原因だった。**

こんな単純なことなのに気づかずに2時間くらい経過してしまったw

一応頑張って検証用のコードを書いたので以下に載せておく。
```
<body>
// box
<div id="test"></div>

<button id="add">margin-topを追加</button><button id="reset">戻す-margin</button><button id="add2">変数追加</button><button id="reset2">戻す-変数</button>

<script>
// 変数yの中身を表示するための要素を作成
const yWatch = document.createElement("p");
yWatch.id = "yWatch";
document.body.appendChild(yWatch);

const byWatch = document.createElement("p");
byWatch.id = "byWatch";
document.body.appendChild(byWatch);

const heightWatch = document.createElement("p");
heightWatch.id = "heightWatch";
document.body.appendChild(heightWatch);


// ボタン
const button = document.querySelector("#add");
const test = document.getElementById("test");
const reset = document.getElementById("reset");
const add2 = document.getElementById("add2");
const reset2 = document.getElementById("reset2")

// margin-topを追加＆戻す
function marginAdd() {
 test.style.marginTop = "10px";
}

function marginReset(){
 test.style.marginTop = "";
}

// 変数を表示させる行を追加＆戻す
function VariableAdd(){
 
  heightWatch.textContent = "canvas.offsetLeft:";
}

function VarReset() {
  heightWatch.textContent = "";

}

button.addEventListener("click", marginAdd);
reset.addEventListener("click", marginReset);
add2.addEventListener("click", VariableAdd);
reset2.addEventListener("click", VarReset);
</script>

</body>
```

### マウス操作

**用語**
||意味|
|-|-|
|`clientX`|ブラウザ画面上にあるマウスカーソルなどの`x座標`を表示するプロパティ。一方`clientY`は`y座標`を表示する。|
|`offsetLeft`||

```
function mouseMoveHandler(e) {
  const relativeX = e.clientX - canvas.offsetLeft;
  if (relativeX > 0 && relativeX < canvas.width) {
    paddleX = relativeX - paddleWidth / 2;
  }
}
```

**解説**

`e.clientX - canvas.offsetLeft`は、マウスカーソルの`x座標`からゲーム画面(`canvas要素`)の左側にある空白部分の距離を引くことで、ゲーム画面の左端からマウスカーソルまでの位置を表している。

具体的には以下になる。

```
// マウスカーソルのx座標(e.clientX)
266

// ゲーム画面左側の空白部分の距離(canvas.offsetLeft)
266

// 計算結果
relativeX = e.clientX - canvas.offsetLeft = 266 - 266 = 0

// つまり
relativeX　= 0
```

上記の場合、`relativeX = 0`となって**マウスカーソルがゲーム画面(`canvas要素`)のちょうど左端に位置する**ことを意味している。

`relativeX　= 0`の時点ではまだパドルの位置は最初に描画される画面の下端の真ん中の位置から動かないが`relativeX　= 1`となった瞬間、条件式より`paddleX = relativeX - paddleWidth / 2`が実行され**パドルは画面の左端にほとんど半分めり込む状態になる。**

この理由は`paddleWidth`は`150`に設定しているため、パドルのx座標が`paddleX = 1 - 75 = -74`となるためだ。

if文の条件を`relativeX > 0 `から`relativeX > 74 `へ変更すると、`paddleX = 75 - 75 = 0`となって**パドルが画面の左端でピタリと止まるようになる。**

右端でも左端と同じように半分ほどめり込んで止まるのはなぜかを見ていく。

右端の条件式は`relativeX < canvas.width`となっていて、`canvas.width`の最大値は`480`となる。となると、実行される最終の値は`relativeX = 479`の時になる。

この時の実行式は`paddleX = relativeX - paddleWidth / 2`なので具体的な数値を代入すると`paddleX = 479 - 75 = 404`。

つまり、パドルの左端からの位置の最大値は`404`となる。

`canvas要素`の幅は`480`なので`480 - 404 = 76`となるので、画面内のパドルの大きさは`76`で、**画面外のパドルの大きさは残りの`74`となる。**

要は**左端と右端ともにパドルは`74`はみ出た地点で動きが止まる。**

**マウスカーソルが常にパドルの真ん中にあるのは**、パドルの`x座標`が`0`の時に`canvas要素`の左端からマウスカーソルまでの距離がちょうどパドルの半分の位置になるように`paddleX = relativeX - paddleWidth / 2`によって調整されているから。

### ライフを実装

**用語**

||意味|
|-|-|
|`!`|if文などに登場するビックリマーク論理否定と呼ばれ、**`true`と`false`をひっくり返す機能を持つ。** つまり、`true`だったら`false`となり、`false`だったら`true`となる。|
|`偽値(falsy)`|if文などで使用される時に`false`に変換される値のこと。具体例:`null`、`NaN`、`0`、空文字列 (`""`または`''`など)、`undefined`|




