*目次*
* [alert](#alert)
* [document.write](#document.write)
* [変数の宣言](#変数の宣言)
* [定数](#定数)
* [関数](#関数)
* [if文](#if文)
* [switch文](#switch文)
* [配列](#配列)
* [for文](#for文)
* [while文](#while文)

### alert

```
alert(*);
```

alertを使うことで画面に文字列や数値をポップアップで表示させることができる。

使い方のルール
* 文字列はクォーテーションで囲むが、数値にクォーテーションは不要。
* 文字列や数値をつなげる時は`+`記号を使用。
* `\n`で改行できる。

**具体例**

```
alert( "円周率は\n" + 3.14159 );  //文字列を改行させる
```

### document.write

```
document.write(*);
```
document.write()を使ってもalertと同じように文章などを表示させることができる。こちらはポップアップ表示ではない。

`\n`では改行できなかったけど、以下のように`<br>`を使うことで改行できた。

```
document.write("<strong>10+20</strong>の計算結果は…<br>");
```

### 変数の宣言

変数を使うには宣言を行う必要がある。

```
var 変数;
var 変数A,変数B,変数C;
var 変数 = 値;
```

1行目は、単に変数を１つだけ宣言。
2行目は、複数の変数をコンマで区切って一括で宣言。
3行目は、変数を宣言すると同時に、変数に値を代入している。

**補足その1**

文字列と数字を「+」で繋げると、数字は文字として扱われる。

そのため`計算結果:3`と表示しようとして以下のように書くと`計算結果:12`と表示される。
```
var num1 = 1;
var num2 = 2;

document.write("計算結果:" + num1 + num2);
```

**補足その2**

関数の中で宣言した変数は、その関数の中でしか使うことができない。これを**ローカル変数**と呼ぶ。

### 定数

定数の宣言には`const`を使用する。

```
const TEISU = 1;

document.write( TEISU+"<br>" ); 
```

### 関数

```
function 関数名(引数) {
 実行したい処理
}
```

引数の部分は使わない時は空のままでok。

**具体例**

```
function fncCall()
{
  var message = "関数が呼び出されました！";
  alert(message);
}
```

### if文

**具体例**

```
var num1 = 10; //ここの変数に代入する値を変更することで出力結果が変わる

function moshimo() {
 if( num1 > 3) {
  alert(num1 + "は3より大きいね");
 }
}

// 以下のコードはhtmlのbody内に記述
<button type="button" onclick=" moshimo()">ボタン</button>
```

他にも、if~else文やelse if文がある。ここら辺は他のプログラミング言語と同じだね。

### switch文

書き方はPHPと同じなので説明は割愛。

### 配列

**配列の宣言**

```
var 配列名 = new Array()
```

**具体例**

```
// 配列内に本当に値が入っているか確認するためのコード
var num1 = new Array("月","火","水");

function day() {
 alert(num1[0]);
 alert(num1[1]);
 alert(num1[2]);
}

// 以下のコードはhtmlのbody内に記述
<button type="button" onclick="day()">ボタン</button>
```

### for文

**具体例**

```
// 1週間の曜日一覧を表にして出力するプログラム
var num1 = new Array("曜日一覧","月","火","水","木","金","土","日");

function day() {
 document.write("<!DOCTYPE html>");
 document.write("<table border=\"2\">"); // 「"2"」を認識させるためエスケープを使用している
 document.write("<tr><th>" + num1[0] + "</th><tr>");
 for( var i = 1; i < 8; i++){
  document.write("<tr><td>" + num1[i] + "</td></tr>");
 }
 document.write("</table>");
}

// 以下のコードはhtmlのbody内に記述
<button type="button" onclick="day()">ボタン</button>
```

**for文の中でbreakとcontinueを使用できる**

||意味|
|-|-|
|break|繰り返し文から抜け出す。|
|continue|continueより下の処理をスルーして、繰り返し文の先頭に戻る。|

```
// 火、水、金だけ表示されるプログラム
var num1 = new Array("曜日一覧","月","火","水","木","金","土","日");

function day() {
 document.write("<!DOCTYPE html>");
 document.write("<table border=\"1\">");
 document.write("<tr><th>" + num1[0] + "</th><tr>");
 for( var i = 1; i < 8; i++){
  if( i == 1 || i == 4) continue;　// ここ
  if(i == 6) break; // ここ
  document.write("<tr><td>" + num1[i] + "</td></tr>");
 }
 document.write("</table>");
}
```

`||`は「または」の意味。

### while文

書き方はJavaと同じ。

**具体例**

```
// うんちを10回表示するプログラム
var num = 0;

function day() {
 document.write("<ol>"); // リストタグを使って番号が出るようにした
 while (num < 10) {
  document.write("<li>");
  document.write("ぷゆゆ" + "💩" + "置いとくね" + "<br>");
  document.write("</li>");
    num ++;
}
 document.write("</ol>");
}
```
