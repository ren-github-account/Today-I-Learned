ApacheでPerlを動かすにはいくつか設定を行う必要がある。

### Apacheのコンフィグファイルの設定

**コンフィグ(config)** とは、英語のconfiguration(意味:設定)の略で、confやcfgとも。

まず、
>>httpd.confを開いてください。
>>httpd.confの最終行に
>AddHandler cgi-script .cgi .pl
>と書き加えます。

|||
|-|-|
