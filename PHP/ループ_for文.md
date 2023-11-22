* [基本](#基本)
* [foreach文](#foreach文)

### 基本

    for(初期化; 条件式; 値の更新) {
     実行する処理
     }

javaの書き方と同じ。

### foreach文

**基本**

    foreach(配列 as 配列のキー => 配列の値){
      実行する処理
      }

 ちなみに上記の「配列のキー」部分と、 「配列の値」部分には **任意の変数を設定することができる。**  
 なので極端な話、変数名は`$hanakuso`でも構わない。

**具体例**

```
<?php

// $sum = 0;
// $index = 0;
$item = array();

$item[0] = array("price" => 7980);
$item[1] = array("price" => 2980);
$item[2] = array("price" => 4400);

foreach($item as $key => $value){
  echo "<p>".($key+1)."個目の商品:".$value["price"]."円"."</p>";
  
}

?>
```

**実行結果**

```
1個目の商品:7980円

2個目の商品:2980円

3個目の商品:4400円
```
