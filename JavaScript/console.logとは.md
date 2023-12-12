### console.logとは
console.logは、コンソール画面に指定した文字列や変数の中身などを表示させることのできるconsoleオブジェクトの一つ。

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
