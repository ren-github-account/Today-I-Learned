### buttonタグとは

ボタンを作成する時に使われる。

### type属性

type属性を使ってボタンの機能を決めることができる。

```
<button type=”*”>ボタン</button>
```

|種類|意味|
|-|-|
|submit|送信ボタン|
|reset|リセットボタン。ユーザーが入力した文字列をワンクリックで削除させたい場合などに使われる。|
|button|何の機能も付与されていないボタン。主にJavaScriptと組み合わせて使われる。|

### onclick属性

ユーザーがボタンをクリックした際に起動する処理を指定することができる。

**基本の書き方**
```
<button type=”button” onclick=”値”>ボタン</button>
```

### ※注意点

`type`や`onclick`の属性を指定する時、「`”`」で囲むとエラーが出ることがあるので「`"`」を使った方が良い。

### 補足

以下のようにtype属性を指定せずとも書くこともできるが、意図せぬエラーを防止するため原則type属性は指定した方が良い。

もしtype属性を指定しなかった場合、type属性にはデフォルト値である`submit`が付与される。  

```
<button onclick="値">ボタン</button>
```
参考:https://zenn.dev/fujiyama/articles/496e5e81ba7df9


**具体例**
```
<button type="button" onclick="fncCall()">ボタン</button>
```
解説

`fncCall()`は関数名。
