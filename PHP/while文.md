*目次*
* [基本](#基本)
* [実践的なwhile文](#実践的なwhile文)
* [do while文](#do while文)

### 基本

    while(条件式){
     実行する処理
    }

基本的な書き方はjavaと同じ。

**具体例**

    <?php
    $index = 1;

    while($index < 10){
    echo "<p>"."$index"."回目の死に戻り"."「俺は絶対君を助ける」"."</p>";
    $index ++;
    }
    
    ?>

### 実践的なwhile文

```
<?php

$sum = 0;
$index = 0;
$item = array();

$item[0] = array("price" => 7980);
$item[1] = array("price" => 2980);
$item[2] = array("price" => 4400);

while(isset($item[$index])){

  $sum += $item[$index]["price"];
  $index ++;
}

echo "合計金額:￥".$sum;

?>
```

**実行結果**

`合計金額:￥15360`

**isset関数**

`isset()`

isset関数は引数で渡した変数が存在するかをチェックする。存在する場合は「true」を、存在しない場合は「false」を返す。

### do while文

```
do {
 実行する処理
} while(条件式);
```  
