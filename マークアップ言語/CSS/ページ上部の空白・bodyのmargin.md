*目次*
* [原因の調査](#原因の調査)
* [対処](#対処)
* [もっと良い方法](#もっと良い方法)
* [bodyのデフォルトのmargin](#bodyのデフォルトのmargin)

以下のCSSリセットを適用したのに、p要素の上側に空白が表示されるという事態が発生。

```
body{
 margin:0;
 padding:0;
 box-sizing:border-box;
}
```

### 原因の調査

該当のp要素をデベロッパーツールで「調査」を選択し、右側のレイアウトタブを下にスクロールしていくとボックスモデルやボックスモデルのプロパティが見れる。

そこで確認したところ、p要素の上下に`16px`のmarginが…。

### 対処

以下のように原因個所のmarginを個別に0にしてあげることで、解決はできた。
```
p{
 margin:0;
}
```

### もっと良い方法

今回のような場合には、個別に0にするより以下のように全称セレクタを使うのが一般的のようだ。

そして、marginが必要な時は各要素に個別に設定していく。

```
* {
 margin:0;
 padding:0;
}
```

### bodyのデフォルトのmargin

body要素にデフォルトで、上下左右に8pxのmarginが設定されている。

これは、user agent stylesheetというブラウザが各要素にたいしてデフォルトで適用しているCSSが原因。

これをリセットして、作業環境を整えるために生まれたのが「リセットCSS」。

