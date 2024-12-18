*目次*
* [概要](#概要)
* [参考記事](#参考記事)
* [canvas要素](#canvas要素)
* [図形を描画](#図形を描画)
* [独立した四角形を描画](#独立した四角形を描画)
* [円を描画](#円を描画)
* [縁にだけ色を塗る](#縁にだけ色を塗る)

### 概要

JavaScriptを使ってブロック崩しゲームを作成するのに必要な知識をまとめていく。

### 参考記事

・MDN-キャンバスを作ってその上に描画する

https://developer.mozilla.org/ja/docs/Games/Tutorials/2D_Breakout_game_pure_JavaScript/Create_the_Canvas_and_draw_on_it


### canvas要素

`canvas要素`は画面上に**グラフィックやアニメーションを描画**する時に使われる。

**使い方**

基本的に`document.querySelector()メソッド`と`getContext()メソッド`と一緒に使用される。

具体的には以下のように書く。　

```
const canvas = document.querySelector("canvas");
const ctx = canvas.getContext("2d");
```

**・解説**

まず、`document.querySelector()メソッド`で`canvas要素`を取得。

その後`getContext()メソッド`の引数を`"2d"`と設定することで、`CanvasRenderingContext2Dオブジェクト`が作成される。

この`CanvasRenderingContext2Dオブジェクト`の持つ色々なメソッドを組み合わせることで、画面にドットを描いたり、その他にも図形、文字、画像を描画したりすることができるようになる。

### 図形を描画

実際に図形を描画するには、上記のコードにたいして以下のように追記する。

```
const canvas = document.querySelector("canvas");
const ctx = canvas.getContext("2d");

// 図形を描画
ctx.fillStyle = "green";
ctx.fillRect(10, 10, 100, 100);
```

**解説**
||意味|
|-|-|
|`fillStyle`|図形の色を指定するプロパティ。|
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
|`rect()`|**ただ四角形の作成のみを行う**メソッド。そのため長方形の作成と`canvas`への描画を同時に行いたい場合は`fillRect()`を使用する。|
|`fill()`|`rect()`で作成した長方形を`canvas`に描画する時に使用する。|

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
|`stroke()`|線を描くメソッド。|
|`strokeStyle()`|`stroke()`で描いた**輪郭線の色**を指定する。デフォルト値は`#000`(黒色）。|




