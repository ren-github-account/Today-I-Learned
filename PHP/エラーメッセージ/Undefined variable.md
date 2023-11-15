### エラーメッセージ
`PHP Warning:  Undefined variable $nameさん in C:\php_program\echo.php on line 4`

### エラーが発生したソースコード
    <?php

    $name = '高橋';
    echo "こんにちわ。$nameさん";

    ?>
### 対処

「$name」と「さん」の間に半角スペースを空けることで解決。  
もしくは、半角スペースを空ける代わりに{$name}として、{}で囲うのでもokだった。
