### エラーメッセージ

`PHP Fatal error:  Uncaught Error: Undefined constant "こんにちわ" in C:\php_program\echo.php:4`

### エラーが発生したソースコード

    <？php

    echo こんにちわ;

    ？>

### 原因と対処
「こんにちわ」をクォーテーションで囲わずに入力したらどうなるかを試してみた結果発生した。  
クォーテーションで囲うことで解決。
