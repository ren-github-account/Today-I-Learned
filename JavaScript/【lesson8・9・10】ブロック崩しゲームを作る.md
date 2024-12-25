*目次*
* [概要](#概要)
* [参考記事](#参考記事)
* [スコアをカウントする](#スコアをカウントする)
* [canvas要素とmargin](#canvas要素とmargin)

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

・`canvas要素`の直下に以下のコードの`box`を作成して、`margin-top:10px;`を設定しても`canvas要素`の左側の`margin`に変化は無い

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
