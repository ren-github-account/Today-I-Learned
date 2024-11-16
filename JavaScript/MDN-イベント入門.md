*目次*
* [概要](#概要)
* [イベントとは](#イベントとは)
* [イベントハンドラーとは](#イベントハンドラーとは)
* [イベントリスナーの使い方](#イベントリスナーの使い方)
* [querySelectorメソッド](#querySelectorメソッド)
* [リスナーを削除する](#リスナーを削除する)

### 概要

本ページは以下の記事の内容を自分なりにまとめたものである。

https://developer.mozilla.org/ja/docs/Learn/JavaScript/Building_blocks/Events

### イベントとは

イベントが発生すると、システムは何らかのシグナルを発生させ(これを**発火**という)、何らかのコードが実行されるメカニズムを提供する。
```
イベントが発生すると、システムは何らかのシグナルを発生（または「発火」）させ、
イベントが発生したときに自動的にアクションが実行される（つまり、何らかのコードが実行される）メカニズムを提供します。
```

イベントの具体例として以下のものがある。

```
・ユーザーが特定の要素を選択したり、クリックしたり、カーソルを当てたりする

・ユーザーがキーボードのキーを押す

・ユーザーがブラウザーウィンドウをリサイズしたり閉じたりする

・ウェブページの読み込みが完了する

・フォームが送信される

・動画が再生される、停止される、再生が終わる

・エラーが発生する
```

### イベントハンドラーとは

イベントハンドラーとは、**イベントが発生した時に実行するコードのまとまり(通常はJavaScript関数)のこと。**

イベントハンドラーがイベントに反応して実行するように設定されていることを、**イベントハンドラーを登録**していると言う。

豆知識:ハンドラーは英語でhandlerと書き、その接尾辞の「er」は「～する人・もの」を表す。(出典:龍谷大学 接尾辞-erと-eeについて) 英語でhandleには「操作」という意味や「制御される部分」といった意味があるので、「制御されるもの、あるいは操作するもの」という意味になる。  
つまり、イベントハンドラーは「イベントを操作するもの」となる。まあ、要は「機能を付与する」ってことだね。

またイベントハンドラーは、**イベントリスナー**と呼ばれることもある。

**イベントハンドラー**と**イベントリスナー**は厳密に言えばちがうものでその違いは以下の表にまとめた。

||違い|
|-|-|
|イベントリスナー|イベントの発生に耳を傾ける。(つまりperceive(知覚する)的な意味)|
|イベントハンドラー|発生したイベントに応答して動作するコードのこと。|

Listenの意味については、[この](https://www.grammar-monster.com/easily_confused/hear_listen.htm)サイトが図解付きでわかりやすかった。

### イベントリスナーの使い方

イベントハンドラーを追加するには、`addEventListener()メソッド`を使用する。

`addEventListener()`の使い方をググると、以下のコードのように`addEventListener()`内に直接、関数の処理を記述しているパターンをよく見かける。

**直接記述パターン**
```
const btn = document.querySelector("button");

function random(number) {
  return Math.floor(Math.random() * (number + 1));
}

btn.addEventListener("click", () => {
  const rndCol = `rgb(${random(255)} ${random(255)} ${random(255)})`; /* 上で定義したrandom関数を参照してはいるが、
　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　関数の処理をaddEventListener()内に直接記述している
  document.body.style.backgroundColor = rndCol;
});
```

しかし、上記の直接記述パターンは個人的に**見づらい**と感じる。そういう時は以下のように、**関数名のみを記述するやり方でもOK。**

**関数名のみ記述パターン**
```
const btn = document.querySelector("button");

function random(number) {
  return Math.floor(Math.random() * (number + 1));
}

function changeBackground() {
  const rndCol = `rgb(${random(255)} ${random(255)} ${random(255)})`;
  document.body.style.backgroundColor = rndCol;
}

btn.addEventListener("click", changeBackground); /* 関数名のみを記述している。ここで使用する関数は上で定義している。
```

### querySelectorメソッド

`addEventListener()`と一緒によく使われるメソッド。

querySelectorメソッドの**戻り値**は、「指定された**CSSセレクター**のset(まとまり)に一致するドキュメント内の最初の要素を表現する**Elementオブジェクト**を返す。一致するものがない場合は`null`を返す」。

上記の説明文でわからない単語が2つ出てきたので以下の表にまとめる。(2つの単語以外にもその前提となる単語をまとめている)

||意味|
|-|-|
|Element|Elementオブジェクトを理解するには、そもそもElementとは何かを理解する必要がある。Elementとは、Document内のすべての要素オブジェクト (つまり、要素を描画するオブジェクト) がinherit(継承する)最も一般的で**ベース(基礎)となるクラスのこと。**(クラスについて詳しくは[こちら](https://github.com/ren-github-account/Today-I-Learned/blob/main/JavaScript/MDN-%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88%E6%8C%87%E5%90%91%E3%83%97%E3%83%AD%E3%82%B0%E3%83%A9%E3%83%9F%E3%83%B3%E3%82%B0(%E3%82%AF%E3%83%A9%E3%82%B9%E3%81%A8%E3%82%A4%E3%83%B3%E3%82%B9%E3%82%BF%E3%83%B3%E3%82%B9%E3%81%AA%E3%81%A9).md#%E3%82%AF%E3%83%A9%E3%82%B9%E3%81%A8%E3%82%A4%E3%83%B3%E3%82%B9%E3%82%BF%E3%83%B3%E3%82%B9%E3%81%A8%E3%81%AF)と[こちら](https://github.com/ren-github-account/Today-I-Learned/blob/main/JavaScript/MDN-%E3%82%AF%E3%83%A9%E3%82%B9(%E5%8F%A4%E5%85%B8%E7%9A%84OOP%E9%A2%A8).md)を参照)|
|Document|Documentは**ブラウザで読み込まれたwebページを表す。** そしてDocumentはDOMツリーであるウェブページのコンテンツへの**入口**としての役割を果たす。**DOMツリーとは**ブラウザがHTML文書を解釈する時に構築するものだが、**DocumentはこのDOMツリーの一番上に位置する。**(参照:[DOMツリーのイメージ図](https://developer.mozilla.org/ja/docs/Web/API/Document_Object_Model/Using_the_Document_Object_Model#dom_%e3%83%84%e3%83%aa%e3%83%bc%e3%81%a8%e3%81%af%ef%bc%9f))つまり、平たくいえば、**DocumentはDOMツリーへの入り口を意味する。**|
|Elementオブジェクト|上述のElementクラスを使って作成されたオブジェクトのこと。Elementオブジェクトのプロパティの具体例には**innerHTMLプロパティ**などがある。その他にも**id**や**classname**や**子要素**などがある。詳しくは[こちら](https://developer.mozilla.org/ja/docs/Web/API/Element/innerHTML)|
|Node(ノード)|**NodeとはブラウザがHTMLを解釈する時に作られるもの。** このNode機能を持つすべてのオブジェクトは、例えばDocumentオブジェクトやElementオブジェクトのクラスから派生したサブクラスがベースとなっている。(つまり、Node機能を持つオブジェクトの親クラスにはDocumentクラスやElementクラスが存在する)|
|innerHTMLプロパティ|HTML内にあるテキストや`<p></p>`などのタグそれ自体などを置き換えたり取得したりできる機能を持つ。(参照:[innerHTMLの使い方](http://javascriptmania.blog111.fc2.com/blog-entry-18.html)) **innerHTMLで値を代入すると、その値として与えられたHTMLを解釈して構築されたノードに置き換える。** **注意点**としてinnerHTMLはXSS防止の観点から非推奨で、**代わりに`textContentプロパティ`の使用が推奨される。**|
|Serialization (シリアライズ)|オブジェクトやデータ構造が転送に適したフォーマットに変換されること。(参照:MDN用語集より)|
|CSSセレクター|CSSを使って**どのHTML要素に変更を加えるかをブラウザーに伝えるもの。** ここで選択された要素にはCSSプロパティ値 (property value)が適用される。参照:[CSSセレクター](https://developer.mozilla.org/ja/docs/Learn/CSS/Building_blocks/Selectors)|

### リスナーを削除する

`addEventListener()`を使ってイベントハンドラーを追加した場合、`removeEventListener()メソッド`を使って再び除去することができる。

`removeEventListener()メソッド`で除去するには以下のコードを書く。

```
要素名.removeEventListener("イベントハンドラープロパティ名(例:clickなど)", 関数名);
```

このリスナーの除去は、**大規模で複雑なプログラムを書く時に役立つ。**

```
単純で小さなプログラムでは、古くて使われていないイベントハンドラーをクリーンアップする必要はありませんが、
大規模で複雑なプログラムでは、効率を向上させることができます。
また、イベントハンドラーを除去する機能により、同じボタンが異なる状況で異なるアクションをするようなことも可能です。
ハンドラーを追加したり除去されたりするだけです。
```
