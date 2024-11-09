### addEventListenerとは
```
ユーザーが特定の操作（クリック、マウスオーバー、キーボード入力など）を行ったときに、
その操作を検知して指定された処理を実行するための仕組みです。
```
引用:https://pikawaka.com/javascript/addeventlistener

### イベントとは

イベントとは、クリック, マウスオーバー, キーボードのキーが押された時などのことを指す。

例えば以下の例は`click`が指定されているため、「クリックイベント」となる。

```
const button = document.getElementById('myButton');

button.addEventListener('click', function() {
  alert('ボタンがクリックされました！');
});
```
引用:上記と同じ

### 使い方

構文
```
要素.addEventListener("event", listener, options)
```

**説明**
||意味|
|-|-|
|`event`|イベントの種類。例:`click`,`mouseover`,`mouseout`など|
|`listener`|イベント発生時に実行される関数。|
|`options`|オプション設定（省略可能）。|
