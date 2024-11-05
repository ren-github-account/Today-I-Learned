### console.logとは
console.logは、コンソール画面に指定した文字列や変数の中身などを表示させることのできるconsoleオブジェクトの一つ。

説明追記
```
基本的に、console.log() は指定されたオブジェクトを代表するような文字列を最初に表示します。
```
引用:https://webfrontend.ninja/js-console-object/

**具体例**

```
function fncCall()
{
 var a ="こんにちわ!";
 document.write(a);
}

let text ="いえーい!";

console.log("たのちぃ!");
console.log(text);
console.log(fncCall());

```

コンソール画面の出力結果
```
たのちぃ! 
いえーい! 
undefined　//関数を呼び出そうとしたらなぜか、この表示になる
```

### 具体例その2

```
function test(){
  console.log("hello");
  return 100;
}


test();

console.log(test());
```

コンソール画面の出力結果
```
hello
hello //挙動として、まず関数の実行処理が行われてhelloが表示され、その後に戻り値である100が表示されるという順序になっているな
100
```
