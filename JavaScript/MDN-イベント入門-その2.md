*目次*
* [概要](#概要)
* [イベントオブジェクト](#イベントオブジェクト)
* [イベントオブジェクトの持つプロパティ](#イベントオブジェクトの持つプロパティ)
* [targetプロパティの活用法](#targetプロパティの活用法)

### 概要

前回[MDN-イベント入門(&DOMの基礎知識)](https://github.com/ren-github-account/Today-I-Learned/blob/main/JavaScript/MDN-%E3%82%A4%E3%83%99%E3%83%B3%E3%83%88%E5%85%A5%E9%96%80(&DOM%E3%81%AE%E5%9F%BA%E7%A4%8E%E7%9F%A5%E8%AD%98).md)の続き。

### イベントオブジェクト

イベントハンドラーとなる関数内で、よく **`event`、`evt`、`e` などと名付けられた引数を見かけることがある。**

これらの引数は、**イベントオブジェクト**と呼ばれ、**役割としてはイベントの追加機能や情報を提供するもので、それらはイベントハンドラーに自動的に渡される。**

ちなみに、このイベントオブジェクトには**好きな名前を付けることができる。** `event`や`ev`などとされることが多いが、これは**慣習的にこういう名前が使われている**だけで、別に自分で好きな名前を使っても動作する。

### イベントオブジェクトの持つプロパティ

イベントオブジェクトが持つプロパティの中に、**target**と**currentTarget**という2つのプロパティがある。

この2つはよく使われるので、使い方を見ていく。

まずこの2つのプロパティがどういったプロパティなのかを以下の表にまとめた。

||意味|
|-|-|
|currentTargetプロパティ|イベントハンドラーが**登録されている**HTML要素を表す|
|targetプロパティ|実際にイベントが**発生した**HTML要素を表す|

**使い方**

実際に使うには、以下のように使う。

【HTMLとJavaScript】
```
<body>

   <div>
      <p>test</p>
      <p>test</p>
      <p>test</p>
    </div> 

<script>

 var listener = function(ev){
          console.log("target:" ,ev.target, "relatedTarget:" ,ev.currentTarget);
      }

      document.body.addEventListener("click",listener);

 </script>
  </body>

```

【CSS】
```
div{
    background-color:ebe3d5;
   }
      
p{
   background-color:#f3eeea;
 }
```

**解説**

` document.body.addEventListener("click",listener);`の部分では、documentオブジェクトのbodyインスタンスプロパティ( 参照:[Document](https://developer.mozilla.org/ja/docs/Web/API/Document) )に対して、addEventListenerを適用している。

`console.log`の引数に**複数指定する**場合は上記のコードのように **カンマ(,)** で区切って入力する。( 参照:[コンソールログへ出力](https://www.javadrive.jp/javascript/console/index5.html) )

上記のコードを実行してみると、p要素をクリックした時には、targetプロパティの値が`<p>test</p>`となり、div要素をクリックすると**targetプロパティは**`<div>…</div>`となり**変化するが、**  
一方で**currentTargetプロパティの値**はずっと`<body>…</body>`のままで**変化しない。** 変化しない理由は、currentTargetプロパティの中には**イベントハンドラーが登録された要素**が入っているから。

### targetプロパティの活用法

イベントオブジェクトの持つプロパティのうち、**targetプロパティは、if文の判定に使用することができる**のでとても便利。

具体的には以下のように使用する。

```

  <body>

   <div>
      <p>test</p>
      <p>test</p>
      <p>test</p>
    </div> 

<script>

 const listener = function(ev){
       if(ev.target.tagName == "P"){
          console.log("p");
       } else if(ev.target.tagName == "DIV"){
          console.log("div");
       }
          
      }

      document.body.addEventListener("click",listener);

 </script>
  </body>
```

**解説**

上記のコードで新しく出てきた`tagNameプロパティ`は、**HTML要素のタグ名を返すプロパティ。**(具体的には、div要素なら`DIV`、p要素なら`P`と返す)

**注意点**として、**HTML文書でtagNameプロパティを使用する場合、タグ名は常に大文字で返される点に注意。** なので、上記のコードのif文の`ev.target.tagName == "DIV"`の部分を`"div"`と小文字にするとエラーは出ないがうまく動かない。

**英語でelse**は「その代わり」といった意味。

**【応用】**

上記のコードでは、tagNameプロパティを使用したけど、それ以外にも**クラス名を取得するclassNameプロパティやidを取得するidプロパティ**も同じように使用することができる。というか、こちらの方が実用性が高いと思う。

**注意点**として、classNameプロパティは**class「Name」** と**Name**の先頭は大文字にする点に注意。そうしないとうまく動かない。

上記のコードのJavaScriptの部分を改良してclassName使って以下のコードを書いてみた。

```
<script>

 const listener = function(ev){
       if(ev.target.className == "tanaka"){
          console.log("こんにちわtanakaです");
       } else {
          console.log("ﾌﾞﾌﾞﾌﾞｯぅー! tanakaじゃないよ");
       }
      
      }

      document.body.addEventListener("click",listener);

 </script>

```

**解説**

上記のコードを実行すると、body内の要素をクリックした時、要素のクラス名が「tanaka」なら「こんにちわtanakaです」と表示され、そうではない場合は、「ﾌﾞﾌﾞﾌﾞｯぅー! tanakaじゃないよ」と表示される。

idプロパティを使っても同じように動作する。



