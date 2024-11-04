### XAMPPとは

「Apache＋MariaDB＋PHP」をまとめてワンパッケージにしたアプリケーションのこと。

MariaDBはMySQLと同じようなデータベースのひとつ。

**豆知識**
```
XAMPPはもともと「LAMPP」と称していました。これは、Linux＋Apache＋MySQL＋PHP＋Perlのそれぞれの頭文字を取ったものです。
その後、対応OSをWindowsとMacも加えてクロスプラットフォームが実現したところから、
Linuxの「L」をクロスの「X」に変更して現在の名称になっています。また、2015年10月にMySQLをMariaDBに変更して現在に至っています。
```
引用:https://atmarkit.itmedia.co.jp/ait/articles/1403/07/news028.html

### PHPを動かす手順メモ

①XAMPPのインストールフォルダの中から「htdocs」を探す。(htdocsはHyperText DocumentSの略)

この「htdocsフォルダ」のことを**ドキュメントルート**と呼ぶ。ドキュメントルートとは「ネットワークからアクセス可能なフォルダのこと」。

ブラウザからアクセスしたいファイルは、全てこのドキュメントルートである「htdocs」の直下に配置する。

②htdocsフォルダの中にPHPプログラムを格納するためのフォルダを作る。名前は自由でOK。

③ブラウザで以下のURLを開くと動かすことができる。

```
http://localhost/②で作成したフォルダ名/
```

### localhostの意味

localhostは「自分自身、つまり、ブラウザが起動したそのPCを表す」。

localは英語で「特定の場所」といった意味があり、ホストは英語では「主催者」などの意味があるがITの分野では「ネットワークに接続されたコンピュータ」のことを指す場合が多い。






