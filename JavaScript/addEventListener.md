### addEventListenerとは
```
ユーザーが特定の操作（クリック、マウスオーバー、キーボード入力など）を行ったときに、
その操作を検知して指定された処理を実行するための仕組みです。
```
引用:https://pikawaka.com/javascript/addeventlistener

### イベントとは

イベントとは、クリック, マウスオーバー, キーボードのキーが押された時などが該当する。

例えば以下の例は`click`が指定されているため、「クリックイベント」となる。

```
const button = document.getElementById('myButton');

button.addEventListener('click', function() {
  alert('ボタンがクリックされました！');
});
```
引用:上記と同じ
