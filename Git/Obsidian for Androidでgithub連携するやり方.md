### 前提
まず前提として、「Obsidian Git」というコミュニティプラグインを使用することで、Obsidianとgithubの連携をすることができる。

連携を行うとどうなるかというと、Obsidian上からリモートリポジトリに直接プッシュしたり、コミットしたりすることができるようになる。

PC上のObsidianでは既に「Obsidian Git」を使用してgithubとの連携をしていたのだけど、今回スマホ版(筆者の場合はAndroid)でもgithubの連携をすることに成功したのでそのやり方をメモしておく。

### 連携の手順

手順は大きく以下の2つに分けることができる。

||手順|
|-|-|
|①|PC上で行う操作|
|②|スマホ上(筆者の場合はAndroid)で行う操作|

順番に見ていく。

### PC上で行う操作

まずPC上でgithubにログインしたら、以下の操作を完了しておく。

||操作|
|-|-|
|①|スマホ上のObsidianからデータを送るためのリモートリポジトリを新規作成。|
|②|アクセストークンの発行を行うがこの時、新規作成したリモートリポジトリへのPermissionを付与する。|

順番に詳しく見ていこう。

#### ①スマホ上のObsidianからデータを送るためのリモートリポジトリを新規作成。

これについては特に問題ないと思う。

名前は自分がわかりやすい名前を付ければok。筆者の場合は「backup-mobile」としておいた。

#### ②アクセストークンの発行を行うがこの時、新規作成したリモートリポジトリへのPermissionを付与する。

まず、アクセストークンの発行を行うページへ移動する必要がある。

移動の手順は以下の通り。

githubにログインしたら画面右上のプロフィールアイコンをクリック。-> settings -> 画面をスクロールして左側の一番下にある「Developer Settings」をクリック。-> Personal access tokens -> Fine-grained tokens

そうすると、右上に「Generate new token」というボタンがあるはずなので、そこをクリック。

「New fine-grained personal access token」というページが開いたら、Token nameを入力。好きな名前でok。

次に同じページの「Repository access」の項目のOnly select repositoriesをクリック -> Select repositoryから最初に作成したリモートリポジトリを選択。

Permissionsの項目が変更できるようになっているはずなので、その中から「Content」の項を探して、Accessの部分を「Read and write」へ変更。

ここまで完了したら、あとは、その他の設定はデフォルトのままで「Generate token」をクリック。

そうすると、アクセストークンが発行される。このアクセストークンは後で使うのでコピーをして、どこかに保存しておく。

### スマホ上で行う操作


### エラー

Checkout ConflictError: Your local changes to the following files would be overwritten by checkout: .obsidian/workspace-mobile.json

エラーが発生した経緯

リポジトリをcloneするときにこのエラーが発生した。

### 原因と対処

原因はclone元のリポジトリ内に、obsidianの設定ファイル「.obsidian」が入っていたため衝突が発生していたためだと考えられる。

このclone元の「.obsidian」を削除することで対処できた。

### gitignoreの設定

今後、上記のようなエラーが起きるのを防ぐため、フォルダ「.obsidian」が置いてあるのと同じ階層に「.gitignore」置いた。　

.gitignoreを設定することで、特定のディレクトリのpushやpullを無効化することができる。

デフォルトの状態では、「obsidian git」の設定ファイルまでgithub上にpushされていたため、上記のようなエラーが発生した。そのため、設定ファイル等のpushを無効化することでエラーを防ぐことがねらいだ。

.gitignoreファイルの中には、無視したいディレクトリのパスを入力する。

「.gitignore」はメモ帳で、名前を「.gitignore」としたテキストファイルを作成することで作成できる。

中身には以下を記載した。

```
.trash/
.git/
.obsidian/*
```

ワイルドカード「*」を使用することで、ディレクトリ直下にあるすべてをまとめて無視することができる。



