*目次*
* [基本](#基本)

### 基本

    switch(判定する値){
     case 値1:
     実行する処理
     break;

     case 値2:
     実行する処理
     break;

     default:
     実行する処理
     }

**具体例**

    <?php

    $a[0] = 1;
    $a[1] = 2;
    $a[2] = 3;

    switch($a){
    
     case 1:
     echo "$a[0]番目の配列だよ～ん";
     break;

     case 2:
     echo "$a[1]番目の配列だよ～ん";
     break;

     case 3:
     echo "$a[2]番目の配列だよ～ん";
     break;

     default:
     echo "なんもないやんけ!";
     break;
    
    ?>
