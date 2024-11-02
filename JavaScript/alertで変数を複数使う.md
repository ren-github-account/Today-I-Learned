### 実現したいこと

alertを使って複数の変数に格納した値を表示させる。

### 解決策

変数を`+`を使ってつなげる。

### コード

**JavaScript**
```
//1～3までの数字をランダムで生成するプログラム

function fncCall() 
{
  var random1 = Math.floor(Math.random() * 3) + 1;
  var random2 = Math.floor(Math.random() * 3) + 1;
  var random3 = Math.floor(Math.random() * 3) + 1;
  alert(random1 + ":" + random2 + ":" + random3);
}
  
```

**HTML**
```
<!DOCTYPE html>
<head>
<link rel="stylesheet" href="style.css">
<title>サイコロプログラム</title>
</head>

<body>
<script src="nandemo_2.js"></script>
<button type="button" onclick="fncCall()">回す</button>
</body>

</html>
```

### 参考

https://uxmilk.jp/14158
