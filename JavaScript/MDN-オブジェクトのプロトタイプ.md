*目次*
* [概要](#概要)
* [プロトタイプとは](#プロトタイプとは)
* [オブジェクトのプロパティにアクセスする時の検索順序](#オブジェクトのプロパティにアクセスする時の検索順序)
* [プロトタイプにアクセスする](#プロトタイプにアクセスする)
* [オブジェクトプロトタイプ](#オブジェクトプロトタイプ)
* [プロトタイプを自分で設定する](#プロトタイプを自分で設定する)

### 概要

本ページは以下の記事の内容をまとめたもの。

https://developer.mozilla.org/ja/docs/Learn/JavaScript/Objects/Object_prototypes

### プロトタイプとは

**すべてのオブジェクトはプロトタイプを持っている。**(そして、プロトタイプとは、すべての**オブジェクトに組み込まれているプロパティのこと。**)

また、プロトタイプそれ自体がオブジェクトでもあるので、**プロトタイプは自分自身でもプロトタイプを持っている。**

以下引用。
```
JavaScript ですべてのオブジェクトは、そのプロトタイプと呼ばれる組み込みプロパティを持っています。
プロトタイプはそれ自体がオブジェクトなので、プロトタイプは自分自身でプロトタイプを持ち、
プロトタイプチェーンと呼ばれるものを持ちます。
自分自身でプロトタイプにnullを持つプロトタイプに到達すると、その連鎖は終わります。
```

### オブジェクトのプロパティにアクセスする時の検索順序

オブジェクトのプロパティにアクセスする時は、以下の順序で検索が行われている。

```
オブジェクトのプロパティにアクセスしようとしたとき、オブジェクト自身にプロパティが見つからない場合は、
プロトタイプを検索してプロパティを探します。それでもプロパティが見つからない場合は、
プロトタイプのプロトタイプが検索され、プロパティが得られるか、
チェーンの終わりに達するか、その場合は undefined を返すまで、そのような具合に繰り返します。
```

### プロトタイプにアクセスする

オブジェクトのプロトタイプにアクセスには`Object.getPrototypeOf()`メソッドを使用する。

**使い方**

以下のように入力する。
```
Object.getPrototypeOf(オブジェクト名);
```

実行すると以下のように、このオブジェクトで利用できるすべてのプロパティの一覧が表示される。

```
/* 実行結果

constructor: ƒ Object()
hasOwnProperty: ƒ hasOwnProperty()
isPrototypeOf: ƒ isPrototypeOf()
propertyIsEnumerable: ƒ propertyIsEnumerable()
toLocaleString: ƒ toLocaleString()
toString: ƒ toString()
valueOf: ƒ valueOf()
__defineGetter__: ƒ __defineGetter__()
__defineSetter__: ƒ __defineSetter__()
__lookupGetter__: ƒ __lookupGetter__()
__lookupSetter__: ƒ __lookupSetter__()
__proto__: (...)　　　/* ここにあった。これがこのオブジェクトのプロトタイプを指し示すプロパティか。
get __proto__: ƒ __proto__()
set __proto__: ƒ __proto__()
```

以下引用。
```
オブジェクトのプロトタイプを指し示すプロパティは prototype という名前ではありません。
その名前は標準ではありませんが、
実際にはすべてのブラウザーが __proto__ を使用しています。
```

### オブジェクトプロトタイプ

`Object.getPrototypeOf()メソッド`は`Object.prototype`と呼ばれるオブジェクトに属する。(`Object.prototype`は、Objectオブジェクトのprototypeプロパティってことなんだろうか。よくわからんな)

また、`Object.prototypeオブジェクト`は、**すべてのオブジェクトがデフォルトで持つ、最も基本的なプロトタイプである。**

そして、`Object.prototypeオブジェクト`のプロトタイプは`null`であり、そこが**プロトタイプの連鎖の終わり**となる。  
(ここはMDNのページに図があってそれを見た方がわかりやすい。URL:[MDN-オブジェクトのプロトタイプ](https://developer.mozilla.org/ja/docs/Learn/JavaScript/Objects/Object_prototypes))

### プロトタイプを自分で設定する

JavaScriptではオブジェクトのプロトタイプを自由に設定するとことができる。

大きくわけて2つの方法がある。

||プロトタイプを設定する方法|
|-|-|
|①|`Object.create()メソッド`を使用する|
|②|コンストラクターを使用する|

順番に見ていく。

まず、`Object.create()メソッド`を使用する方法から。

**・Object.create()メソッドを使用する**

`Object.create()メソッド`は、**新しいオブジェクトを作成し、その新しいオブジェクトのプロトタイプとして使用するオブジェクトを指定することができる。**

使うには以下のように書く。

```
Object.create(プロトタイプとして使用したいオブジェクト名);
```

実際に使ってみる。以下のコードを書く。

```
const personPrototype = {
  greet() {
    console.log("hello!");
 }
};

const carl = Object.create(personPrototype); /* carlは人名。
```

動作を確認するには以下のコードを書く。

```
carl.greet(); /* 出力結果:hello!
```


**上記のコードで何が起こったのか**

`const carl = Object.create(personPrototype);`を書くことで何が行われたのかを以下の表にまとめてみた。

||処理の内容|
|-|-|
|①|まずcarlオブジェクトが作成され、`Object.create()メソッド`によってcarlオブジェクトのプロトタイプに`personPrototypeオブジェクト`が紐づけられる。|
|②|呼び出す時に`carl.greet();`と書くことで、まずcarlオブジェクト内で`greet()メソッド`が検索され、見つからなかったので、carlオブジェクトのプロトタイプとして設定されている`personPrototypeオブジェクト`内で検索がなされ、無事見つかったので実行される。|


