*目次*
* [webページが表示される仕組み](#webページが表示される仕組み)
* [phpの特徴](#phpの特徴)

phpはwebサーバー上で動く。  

### webページが表示される仕組み  
以下の順番で表示される。 

|表示の流れ|
|---|
|①ブラウザがwebサーバーに「このページくださ～い」とリクエストを送る。|  
|②リクエストを受け取ったwebサーバーから「ほれ、そのページだよ」と返事がくる。|  
|③ブラウザは受け取ったページを表示する。|

上の流れの中では、phpは②の時に動いている。  
具体的には、webサーバーがリクエストを受け取った時、phpが動いてページを作成し、  
出来たページをブラウザに返すイメージ。

### phpの特徴

* **htmlと相性が良い**

phpはhtmlととても相性が良く、htmlの中にphpを埋め込むことが可能。

* **コンパイル不要**  

実行時にコンパイルされるため、コンパイルを意識しなくてよい。  
反面、文法ミスなどが動かすまでわからないというデメリットがある。

コンパイルが不要な理由は、phpが**インタプリタ型**の言語だから。  
インタプリタとは、ソートコードとCPUの間にある翻訳機のことで、ソースコードを1行ずつ翻訳しながら実行する。

* **データベースとの連携に優れる**

PHPはデータベース関連の機能が充実している。Perl や C/C++ で作るプログラムはHTML文書から独立して外部に置かれるが、PHP は少し変わっていて、HTML文書中に　`<?php ～?> `というような処理命令を埋め込んでプログラムを記述できる。

