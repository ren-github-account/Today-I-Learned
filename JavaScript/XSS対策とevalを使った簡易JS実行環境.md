## やりたいこと

`eval()`プロパティを使用してブラウザ上で動くJSの簡易実行環境を作成したのち、`createTextNode()メソッド`を使用したXSS対策を自分で確かめてみる。

【参考】

・JSの簡易実行環境の作り方

https://blog.goo.ne.jp/jeans201/e/3eb2b3a9303916b49175309f7d890f55

・`createTextNode()メソッド`を使ったXSS対策について

https://upa-pc.blogspot.com/2015/02/javascript-dom-based-xss-protect-createTextNode.html

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

**用語**

`eval()関数`の仕様を読み解くにあたってわからない単語が出てきたので意味をまとめておく。

JavaScriptには、文字列、数値、真偽値(trueまたはfalseのこと)を扱う時の **「値の型」** が存在する。

こういった値の型のことを**データ型**と呼ぶ。

データ型は大きく分けて、**プリミティブ型とオブジェクトの2つに分類される。**

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

**これは歴史的経緯のある有名なバグで仕様とのこと。** ( 参照:[データ型](https://jsprimer.net/basic/data-type/) )

その他の調べたことは以下の表にまとめた。

||意味|
|-|-|
|`String(引数)`|引数を文字列型に変換する。|
|||

**`String()`の使い方。**

以下のように書く。

```
<script>

const result = 2 + 2;　// resultに変数結果の4が格納される。この時点のデータ型は'number'

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










