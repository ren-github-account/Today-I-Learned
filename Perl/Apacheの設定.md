ApacheでPerlを動かすにはいくつか設定を行う必要がある。

### Apacheのコンフィグファイルの設定

**コンフィグ(config)** とは、英語のconfiguration(意味:設定)の略で、confやcfgとも。

まず、

>httpd.confを開いてください。
>httpd.confの最終行に
>AddHandler cgi-script .cgi .pl
>と書き加えます。

* **コードの意味**

AddHandlerを使った記述のルールを見ていく。

**基本**
```
AddHandler handler-name extension
```

||意味|
|-|-|
|AddHandler|ハンドラを設定するという意味。ハンドラとは「動作の総称」のこと。|
|handler-name|ハンドラの名前。例:`cgi-script`はcgiを使用するという意味になる。|
|extension|ハンドラを設定する対象の拡張子を入力。複数指定する場合は半角スペースで区切る。英語extensionは「拡張」の意味。|
