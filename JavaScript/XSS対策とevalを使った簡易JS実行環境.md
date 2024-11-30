## やりたいこと

`eval()`プロパティを使用してブラウザ上で動くJSの簡易実行環境を作成したのち、`createTextNode()メソッド`を使用したXSS対策を自分で確かめてみる。

【参考】

・JSの簡易実行環境の作り方

https://blog.goo.ne.jp/jeans201/e/3eb2b3a9303916b49175309f7d890f55

・`createTextNode()メソッド`を使ったXSS対策について

https://upa-pc.blogspot.com/2015/02/javascript-dom-based-xss-protect-createTextNode.html

### JS簡易実行環境のコード

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

**解説**

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

