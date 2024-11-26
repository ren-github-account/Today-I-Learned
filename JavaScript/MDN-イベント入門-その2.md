*目次*
* [概要](#概要)
* [イベントオブジェクト](#イベントオブジェクト)
* [イベントオブジェクトの持つプロパティ](#イベントオブジェクトの持つプロパティ)
* [targetプロパティの活用法](#targetプロパティの活用法)
* [keydownイベント](#keydownイベント)
* [デフォルトのアクションを停止する](#デフォルトのアクションを停止する)
* [イベントのバブリング](#イベントのバブリング)
* [stopPropagationメソッド](#stopPropagationメソッド)
* [for-of文](#for-of文)
* [onclick属性は使わない方が良い](#onclick属性は使わない方が良い)

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

上記のコードのJavaScriptの部分を改良してclassNameプロパティを使って以下のコードを書いてみた。

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

### keydownイベント

`keydown`イベントは、**ユーザーがキーボードのキーを押したときに発行される。**

そして、keydownイベントは**`KeyboardEvent`オブジェクト**に属する`keyプロパティ`と組み合わせることで、どのキーが押されたかを識別できる。

`keyプロパティ`は、**どのキーが押されたかを表す。**

そして、KeyboardEventオブジェクトは**イベントオブジェクトの一つ**であるため、そのKeyboardEventオブジェクトに属する`keyプロパティ`は、`event.key`のように記述することができる。

**具体例**

HTMLとJavaScript
```
<body>

   <input id="textBox" type="text">
   <div id="output"></div>
<script>

const textBox = document.querySelector("#textBox");
const output = document.querySelector("#output");

function keyboard(event){
 output.textContent = `"${event.key}" が押されました。`;
 
}

textBox.addEventListener("keydown",keyboard);

 </script>
  </body>

```

**解説**

`querySelectorメソッド`の引数がidの場合、先頭に`#`を付けて使用する。

`textContentプロパティ`を使用することで、div要素に対して**テキストを設定している。**  
ブラウザのデベロッパーツールの画面のうち「Elements」タブの画面を見ながら、上記のコードを実行するとわかるが、**div内のテキストの内容はキーを押すごとに動的に入れ替わっていることが確認できる。**

`${event.key}`の`${}`の部分のことを、**テンプレートリテラル**と呼ぶ。文字列と変数などを結合して表示したい時に使うことで`+`などの連結演算子を使用することなく文字列との結合が可能となる。

`+`などの連結演算子を使用して、文字列と変数や式をつなげようとすると、**見た目が煩雑で読みづらくなりがち**だが、この**テンプレートリテラルを使うことでシンプルに記述できるため可読性を向上させることができる。** 参照:[MDN-テンプレートリテラル](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Template_literals#%E6%96%87%E5%AD%97%E5%88%97%E3%81%AE%E8%A3%9C%E9%96%93)、[フーのブログ](https://fuuno.net/nani/nani02/nani02.html)

### デフォルトのアクションを停止する

`preventDefault()メソッド`を使用することで、ブラウザにデフォルトで備わっているアクション **(デフォルトアクション)を動作させないようにすることができる。**

**デフォルトアクションとは**、具体的には、リンクをクリックしたら「リンク先」へ飛ぶことや、フォームの送信ボタンを押すとフォームが送信されることなどを指し、特別なことをしなくともデフォルトで備わっている動作のこと。参照:[イベントオブジェクトのメソッド](https://uhyohyo.net/javascript/3_5.html)

preventは英語で **「何かが生じたりしないようにする」** といった意味がある。

**具体例**

HTMLとJavaScript
```
<body>

<form action="mdn-learning.html" method = "get">
  <div>
    <label for="fname">姓: </label>
    <input id="fname" name = "sei" type="text" />
  </div>
  <div>
    <label for="lname">名: </label>
    <input id="lname" name = "mei" type="text" />
  </div>
  <div>
    <input id="submit" type="submit" />
  </div>
</form>
<p></p>

<script>
const form = document.querySelector("form");
const fname = document.getElementById("fname");
const lname = document.getElementById("lname");
const para = document.querySelector("p");

const formCheck = function(e) {
  if(fname.value === "" || lname.value === ""){
    e.preventDefault();
    para.textContent = "名前が空っぽですよ!";
 }
}

form.addEventListener("submit",formCheck);

 </script>
  </body>
```

**解説**

まず、body内の最初の部分で、`<form action="mdn-learning.html" method = "get">`と記述してある。

これは `<form>要素`に対して、ユーザーがデータを送信した時の処理方法を設定するための2つの属性、`action属性`と`method属性`を付与している。

この2つの属性の違いは以下の表にまとめた。( 参照:[フォームデータの送信](https://developer.mozilla.org/ja/docs/Learn/Forms/Sending_and_retrieving_form_data)、[フォームデータの確認](https://www.webword.jp/xhtml/form/index2.html) )

||意味|
|-|-|
|action属性|どこにデータを送信するかを定義する。上記のコードでは<form>要素を記述しているhtmlファイルである`mdn-learning.html`を指定している。|
|method属性|どのようにデータを送信するかというその方法を定義する。`GETメソッド`と`POSTメソッド`がよく使われる。|

上記のコードでは、`method属性`のうち`GETメソッド`を設定している。

**`GETメソッド`では、フォームで送信されたデータがaction属性で指定したURL(もしくはhtmlファイル)の末尾に追加される。**

フォームデータの送信前と送信後では、**URLの末尾は以下のように変化する。**

```
/* 送信前  */
mdn-learning.html

/* 送信後 */
mdn-learning.html?sei=tanaka&mei=tarou
```

**変化の内容をまとめると**、まずURLの先頭に`?`が追加され、その後に **`<input>要素`のname属性で指定した値=フォームで送信した値** という形が続き、その間が`&`で区切られている。

**POSTメソッドとの違い**

一方で、`POSTメソッド`を設定した場合、送信したデータは**URLの末尾に表示されない。**　では、どこで見れるかというと **デベロッパーツールの「Networkタブ」** からmethod属性を付与したhtmlファイルを選択すると見ることができる。

ここで重要な点は、**`GETメソッド`ではフォームで送信したデータをURLを通して、ユーザーが見ることができるが、`POSTメソッド`の場合は見ることができないということ。**

使い分けとして、**大量のデータを送信する必要がある場合は`POSTメソッド`が好ましい**とされる。理由としては、URLの長さ制限があるブラウザーが存在するため(GETメソッドでは、送信したデータがURLの末尾に追加されていくため、どんどん長くなってしまう)。参照:[HTTP リクエストの表示](https://developer.mozilla.org/ja/docs/Learn/Forms/Sending_and_retrieving_form_data)

あとは、よく言われるのがパスワードなどの漏れてはいけないデータを送信する時は`GETメソッド`を使用してはいけない。

**上記のコードの解説 続き**

上記のコードの解説に話を戻すと、

上記コードでは、フォームの入力欄の片方もしくは両方が「空(何も入力されていない状態)」だった場合に、**`preventDefault()メソッド`を使用して送信処理を停止する**という内容のJavaScriptが記述されている。

なので、もし上記のJavaScriptコードを丸ごと削除して実行した場合、フォームに苗字しか入力せずに**名前の欄を空欄にして送信した場合も処理が実行されてしまうということが起こる。**

**具体例**は以下に示す。

名前欄が空白でも以下に示すようにURLの末尾の部分が`&mei=`となって処理がされていることがわかる。

```
/* 名前欄しか入力しなかった場合 */
mdn-learning.html?sei=tanaka&mei=
```
こうなってしまっては、サービスの運営側としては困ったことになるという訳である。こういった問題を未然に防ぐために`preventDefault()メソッド`は使用できる。

### イベントのバブリング

イベントのバブリングでは、**HTML文書が入れ子構造(親要素と子要素が存在する構造)になっていた場合に、イベントをどのように処理するのか**を定めている。

**具体例**として以下のコードを見ていく。

```
 <body>
<div id="container">
  <button>クリックしてください</button>
</div>
<pre id="output"></pre>

<script>

const output = document.querySelector("#output");
function handleClick(e) {
  output.textContent += `${e.target.tagName} 要素をクリックしました\n`;
}

const container = document.querySelector("#container");

container.addEventListener("click", handleClick);

 </script>
  </body>
```

上記のコードで**重要な点**は以下の2点。
||重要な点|
|-|-|
|①|`<div>要素`の中に`<button>要素`があるという点。|
|②|`<button>要素`の親要素である`<div>要素`に、addEventListener`を使ってイベント付与したという点。|

上記のコードを実行してみると、どうなるかというと、以下のことが起こる。

・`<div>要素`の**子要素である`<button>要素`をクリックしたのに、親要素`<div>要素`に登録したイベントが実行された。**

つまり、これは次のことを意味している。**要素同士が親子関係にある場合、親要素にイベント処理を登録しても、子要素をクリックすると、親要素に登録したイベント処理が実行される。**

MDNの説明にも以下のように書かれている。

```
これは理にかなっています。ボタンは <div> 内にあるため、ボタンをクリックすると、
そのボタンが属する要素も暗黙的にクリックされます。
```

つまり、これは**子要素から親要素へとイベントのバブリングが発生した**ということになる。

**※豆知識**

これは個人的な解釈だが、おそらくは  
こういった一番内側からどんどん上に向かって実行されていく挙動が、バブル(泡)が上にブクブクと浮き上がっていく様に似ているから、バブリングと名付けられたのだと思う。

たしか、これと似た話がアルゴリズムの一種であるバブルソートの由来のところで出てきた。


**解説**

`<pre>要素`は、文字を入力した時のスペースや改行をそのまま表示させることができる。HTMLでは通常、改行を入れたい時はその場所に`<br>`を入れないと改行できないが、`<pre>要素`を使うと`<br>`を使わずとも改行を反映させることができる。

こういった特性から`<pre>要素`はアスキーアートを表現する時にも使われる。参照:[pre要素](https://htmlcss.jp/html/pre.html)

言葉だけでは分かりずらいので具体例を見てみよう。

まず比較のため`<p>要素`内で改行をしてみると、以下のように反映されない。

```
<p>うんちはくさいが
そうでもないかも</p>

/* 実行結果 */

うんちはくさいが そうでもないかも
```

「うんちはくさいが」の後の改行の部分が「空白」に変換されて表示されているが、これは **「改行、Tab、スペース」は空白に変換されるというCSSの仕様によるもの。** ( 参照:[CSSの空白処理](https://zenn.dev/jiin/articles/370909a1c56f5a) )

一方、**以下のように`<pre>要素`を使用すると、改行が反映された!** 文字と文字との間に空白を空けても同じように反映されている。

```
<pre>うんちはくさいが
そうでもないかも</pre>

<pre>うんちはくさいが          そうでもないかも</pre>

/* 実行結果 */

うんちはくさいが
そうでもないかも

うんちはくさいが          そうでもないかも
```

イベント関数の中のコードをよく見ると`output.textContent += `${e.target.tagName} 要素をクリックしました\n`;`の部分の演算子が`+=`となっている。

こうすることで、以下に示すように**ボタンを押すごとに`<pre>要素`内で文字列が増えていく**という動作が可能となる。

```
BUTTON 要素をクリックしました
BUTTON 要素をクリックしました
BUTTON 要素をクリックしました
BUTTON 要素をクリックしました
BUTTON 要素をクリックしました
BUTTON 要素をクリックしました
BUTTON 要素をクリックしました
BUTTON 要素をクリックしました
BUTTON 要素をクリックしました
BUTTON 要素をクリックしました
BUTTON 要素をクリックしました
```

一方で単なる`=`だけだと、以下のように1回しか実行されない。

```
BUTTON 要素をクリックしました
```

### stopPropagationメソッド

イベントオブジェクトの中には、先ほど学習した`preventDefault()メソッド`の他に、`stopPropagation()メソッド`がある。

これは、イベントが子要素からその親要素へそのまた親要素へ…etcと内側から外側へと伝わっていくという**イベントバブリングの流れ(イベントフロー)を途中で中断する**という機能を持つ。

英語でPropagationには **「特定の方向への、光や音の伝達」** といった意味がある。(Google翻訳の辞書より)

つまり、メソッドの説明に当てはめると、**`stopPropagation()メソッド`はイベントの伝達を途中でstopする**機能を持つということになる。

**具体例**

まず比較のために`stopPropagation()メソッド`を**使わない場合を見てみる。**

HTMLとJavaScript
```
  <body>

<div id="container">
  <button>クリックしてください</button>
</div>
<pre id="output"></pre>


<script>

const output = document.querySelector("#output");
function handleClick(e) {
  output.textContent += `${e.currentTarget.tagName} 要素をクリックしました\n`;
  
}

const container = document.querySelector("#container");
const button = document.querySelector("button");

document.body.addEventListener("click", handleClick);
container.addEventListener("click", handleClick);
button.addEventListener("click", handleClick);


 </script>
```

上記のコードを実行すると、イベントのバブリングによって以下のように表示される。

```
/* 実行結果 */
BUTTON 要素をクリックしました
DIV 要素をクリックしました
BODY 要素をクリックしました
```

それでは次に`stopPropagation()メソッド`を**追加することで、実行結果がどのように変わるのか見ていく。**

上記のコードのイベント関数内を以下のように変更する。
```
function handleClick(e) {
  output.textContent += `${e.currentTarget.tagName} 要素をクリックしました\n`;
　e.stopPropagation(); /* ここに追加 */
  
}
```

すると実行結果は以下のようになる。

```
BUTTON 要素をクリックしました
```

`stopPropagation()メソッド`により、イベントバブリングが`button要素`のイベント処理が終わった時点で停止されたことがわかる。

### for-of文

for-of文とは、**イテレータの中身を1つずつ取り出して処理する文**のこと。

イテレータ（iterator）とは、ざっくり言うと**一連の複数のデータからなるものを指す。**

for-of文に渡す配列や文字列のことを**iterable**と呼ぶ。

そして**for-of文にiterableが渡されると、それに対応するイテレータが作られ、そのイテレータにより値が順番に取り出される。** 参照:[イテレータ](https://uhyohyo.net/javascript/16_2.html)

**使い方**

以下のように書く。下記のコードで`of "Hello!"`となっている部分を、配列名の`arr`にしても動く。

```
 <body>

<pre id="output"></pre>

<script>

let arr=[0,1,2,3];
const output = document.querySelector("#output");

for(let value of "Hello!"){
 output.textContent += value;
 
}

 </script>
  </body>
```

```
/* 実行結果 */
Hello!

/* ofの後をarrにした場合の実行結果 */
0123
```

### onclick属性は使わない方が良い

HTMLにJavaScriptを埋め込む時、`onclick属性`を使うやり方が紹介されることがあるが、これは使うべきではないとされている。

**理由は、大きく2つ。**

1つ目は、バグの原因となりやすいこと。

2つ目は、コードのメンテナンスのしやすさの観点から非効率となること。

順番に詳しく見ていく。

**①バグの原因となりやすい**

`onclick属性`を使用した時にバグが発生する具体例として、例えば`form要素`にて`onclick属性`を使用した時に発生する。

どういうことかというと、**`onclick属性`で呼び出す関数名と、`form要素`で指定したname属性の値の名前が同じ時に上手く動作しない現象が発生する。**

実際のコードは以下になる。

```
<body>

<form onsubmit="return false;">
  <input type="submit" name="hello" onclick="hello()">
</form>

<script>
function hello() {
  alert('hello')
}
</script>

</body>
```

上記のコードを実行しようとすると以下のようなエラーメッセージが表示される。
```
 Uncaught TypeError: hello is not a function
    at HTMLInputElement.onclick

訳:キャッチされない TypeError: hello は関数ではありません
HTMLInputElement.onclick
```

このエラーを解消するには、`input要素`のname属性の値を**関数名とは違う名前に変更すればok。**

```
/* name属性の値をhelloからhelloEverybodyへ変更した */
<form onsubmit="return false;">
  <input type="submit" name="helloEverybody" onclick="hello()">
</form>
```

**補足解説**

ちなみに、上記のコードの`<form onsubmit="return false;">`の記述の意味を解説しておく。

まず`onsubmit`はイベント属性の一つで、**送信処理がなされた時にどのような動作をするか**を決定する属性。

ここで、onsubmit属性の値を`"return false;"`とすることで、**フォームの送信処理がキャンセルできる。** (つまり送信処理自体が行われなくなる) 参照:[onsubmit属性について](https://dtnavi.tcdigital.jp/cat_system/language_041/)

この`return false`は、上の方で以前学んだ`preventDefault()メソッド`と同じ効果があるとのこと。参照:[return falseの意味や使い方](https://boukenki.info/javascript-return-false-imi-tukaikata/)

フォームの送信処理自体が行われなくなるため、以下のようにURLの末尾も変化しない。

```

mdn-learning.html
```

```
 <body>

<button>クリック</button>
<button>クリック</button>
<button>クリック</button>


<script>

const buttons = document.querySelectorAll("button");

function buttonChange(){
  alert("Hello! everybody");
  }

for (const button of buttons) {
  button.addEventListener("click", buttonChange);
}

 </script>

```















