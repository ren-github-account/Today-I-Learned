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

**これは歴史的経緯のある有名なバグで仕様とのこと。** ( 参照:[データ型](https://jsprimer.net/basic/data-type/) )

その他の調べたことは以下の表にまとめた。

||意味|
|-|-|
|`String(引数)`|引数を文字列型に変換する。|
|`Symbol`|`Symbol`はJavaScriptの仕様のES2015にて新しく追加されたもの。言語の互換性を維持した状態でオブジェクトに新たな機能やプロパティを追加するために考案された。( 参照:[とほほの](https://www.tohoho-web.com/js/symbol.htm#useful) ) `Symbol`は、`Symbol()コンストラクター`を持ち、これを使って**シンボルプリミティブ**(**シンボル値**または単に**シンボルとも呼ばれる**)を量産することができる。( 参照:[MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Symbol/Symbol) )`[Symbol.iterator]()`は、**ウェルノウンシンボル**と呼ばれる`Symbolコンストラクター`のプロパティのひとつで、イテレータ(iterator)を返す引数なしの関数のこと。( 参照:[MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Symbol/iterator#%E8%A7%A3%E8%AA%AC) )|
|イテレータ(iterator)|ざっくり言うと**一連の複数のデータからなるものを指す。** ( 参照:[for-of文メモ](https://github.com/ren-github-account/Today-I-Learned/blob/fd9148c0f27cd2951c83026e4d524d3329093e33/JavaScript/MDN-%E3%82%A4%E3%83%99%E3%83%B3%E3%83%88%E5%85%A5%E9%96%80-%E3%81%9D%E3%81%AE2.md#for-of%E6%96%87) )|
|iterable|オブジェクトを反復処理(iterated)する必要がある時に**渡す配列や文字列**のこと。反復処理とは、具体的にはfor-of文のループ開始時などを指す。|
|iterable protocol|MDNの日本語訳では「反復可能プロトコル」と訳されている。このプロトコルによってJavaScriptのオブジェクトは、反復処理(iteration)がどう振る舞うかを定義またはカスタマイズすることができる。具体的にはfor-of文の中でどの値がループに使われるかなど。( 参照:[MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Iteration_protocols#%E5%8F%8D%E5%BE%A9%E5%8F%AF%E8%83%BD%E3%83%97%E3%83%AD%E3%83%88%E3%82%B3%E3%83%AB) )|
|反復可能オブジェクト|**iterable**のこと。|

**`String()`の使い方。**

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










