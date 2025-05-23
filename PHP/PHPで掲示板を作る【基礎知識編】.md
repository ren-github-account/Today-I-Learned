*目次*
* [概要](#概要)
* [PHP](#PHP)
* [三項演算子](#三項演算子)
* [isset](#isset)
* [fopen](#fopen)
* [fwrite](#fwrite)
* [fgets](#fgets)
* [explode](#explode)
* [ダブルアロー演算子](#ダブルアロー演算子)
* [多次元配列](#多次元配列)
* [キーの省略](#キーの省略)
* [echo](#echo)
* [$_POST](#POST)
* [endforeach](#endforeach)
* [JavaScript](#JavaScript)

## 概要

以下のページを参考にPHPで掲示板を作成する時に調べたことをまとめる。

https://web.archive.org/web/20240123041312/https://skill-up-engineering.com/gachinko/?p=650

## PHP
### 三項演算子

if文の代わりとして使用され、以下のように書く。

```
条件文 ? 真の時に実行される式 : 偽の時に実行される式 ;
```

**具体例**

この三項演算子はJavaScriptにもあるので以下の例はJavaScriptの例をのせている。( 参照:[MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Operators/Conditional_operator) )

```
const age = 26;
const beverage = age >= 21 ? "ビール" : "ジュース";
console.log(beverage); // "ビール"
```

**解説**

条件文`age >= 21`が`true`であることから、変数`beverage`に`ビール`が格納されて`console.log();`で表示されている。

変数名の`beverage`は英語で「飲み物」という意味。

### isset

`isset`の戻り値は「変数が存在して`null`以外の値をとれば`true`、 そうでなければ`false`を返す」。

**具体例**

例えば以下のコードを書くと、`isset`は戻り値として`true`を返す。

```
<?php

$var = '';

// これは TRUE と評価されるので、テキストが出力される
if (isset($var)) {
    echo "This var is set so I will print.";
}
```

### fopen

`fopen`は「ファイルまたはURLをオープンする」する。

使う時は以下のように書く。

```
fopen( “ファイル名” ,”パラメータ” );
```
パラメータの部分では、ファイルに対するアクセス形式を指定する。

例えば`fopen( “data.txt” ,”a” );`のように書くと、`a`は「追加の書き込み」を表すのでファイルに対して「追加の書き込み」ができるようになる。

### fwrite

`fwrite`は取得したファイルポインタに対する書き込み処理を行う。

書き方は以下のように書く。

```
fwrite(ファイルポインタ、書き込み内容);
```

ファイルポインタとは、ファイルの入力、出力をする際に必要となる本のしおりのようなもので、

このしおりに入力、書き込みをすることで実際のファイルへの入力、出力ができるようになる。(つまり仲介するってことか)

ファイルポインタは以下のように書くことで取得できる。

```
$fp = fopen( “data.txt” ,”a” );
```

変数`fp`にファイルポインタが格納される。

**具体例**

```
fwrite( $fp ,  $name.”\t”.$comment.”\n”);
```

**解説**

「`.`」は連結演算子といい、変数と文字列を結合させることができる。

`\t`はタブのことで、タブキーを押した時にできる空白を表す。

`\n`は改行を表す。

### fgets

`fgets`は「ファイルポインタから1行取得して戻り値として返す」。そして、ファイルポインタから読み込むデータがもうない場合は`false`を返す。

`false`を返すということは、`while()`などの条件文として使用できる。

書き方は以下のように書く。

```
fgets(ファイルポインタ)
```

このファイルポインタは、公式マニュアルによると「有効なファイルポインタである必要があり、`fopen()`または`fsockopen()`で正常にオープンされた（そしてまだ`fclose()`でクローズされていない)ファイルを指している必要があります」とのこと。

**具体例**

```
 $res = fgets($fp)
```

**解説**

変数`$fp`に格納されたファイルポインタを1行ずつ読み込み、その結果を変数`$res`に格納している。

### explode

`explode`は、指定した文字列をこれまた指定した分割記号によって分割した配列を返す。

書き方は以下のように書く。

```
explode(分割記号, 文字列)
```

**具体例**

```
$pizza  = "piece1 piece2 piece3 piece4 piece5 piece6";
$pieces = explode(" ", $pizza);
echo $pieces[0]; // piece1
echo $pieces[1]; // piece2
```

### ダブルアロー演算子

配列で出てくる`=>`はダブルアロー演算子と呼ばれ、意味は「連想配列のこのキーにこの値を入れる」ということを表している。

**具体例**

具体的には以下のように書く。

```
<?php
$array = array(
    "foo" => "bar",
    "bar" => "foo",
);

// 配列の短縮構文
$array = [
    "foo" => "bar",
    "bar" => "foo",
];
?>
```

例えば以下のコードだったら、「`name`をキーに持つ連想配列に`$tmp[0]`の値を入れますよ」という意味。

```
 $arr = array(
        "name"=>$tmp[0],
        "comment"=>$tmp[1]
    );
```

### 多次元配列

多次元配列は、「配列の中に配列が入っているもののこと」を指す。( 参照:[javadrive](https://www.javadrive.jp/php/array/index6.html) )

**具体例**
```
$maker = array('富士通', 'NEC', 'Sony', 'Sharp');
$type = array('Note', 'Desktop');

$pc = array($maker, $type);

print $pc[0][1];    // NEC と出力
print $pc[1][0];  　// Note と出力
```

### キーの省略

配列のキーは以下のように省略して書くこともできる。

```
$arr[] = value;
```

### echo 

`echo`は、「1 つ以上の文字列を出力する」。

主にPHPで作成したデータをHTMLへ表示する時に使用する。

### POST

**書き方**

フォームで使用する時は以下のように`$_POST`のキーにname属性の値を入力すればok。
```
$_POST["name属性の値"]
```

`form`の`method属性`を「post」にすると、送信されたデータは、`$_POST`という**連想配列（スーパーグローバル変数）に格納される。**

送信ボタンの`name属性`が「send」をに設定した場合、スーパーグローバル変数`$_POST`のキーに「send」を指定して`$_POST["send"]`とする。( 参照:[webdesignleaves](https://www.webdesignleaves.com/pr/php/php_basic_06.php) )

### endforeach

`if文`や`foreach文`を書く時は、波括弧`{}`を使う代わりに以下のように書くことも可能。( 参照:[PHP公式マニュアル](https://www.php.net/manual/ja/control-structures.alternative-syntax.php) )

```
// endifを使用した場合
<?php if ($a == 5): ?>
Aは5に等しい
<?php endif; ?>

// 通常のif文
if (条件) {
  処理
}
```

## JavaScript

### entries

`entries()`は、配列内の各要素に対するキー/値のペアを含む新しい配列iterableオブジェクトを返す。( 参照:[MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array/entries) )

**具体例**

```
const a = ["a", "b", "c"];

for (const [index, element] of a.entries()) {
  console.log(index, element);
}

/* 実行結果 */
// 0 'a'
// 1 'b'
// 2 'c'
```











