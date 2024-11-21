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

**構文**
```
ターゲット.addEventListener("event", listener, options)
```

**説明**

上記のコードにある**ターゲット**とは、イベントターゲットのことで、`addEventListener`の対象としたい要素やオブジェクトを記述する。詳しくは[EventTargetインターフェースの項目](https://github.com/ren-github-account/Today-I-Learned/blob/main/JavaScript/MDN-%E3%82%A4%E3%83%99%E3%83%B3%E3%83%88%E5%85%A5%E9%96%80(&DOM%E3%81%AE%E5%9F%BA%E7%A4%8E%E7%9F%A5%E8%AD%98).md#DOM%E3%81%AE%E5%9F%BA%E7%A4%8E%E7%9F%A5%E8%AD%98)を参照。

||意味|
|-|-|
|`event`|イベントの種類。例:`click`,`mouseover`,`mouseout`など|
|`listener`|イベント発生時に実行される関数。|
|`options`|オプション設定（省略可能）。|
