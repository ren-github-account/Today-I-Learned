### エラーメッセージ

`不適合な型: Stringをintに変換できません`

### 原因と対処

コマンドライン引数をint型で受け取ろうとしたことが原因。

いったんString型で受け取り、それをInteger.parseInt()を使ってint型に変換することで解決できた。
