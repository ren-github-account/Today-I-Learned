*目次*
* [概要](#概要)
* [古典的OOP風のクラス定義](#古典的OOP風のクラス定義)
* [使い方](#使い方)
* [サブクラスを定義する](#サブクラスを定義する)

### 概要

本ページは以下の記事の内容を自分なりにまとめたものである。

https://developer.mozilla.org/ja/docs/Learn/JavaScript/Objects/Classes_in_JavaScript

### 古典的OOP風のクラス定義

このトピックでは、JavaやC++などの古典的なオブジェクト指向プログラミング(OOP)に近い形でJavaScriptのクラス定義する方法を学ぶ。

古典的なOOPに近い形で**クラスを宣言**するには以下のように書く。

```
class クラス名 { プロパティ名; }　/* このように先頭にclassを付ける。「プロパティ名;」の箇所はオプションなので無くても動くが、
                                   ここにコンストラクターの部分で書くプロパティ名を記しておくことで、コードを読み人にとって
　　　　　                          分かりやすくなる。
```

また、class内では**コンストラクターを定義**することができ、その場合は`constructor`というキーワードを使って以下のように書く。  
**このコンストラクターの箇所は省略してもOK。**

```
constructor(引数) {}
```

この`constructorキーワード`を使って定義されたコンストラクターは、  
[MDN-オブジェクトの基本](https://github.com/ren-github-account/Today-I-Learned/blob/main/JavaScript/MDN-%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88%E3%81%AE%E5%9F%BA%E6%9C%AC.md#%E3%82%B3%E3%83%B3%E3%82%B9%E3%83%88%E3%83%A9%E3%82%AF%E3%82%BF%E3%83%BC%E3%81%AE%E5%B0%8E%E5%85%A5)のところでやったのと同じように以下の処理が行われる。

```
・新しいオブジェクトを生成する

・新しいオブジェクトに this を結びつけ、コンストラクターのコードで this を参照することができるようにする

・コンストラクターでコードを実行する

・新しいオブジェクトを返す
```

クラス内では以下のように**メソッドも定義**することができる。(メソッドの記述には[MDN-オブジェクトの基本](https://github.com/ren-github-account/Today-I-Learned/blob/main/JavaScript/MDN-%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88%E3%81%AE%E5%9F%BA%E6%9C%AC.md)でやった省略記法を使用している)

```
class クラス名 {
  プロパティ名;

  constructor(引数){
  }

  プロパティ名(){ /* メソッド部分。ここでは省略記法を使用している。
  }

}
```

**メソッドはプロトタイプで定義し、データプロパティはコンストラクターで定義する**という考え方は[オブジェクトのプロトタイプ](https://developer.mozilla.org/ja/docs/Learn/JavaScript/Objects/Object_prototypes)(独自プロパティの箇所を参照)のページで学んだが、この考え方はクラスを宣言する時にも使えると思った。

つまり、**他のオブジェクトに使いまわすことが多い部分に関しては、クラスで定義し、個別に設定する必要のある部分についてはインスタンスで定義する**といった具合に。

### 使い方

クラス宣言されたものを使ってインスタンスを作成するやり方は、**コンストラクターのやり方と同じ**で以下のコードのように**先頭に`new`を付けて呼び出せばOK。**(参照:[コンストラクターメモ書き](https://github.com/ren-github-account/Today-I-Learned/blob/main/JavaScript/MDN-%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88%E3%81%AE%E5%9F%BA%E6%9C%AC.md#%E3%82%B3%E3%83%B3%E3%82%B9%E3%83%88%E3%83%A9%E3%82%AF%E3%82%BF%E3%83%BC%E3%81%AE%E5%B0%8E%E5%85%A5))

```
const オブジェクト名 = new クラス名(引数); /* 引数の箇所には「"人名"」などを入力する
```

つまり、クラスを使ってインスタンスを作成する時も結局はコンストラクターを使用するため、使う時もコンストラクターの時と同じになるということだね。

実際、MDNのページ[オブジェクト指向プログラミング](https://developer.mozilla.org/ja/docs/Learn/JavaScript/Objects/Object-oriented_programming)に以下の記述がある。(この記述は[メモ書き](https://github.com/ren-github-account/Today-I-Learned/blob/main/JavaScript/MDN-%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88%E6%8C%87%E5%90%91%E3%83%97%E3%83%AD%E3%82%B0%E3%83%A9%E3%83%9F%E3%83%B3%E3%82%B0(%E3%82%AF%E3%83%A9%E3%82%B9%E3%81%A8%E3%82%A4%E3%83%B3%E3%82%B9%E3%82%BF%E3%83%B3%E3%82%B9%E3%81%AA%E3%81%A9).md#%E3%82%AF%E3%83%A9%E3%82%B9%E3%81%A8%E3%82%A4%E3%83%B3%E3%82%B9%E3%82%BF%E3%83%B3%E3%82%B9%E3%81%A8%E3%81%AF)の箇所でも引用していた)

```
インスタンスを作成する処理は、コンストラクターと呼ばれる特別な関数によって実行されます。
コンストラクターには、新しいインスタンスで初期化したい内部状態を表す値を渡します。
```

上記の引用文のうち**以下の記述の意味**が、今やっと理解できた。

>コンストラクターには、新しいインスタンスで初期化したい内部状態を表す値を渡します。

つまり、コンストラクターを使って**新しくオブジェクト(インスタンス)を作成するたびに初期化したい値を、コンストラクター関数内に記述すれば良い**ということか。

実際、以下に引用した[MDNのページ](https://developer.mozilla.org/ja/docs/Learn/JavaScript/Objects/Classes_in_JavaScript)にある具体例のコードを見ても、新しくオブジェクトを作成するごとに**初期化する必要のあるnameプロパティの値**が代入されているし、[メモ書き-コンストラクターの代入](https://github.com/ren-github-account/Today-I-Learned/blob/main/JavaScript/MDN-%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88%E3%81%AE%E5%9F%BA%E6%9C%AC.md#%E3%82%B3%E3%83%B3%E3%82%B9%E3%83%88%E3%83%A9%E3%82%AF%E3%82%BF%E3%83%BC%E3%81%AE%E5%B0%8E%E5%85%A5)のところで書いたコードでも同じように**初期化処理**が行われている。

**MDNのページにある具体例のコード**
```
constructor(name) {
    this.name = name;
  }
```

**メモ書きで書いたコード**
```
function createPerson(name){
  this.userName = name; /* ここで初期化処理を行っている
  this.introduceSelf = function(){
    console.log(`Hi! I'm ${this.userName}.`);
   };
 }

```

### サブクラスを定義する

サブクラスを定義するには`extends`を使用して以下のように書く。英語で`extends`は「長くしたり幅を広くしたりする」といった意味がある。

