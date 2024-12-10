*目次*
* [やりたいこと](#やりたいこと)
* [完成コード](#完成コード)
* [evalの挙動](#evalの挙動)
* [createTextNodeはオブジェクトとなって実行されない](#createTextNodeはオブジェクトとなって実行されない)
* [evalで文字を数値に変換](#evalで文字を数値に変換)
* [前知識](#前知識)
* [JS簡易実行環境のコード](#JS簡易実行環境のコード)
* [解説](#解説)
* [value](#value)
* [textarea要素](#textarea要素)
* [eval](#eval)
* [evalに関連する用語](#evalに関連する用語)
* [関連する用語その2](#関連する用語その2)
* [String関数の使い方](#String関数の使い方)
* [Symbolの使い方](#Symbolの使い方)
* [Symbol.iteratorの使い方](#Symboliteratorの使い方)
* [用語解説Symboliterator](#用語解説Symboliterator)
* [本当にイテレータオブジェクトなのか検証](#本当にイテレータオブジェクトなのか検証)

## やりたいこと

`eval()`プロパティを使用してブラウザ上で動くJSの簡易実行環境を作成したのち、`createTextNode()メソッド`を使用したXSS対策を自分で確かめてみる。

【参考】

・JSの簡易実行環境の作り方

https://blog.goo.ne.jp/jeans201/e/3eb2b3a9303916b49175309f7d890f55

・`createTextNode()メソッド`を使ったXSS対策について

https://upa-pc.blogspot.com/2015/02/javascript-dom-based-xss-protect-createTextNode.html

### 完成コード

```
<body>

JavaScriptコード:<br>

<textarea id="JsView" rows="20" cols="50"></textarea><br>
<input type="button" value="実行"/>

<script>

const testJScodeRun = function JScodeRun() {
  
  const JS = document.getElementById("JsView").value;
  console.log(JS);

  /*
　コメントアウト部分
  const changeJS = document.createTextNode(JS);
  console.log(changeJS);
　 */
　
  eval(JS);

}

const input = document.querySelector("input");
input.addEventListener("click", testJScodeRun);


</script>

</body>
```

**解説**

上記のコードのまま実行すると、下の「JS簡易実行環境のコード」の節で書いているコードと同じ動作になるが、以下の変更を加えると **`eval`が実行されなくなる。**

変更を加えるには、まず「コメントアウト部分」のコメントアウトを解除した後、evalの引数を変数名`changeJS`に変更すればok。変更後、実際に実行してみるとJavaScriptコードが実行されないことがわかる。

`console.log();`の部分は`eval`の挙動を確かめるために変数の中身を表示させている。

`.value`を使用して`textarea要素`の値を取得すると、**その値は文字列で取得される。** ( 参照:[MDN](https://developer.mozilla.org/ja/docs/Web/API/HTMLTextAreaElement/value) )

### evalの挙動

上記の完成コードにてなぜ`eval`が実行されなくなるのかを考えていたところ、`eval`の挙動が気になったので以下のコードを書いて確かめてみた。

```
<body>

JavaScriptコード:<br>

<textarea id="JsView" rows="20" cols="50">"こんにちわ"</textarea><br>
<input type="button" value="実行"/>

<script>

const testJScodeRun = function JScodeRun() {
  
  const JS = document.getElementById("JsView").value;
  console.log("JS:",JS);
  console.log("typeof:",typeof JS);
  /* 
  const changeJS = document.createTextNode(JS);
  console.log(changeJS);
　 */
　
  const test1 = eval(JS);　/* ここの戻り値に注目  */
  console.log("test1:",test1);
　console.log(typeof test1);
  
  const test2 = eval(test1);
  console.log(test2);
  console.log(typeof test2);
　
}

const input = document.querySelector("input");
input.addEventListener("click", testJScodeRun);

</script>

</body>
```

**解説**

上記のコードでは、`textarea要素`内にデフォルトで`"こんにちわ"`とダブルクォーテーション付きの文字を設定している。

このダブルクォーテーション付きの`"こんにちわ"`が変数`JS`に代入され、`eval`の引数として`const test1 = eval(JS);`の部分で実行されると、変数test1には`eval`の戻り値として**ダブルクォーテーションが取れた形の`こんにちわ`が返される。(この時点では戻り値として返されるだけで実行はされない)**

この`こんにちわ`はただの文字なので、その後の行の`const test2 = eval(test1);`の部分で実行しようとするとエラーになるが、一方で`"const test2 = eval(test1);"`のようなスクリプトとして実行可能な文字列を`textarea要素`内に入力すると`const test2 = eval(test1);`の行で実行できてしまう。

### createTextNodeはオブジェクトとなって実行されない

`createTextNode`を使用した場合も上記の「evalの挙動」の節で書いたコードのようにすることで`eval`が実行できるのではないかと考え、以下のコードを書いて確かめてみた。

**結果、実行されないことがわかった。**

`typeof演算子`を使って確かめたところ、そもそもデータ型が`object`となっていて、MDNの`eval`説明にある **「`eval()`の引数が文字列でない場合、`eval()`は引数を変更せずに返します。」** ( 参照:[MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/eval) )の通りの動作をした。

つまり、`createTextNode`を使用すると変数JSのデータ型が文字列型を表す`string`から変化して、プリミティブ以外のデータ型を表す`object`に変わった。**このことにより、`eval`で実行しようとしても引数が変更されずにそのまま返されるため実行できなくなっている。**　ここは流石によくできてるなw

以上のことから、`createTextNode`はXSS対策に有効ということがわかる。

```
<body>

JavaScriptコード:<br>

<textarea id="JsView" rows="20" cols="50"></textarea><br>
<input type="button" value="実行"/>

<script>

const testJScodeRun = function JScodeRun() {
  
  const JS = document.getElementById("JsView").value;
  console.log("JS:",JS);
  console.log("typeof:",typeof JS);

  const changeJS = document.createTextNode(JS);
  console.log("changeJS:",changeJS);
  console.log(typeof changeJS);
　
　
  const test1 = eval(changeJS);
  console.log(test1);
  console.log(typeof test1);
}

const input = document.querySelector("input");
input.addEventListener("click", testJScodeRun);

</script>

</body>
```

### evalで文字を数値に変換

`eval`は古いコードだと以下のように、**文字列を数値に変換する**用途で使われることがある。( 参照:[はてなブログ](https://kuronekojima.hatenadiary.org/entry/20080207/1202450418) )

```
var leftValue = document.getElementById("left").value;
var rightValue = document.getElementById("right").value;
var totalValue = eval(leftValue) + eval(rightValue); /* ここで合計を計算している */
```

なぜこういった使い方ができるのかというと、`eval`はソースコードを評価した結果を返すため、引数にデータ型が文字列の数字を入力するとその数字が解釈され、**最終的に計算に扱える形の数値に変換されて返される。**

このことを実際に確かめるには、以下のコードを入力してtextarea要素内に`4`とか`3`とか数字を入力して実行してみると良い。

```
<body>

JavaScriptコード:<br>

<textarea id="JsView" rows="20" cols="50"></textarea><br>
<input type="button" value="実行"/>

<script>

const testJScodeRun = function JScodeRun() {
  
  const JS = document.getElementById("JsView").value;
  console.log("JS:",JS);
  console.log("typeof:",typeof JS);
  
  /* 
  const changeJS = document.createTextNode(JS);
  console.log("changeJS:",changeJS);
  console.log(typeof changeJS);
　 */
　
　
  const test1 = eval(JS);
  console.log("test1:",test1);
  console.log(typeof test1);
}

const input = document.querySelector("input");
input.addEventListener("click", testJScodeRun);

</script>

</body>
```
実行結果は以下になる。

最初は文字列型だった値が、`eval`を通すことでソースコードとして評価がなされ、計算に使用できる **「数値型」** に変更されたことがわかる。

```
/* 入力 */
4

/* 実行結果 */

JS: 4
typeof: string
test1: 4
number
```

## 前知識
### inputイベント

 `<input>`, `<select>`, `<textarea>`の**各要素の値(`value`)が変更されたときに発生する。** (`value`の意味については後述)

 実際に使用するには以下のようにコードを書く。

```
<body>

<input placeholder="Enter some text" name="name" />
<p id="values"></p>

<script>

const input = document.querySelector("input");
const logOutput = document.getElementById("values");

input.addEventListener("input", updateValue);

function updateValue(e) {
  logOutput.textContent = e.target.value;
} 

</script>

</body>
```

**解説**

`e.target`はイベントが発生した要素を取得するプロパティ。( 参照:[メモ](https://github.com/ren-github-account/Today-I-Learned/blob/15d230cff3f1879750be707af573ddcef37c1a27/JavaScript/MDN-%E3%82%A4%E3%83%99%E3%83%B3%E3%83%88%E5%85%A5%E9%96%80-%E3%81%9D%E3%81%AE2.md#%E3%82%A4%E3%83%99%E3%83%B3%E3%83%88%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88%E3%81%AE%E6%8C%81%E3%81%A4%E3%83%97%E3%83%AD%E3%83%91%E3%83%86%E3%82%A3) )　`.value`を付けることでその要素の中のテキストや数字を取得している。

そして、`textContentプロパティ`を使うことで`e.target.value`にて取得したテキストや数字を、`document.getElementById("values")`の部分で取得した要素内に表示させている。

**注意点**として、上記のコードでは関数を後から宣言して呼び出しているが、この関数を定義する前から呼び出すことは「巻き上げ」と呼ばれバグの原因となるため推奨されないとのこと。( 参照:[qiita](https://qiita.com/redrabbit1104/items/2308a79b4d670f5019b9) )

代用として、「巻き上げ」が起こらない関数を変数に代入した形である**関数式を使うと良い。**

## JS簡易実行環境のコード

HTMLとJavaScript
```
<body>

JavaScriptコード:<br>

<textarea id="JsView" rows="20" cols="50"></textarea><br>
<input type="button" value="実行" onclick="JScodeRun();"/>

<script>

function JScodeRun() {
  const JS = document.getElementById("JsView").value;
  eval(JS);

}

</script>

</body>
```

## 解説

### value

`document.getElementById("JsView")`に`.value`を付けることで、`document.getElementById()`で**取得した要素内に存在する、テキストなどの値を取得できる。**

例えば以下のコードのように`textarea`内に「test」と入力されていた場合、`.value`では「test」というテキストが取得される。

```
<textarea id="JsView" rows="20" cols="50">test</textarea>

/* コンソール画面 */
console.log(JS);

/* 実行結果 */
test
```

### textarea要素

`<textarea>要素`は、**複数行のテキスト入力欄を作成することができる。**

`rows属性`で**行数**を、`cols属性`で**幅**を指定できる。

その他の機能としては、`id属性`を使って`<textarea>要素`を **`<label>要素`に結び付けることができる。**

`<label>要素`と結び付けるには、まず`<textarea>要素`にid属性を設定して、以下のコードに示すように **`<label>要素`の`for属性`の値を`<textarea>要素`のid属性の値と同じにする。**

```
<label for="story">Tell us your story:</label> /* ここでfor="id"とすることでtextarea要素と結び付けている */

<textarea id="story" rows="5" cols="33">
It was a dark and stormy night...
</textarea>
```

結び付け可能な要素として、`<textarea>要素`の他に **`<input>要素`をよく使う。**

結び付けることの**メリット**は、例えばチェックボックスを作成した場合、チェックボックス自体ではなく**ラベル部分をクリックしてもチェックされるようになる。** ( 参照:[so-zou](https://so-zou.jp/web-app/tech/html/element/form/label/) )

### eval

`eval()関数`は、**文字列として与えられたJavaScriptコードを解釈して、その実行結果を返す。**

### evalに関連する用語

`eval()関数`の仕様を読み解くにあたってわからない単語が出てきたので意味をまとめておく。

JavaScriptには、文字列、数値、真偽値(trueまたはfalseのこと)などを扱う時の **「値の型」** が存在する。

こういった値の型のことを**データ型**と呼ぶ。

データ型には大きく分けて、**プリミティブ型とオブジェクトの2つがある。**

それぞれの違いを以下の表にまとめた。

||意味|
|-|-|
|プリミティブ型|一度作成したらその値自体を変更できないという特性を持つ。プリミティブ型の**具体例**として、真偽値(trueやfalse), 数値(123など), 文字列("文字列"), undefined(値が未定義であることを意味するデータ型), null(値が存在しないことを意味するデータ型)などがある。( 参照:[データ型](https://jsprimer.net/basic/data-type/) )|
|オブジェクト|プリミティブ型以外のデータのこと。|

データ型は、**`typeof演算子`を使用すると調べることができる。**

使うには以下のように書く。

```
console.log(typeof true)

/* 実行結果 */
boolean

console.log(typeof null)

/* 実行結果 */
object
```

ちなみに、上記の実行結果を見て**こう疑問に思ったかもしれない。**

`null`はプリミティブ型のはずなのに、実行結果がオブジェクトになっているのはなぜ？と。

**これは歴史的経緯のある有名なバグで仕様である。** ( 参照:[データ型](https://jsprimer.net/basic/data-type/) )

### 関連する用語その2

その他の調べたことは以下の表にまとめた。

||意味|
|-|-|
|`String(引数)`|引数を文字列型に変換する関数。|
|`Symbol`|`Symbol`はJavaScriptの仕様のES2015にて新しく追加されたもの。言語の互換性を維持した状態でオブジェクトに新たな機能やプロパティを追加するために考案された。( 参照:[とほほの](https://www.tohoho-web.com/js/symbol.htm#useful) ) `Symbol`は、`Symbol()コンストラクター`を持ち、これを使って**シンボルプリミティブ**(**シンボル値**または単に**シンボルとも呼ばれる**)を量産することができる。この作成される**シンボル**(シンボルプリミティブ)は、**重複のない一意なもの**であり、`Symbol()`を呼び出すたびに戻り値として返される。`Symbol`はよく、**一意のプロパティキー(プロパティ名)をオブジェクトに追加するために使用される。** ( 参照:[MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Symbol/Symbol) )`[Symbol.iterator]()`は、**ウェルノウンシンボル**と呼ばれる`Symbolコンストラクター`のプロパティのひとつで、イテレータ(iterator)を返す引数なしの関数のこと。( 参照:[MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Symbol/iterator#%E8%A7%A3%E8%AA%AC) )|
|イテレータ(iterator)|ざっくり言うと**一連の複数のデータからなるものを指す。** ( 参照:[for-of文メモ](https://github.com/ren-github-account/Today-I-Learned/blob/fd9148c0f27cd2951c83026e4d524d3329093e33/JavaScript/MDN-%E3%82%A4%E3%83%99%E3%83%B3%E3%83%88%E5%85%A5%E9%96%80-%E3%81%9D%E3%81%AE2.md#for-of%E6%96%87) )|
|iterable|オブジェクトを反復処理(iterated)する必要がある時に**渡す配列や文字列**のこと。反復処理とは、具体的にはfor-of文のループ開始時などを指す。|
|iterable protocol|MDNの日本語訳では「反復可能プロトコル」と訳されている。このプロトコルによってJavaScriptのオブジェクトは、反復処理(iteration)の振る舞いを定義またはカスタマイズすることができる。具体的にはfor-of文の中でどの値がループに使われるかなど。( 参照:[MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Iteration_protocols#%E5%8F%8D%E5%BE%A9%E5%8F%AF%E8%83%BD%E3%83%97%E3%83%AD%E3%83%88%E3%82%B3%E3%83%AB) )|
|反復可能オブジェクト|まず前提として、配列を表す`Arrayオブジェクト`や、`test.set(キー名, 値);`のように記述することで**キーと値のペアを保存**することのできる`Mapオブジェクト`などは**デフォルトで反復動作を持っているため、「組み込み反復可能オブジェクト」と呼ばれる。** しかし、それ以外の**通常のオブジェクトはそうではなく、デフォルトでは反復動作を持たない。** ( 参照:[MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Iteration_protocols#%E5%8F%8D%E5%BE%A9%E5%8F%AF%E8%83%BD%E3%83%97%E3%83%AD%E3%83%88%E3%82%B3%E3%83%AB) ) **反復動作を持たないオブジェクトを反復可能にするには、後述のジェネレータ関数を使用すればok。ジェネレータ関数を使用することで、通常のオブジェクトでも「配列式や文字列などの反復可能オブジェクト( 参照:[MDN-スプレッド構文](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Operators/Spread_syntax) )」と同じような動作をさせることができる。** (ということはジェネレータ関数を呼び出してその戻り値を変数に代入した後、for-of文のofの部分に`of 変数名`とい形でその変数を使えるってことかな？)|

### String関数の使い方

以下のように書く。

```
<script>

const result = 2 + 2;　// resultに計算結果の4が格納される。この時点のデータ型は'number'

const b = String(result); // Stringを使用してresultを文字列型に変換している

</script>

```

本当に文字列型に変換されているかは以下のコードで確認できる。

```
/* コンソール画面 */
typeof b;

/* 実行結果 */
'string'
```

### Symbolの使い方

上記の[関連する用語その2](#関連する用語その2)の節にて`Symbol`の意味について見てきた。

ここからはその具体的な使い方を見ていく。

実際に使うには以下のように書く。

```
const obj = {};
obj["foo"]="hoge";

const s = Symbol();
obj[s]="piyo";
```

**解説**

まず1行目で`{}`と記述することで**空のオブジェクトを作成している。**(上記のコードの場合、オブジェクト名は`obj`)

2行目は、**ブラケット記法**を使用しており、`オブジェクト名["プロパティ名"]`の形で書かれている。この行では、1行目で作成した空の`objオブジェクト`に対してプロパティ名`foo`のプロパティとその値`hoge`を追加している。

3行目は`Symbol()`によって一意のシンボルが戻り値として返され、変数sに代入されている。

4行目では、再びブラケット記法を使用して、3行目で戻り値として返された**一意のシンボルをobjオブジェクトのプロパティ名として設定し、値を代入している。**

ここまで理解できれば後は[appendChild()メソッド](https://github.com/ren-github-account/Today-I-Learned/blob/main/JavaScript/%E7%94%BB%E9%9D%A2%E3%81%AB%E3%82%AB%E3%83%A9%E3%83%95%E3%83%AB%E3%81%AA%E8%89%B2%E3%82%92%E3%83%A9%E3%83%B3%E3%83%80%E3%83%A0%E7%94%9F%E6%88%90.md#appendchild%E3%83%A1%E3%82%BD%E3%83%83%E3%83%89)のところで学習した`appendChild()メソッド`と`createTextNode()メソッド`を組み合わせるやり方を使って、**上記のコード4行目にて、objオブジェクトの`Symbolプロパティ`に代入した値を、ブラウザの画面に表示させることができる。**

ブラウザの画面に表示させるには以下のコードを書く。

```
<body>
<p id="p1"></p>

<script>

const obj = {};
obj["foo"] = "hogehoge";

const s = Symbol();
obj[s] = "piyopiyo!";

const newText = document.createTextNode(obj[s]);　/* ここで引数にobj[s]を設定している */
const p1 = document.getElementById("p1");
p1.appendChild(newText);

</script>
</body>
```

```
/* 実行結果 */
/* ブラウザ画面 */
piyopiyo!
```

### Symboliteratorの使い方

以下のコードを書く。

```
const iterable1 = {}; /* ここでオブジェクトを作成 */

iterable1[Symbol.iterator] = function* () {　/* ここではブラケット記法が使われている */
 yield 1;
 yield 2;
 yield 3;
};

console.log(...iterable1);

```

**注意点**として、上記のコードでは**ブラケット記法が使われている。** つまり、iterable1オブジェクトに`Symbol.iteratorプロパティ`を新しく追加している。

### 用語解説Symboliterator

ここからは、上記の`[Symbol.iterator]`を使ったコードを理解するために必要な知識をまとめていく。

||意味|
|-|-|
|イテレータオブジェクト|イテレータは以前学習したように「一連の複数のデータからなるもの」だが、このイテレータ自体も**イテレータオブジェクト**というオブジェクトである。そしてこのイテレータオブジェクトは`next()メソッド`を持つ。|
|`next()メソッド`|valueプロパティとdoneプロパティの2つのプロパティを持ち、`{}`で囲まれたオブジェクトを戻り値として返すメソッド。( 参照:[MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Generator/next) )|
|ジェネレータ関数|functionの後に **`*`(アスタリスク)を付けて宣言された関数は、ジェネレータ関数となる。** この関数の変わった点は、**`変数名();`と書いて関数を呼び出しただけでは関数は実行されないことにある。** 呼び出されると関数が実行されない代わりに、関数の実行を制御するためのオブジェクトである**Generatorオブジェクトが生成されて戻り値として返される。** このGeneratorオブジェクトは`nextメソッド`を持ち、**この`nextメソッド`を呼び出すことで初めて関数が実行される。**|
|`...`|この点三つのことを**スプレッド構文**と呼ぶ。先頭に付けることで、**配列や文字列などの反復可能オブジェクトを展開することができる。** 参照:[MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Operators/Spread_syntax)|
|`[]`|配列リテラル。文字列リテラルの`""`と同じように使うことができ、`[]`で囲むことで簡単に配列を作成できる。参照:[jsprimer](https://jsprimer.net/basic/data-type/)|
|`*プロパティ名`|**ジェネレータ関数にも省力記法**が存在し、プロパティ名の先頭に`*`を付けることで使用できる。参照:[uhyohyo](https://uhyohyo.net/javascript/16_8.html), コードの具体例:[その1](https://developer.mozilla.org/ja/docs/Web/JavaScript/Guide/Iterators_and_generators#%E5%8F%8D%E5%BE%A9%E5%8F%AF%E8%83%BD%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88), [その2](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Statements/function*#%E4%BE%8B)|

上記のコードに以下のコードを追記して、`console.log();`でその中身を確認してみた。
```
const iteratorObj = iterable1[Symbol.iterator]();
```

【コンソール画面】
```
/* 入力文その1 */
console.log(iteratorObj);

/* 実行結果 */
iterable1.<computed> {<suspended>}
```

今度は先頭に`...`を付けてみる。すると…

```
/* 入力文その2 */
console.log(...iteratorObj);

/* 実行結果 */
1 2 3
```

### 本当にイテレータオブジェクトなのか検証

あれこれ検証した結果、最終的に以下の結論になった。

||内容|
|-|-|
|結論|結局、`オブジェクト名[Symbol.iterator]`を書くかどうかではなく、**ジェネレータ関数を使っているというのが重要なポイントだった。** ジェネレータ関数が呼び出されるとGeneratorオブジェクトが戻り値として返されるが、この**Generatorオブジェクトはイテレータとしても使用できる。** なぜイテレータとして使えるかというと、Generatorオブジェクトはnextメソッドを持ち、その戻り値はvalueとdoneプロパティを持つオブジェクトで、これはイテレータの特徴を満たしているから。( 参照:[uhyohyo](https://uhyohyo.net/javascript/16_6.html) )|

そして、`console.log`の引数を`...iterable1`, `...iteratorObj`, `...geneObj`のどれを書いても実行結果が`1 2 3`となって同じになるのは、**おそらく`...`が高機能でオブジェクト名の`iterable1`を書いたとしてもオブジェクト内にある`Symbol.iteratorプロパティ`を持ったジェネレータ関数を自動的に呼び出してくれて、そこからイテレータの値を取り出してくれるからだと考えられる。**

というより、より正確に言うならば、MDNの解説ページにある次の文章 **「反復可能オブジェクトにするには、オブジェクトは`[Symbol.iterator]()メソッド`を実装する必要があります。」** ( 参照:[MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Guide/Iterators_and_generators#%E5%8F%8D%E5%BE%A9%E5%8F%AF%E8%83%BD%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88) )とあるように、ブラケット記法で`オブジェクト名[Symbol.iterator]`と書くことで、**そのオブジェクト自体が「反復可能オブジェクト」となる。**

反復可能オブジェクトとなることで**for-of文において配列や文字列(これらは共にデフォルトで反復可能オブジェクト)と同じように反復機能を持たない通常のオブジェクトを扱うことが可能となる。**

具体的には以下のように書くことで、オブジェクト名を配列や文字列と同じように扱って実行することが可能となる。

```
for(const 変数名 of オブジェクト名){
 処理内容
  
}
```

実際にコードを書いてみると以下になる。

```
<body>
<p id="outputID"></p>

<script>

const iterable1 = {};

iterable1[Symbol.iterator] = function* () {
  yield 10;
  yield 11;
  yield 12;
};

const output = document.getElementById("outputID")

for(const value of iterable1){
 output.textContent += value;
  
}

</script>

</body>
```
```
/* 実行結果 */
101112
```

**`yield 値;`の値の部分を変更することで**、for-of文に渡す**iterable**を変更することができる。( 参照:[MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Iteration_protocols#%E5%8F%8D%E5%BE%A9%E5%8F%AF%E8%83%BD%E3%83%97%E3%83%AD%E3%83%88%E3%82%B3%E3%83%AB) ) 例えば、以下のようにすると実行結果が変更できる。

```
iterable1[Symbol.iterator] = function* () {
  yield 4;
  yield 5;
  yield 6;
};

/* 実行結果 */
456
```
また、以下のように**テキストを渡すこともできる。**
```
iterable1[Symbol.iterator] = function* () {
  yield "りんご, ";
  yield "みかん, ";
  yield "バナナ";
};

/* 実行結果 */
りんご, みかん, バナナ
```

【▼検証開始】

上記の入力文その1で表示された`iterable1.<computed> {<suspended>}`が本当にイテレータオブジェクトなのかを確かめる。

まず、検証にあたって最初のコードに以下のように追記を行った。

```
<script>

const iterable1 = {};

iterable1[Symbol.iterator] = function* () {
  yield 1;
  yield 2;
  yield 3;
};

iterable1["test"] = function(){ alert("hello!");}; //追加その1

iterable1["foo"] = function* (){　//追加その2 
  yield 1;
  yield 2;
  yield 3;
};

const iteratorObj = iterable1[Symbol.iterator]();
 
const geneObj = iterable1["foo"]();

</script>
```

**注意点**として、上記のコードの`iterable1["foo"]`の部分では条件を揃えるため関数名は付けていない。

以下では、通常のジェネレータ関数と、`[Symbol.iterator]`を使って作られたジェネレータ関数を比較している。

【通常のジェネレータ関数】

まずは通常のジェネレータ関数の場合はどうなるかを見てみる。

前準備として以下のコードをJavaScriptに追記。
```
const geneObj = iterable1["foo"]();
```

以下のコードの説明をしておくと、まず`[Symbol.iterator]`を使わない通常のジェネレータ関数を作成したのち、その関数を呼び出し`geneObj変数`に代入してからコンソール画面にて`console.log();`を使用して中身を表示させている。

```
/* 入力文 */
console.log(geneObj);

/* 実行結果 */
iterable1.foo {<suspended>}
[[GeneratorLocation]]: mdn-learning.html:25
[[Prototype]]: Generator
[[GeneratorState]]: "suspended"
[[GeneratorFunction]]: ƒ* gen()
[[GeneratorReceiver]]: Object
[[Scopes]]: Scopes[3]
```

【`[Symbol.iterator]`を使ったジェネレータ関数】

同様にして、次は`[Symbol.iterator]`を使って作られたジェネレータ関数の場合はどうなるかを見てみる。

まず、前準備としてJavaScript内に以下のコードを追記。
```
const iteratorObj = iterable1[Symbol.iterator]();
```

```
/* 入力文 */
console.log(iteratorObj);

/* 実行結果 */
iterable1.<computed> {<suspended>}
[[GeneratorLocation]]: mdn-learning.html:17
[[Prototype]]: Generator
[[GeneratorState]]: "suspended"
[[GeneratorFunction]]: ƒ* ()
[[GeneratorReceiver]]: Object
[[Scopes]]: Scopes[3]
```



