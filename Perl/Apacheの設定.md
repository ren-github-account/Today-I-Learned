### 500エラーで動かない
XAMPPのApacheを使ってPerlプログラムを動かそうとしたところ、500エラーが出て動かなかった。

**解決**
Perlのソースコードの一行目を以下のように変更すると動いた。パスが違ったことが原因だったようだ。

```
#!C:\xampp\perl\bin\perl
```

### 追記:設定は不要でした
以下のコンフィグファイルの設定は確認したところ、デフォルトでされていたので、わざわざいじる必要はなかった。

ApacheでPerlを動かすにはいくつか設定を行う必要がある。

### Apacheのコンフィグファイルの設定

**コンフィグ(config)** とは、英語のconfiguration(意味:設定)の略で、confやcfgとも。

まず、

>httpd.confを開いてください。
>httpd.confの最終行に
>AddHandler cgi-script .cgi .pl
>と書き加えます。

`httod.conf`は`apache\conf\httpd.conf`の場所にある。

* **コードの意味**

AddHandlerを使った記述のルールを見ていく。

**基本**
```
AddHandler handler-name extension
```

||意味|
|-|-|
|AddHandler|ハンドラを設定するという意味。ハンドラとは「何らかの処理要求が発生したときに起動されるもの」のこと。|
|handler-name|ハンドラの名前。例:`cgi-script`と入力した場合は、cgiを使用するという意味になる。|
|extension|ハンドラを設定する対象の拡張子を入力。複数指定する場合は半角スペースで区切る。英語extensionは「拡張」の意味。|
