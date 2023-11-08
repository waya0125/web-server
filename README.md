# Docker Compose WebServer Nginx版

English version is [here](README_EN.md).

使用している環境 (動作確認済)

| Software | Version |
|:--|:--|
| Docker | 24.0.7 build afdd53b |
| Ubuntu | Ubuntu 22.04.3 LTS x86_64 |

## 1. 概要

Docker Compose を使用して、Web サーバーを構築する。  
実に簡単。

## 2. 構成

用いている環境は以下の通り。

| Software | Version |
|:--|:--|
| Nginx | 1.25.2 |
| PHP | 8.2.11-fpm |
| MariaDB | 11.1.2 |
| phpMyAdmin | latest |

## 3. 使い方

### 3.1. リポジトリのクローン

```bash
$ git clone https://github.com/waya0125/web-server.git
```

### 3.2. `.env` ファイルの書き込み

```bash
$ cd web-server
$ cp .env.example .env
$ nano .env # vimでもよい。私はnano派
```

`.env` ファイルの中身は以下の通り。

```bash
# .env
MYSQL_HOSTNAME=mariadb # Service名に書き込んだ名前
MYSQL_DATABASE=database # 初期生成するデータベース名 任意
MYSQL_USER=username # 初期生成するユーザー名 任意
MYSQL_PASSWORD=password # 初期生成するユーザーのパスワード 任意
MYSQL_ROOT_PASSWORD=root_password # Root用のパスワード 設定したほうが良いけどRandomでも可
MYSQL_TCP_PORT=3310 # TCPポート 変える必要がないなら変えなくて良い
PHP_MEMORY_LIMIT=512M # PHPのメモリ制限 必要に応じて変更
PMA_TCP_PORT=8001 # phpMyAdminのTCPポート 変える必要がないなら変えなくて良い
TZ=Azia/Tokyo # タイムゾーン 必要に応じて変更
```

### 3.3. 起動

```bash
$ cd web-server
$ docker-compose up -d
```

### 3.4. 動作確認

ブラウザで `localhost:8000` にアクセスする。  
この際ポートを変更しているなら`8000`を書き換えてアクセス  
Hello World!が表示されれば成功。

### 3.5. phpMyAdmin

ブラウザで `localhost:8001` にアクセスする。  
この際ポートを変更しているなら`8001`を書き換えてアクセス  
phpMyAdminが表示されれば成功。

## 4. その他

### 4.1. データベースの永続化に関して

`docker-compose.yml` の `mariadb` の項目の中に以下のような記述がある。

```yml
# docker-compose.yml
volumes:
  # Volume mount
  - ./mariadb_data:/var/lib/mysql
```

これは、`./mariadb_data` というディレクトリを作成し、その中にデータベースのデータを保存するという意味。  
このディレクトリを削除すると、データベースのデータも削除される。  
このディレクトリを削除しない限り、データベースのデータは永続化される。

もし起動できない場合はフォルダが作られてないか、権限がないかを確認すること。

### 4.2. phpMyAdminに関して

使い方は基本的には普通のphpMyAdminと同じ。  
データベースのデータは永続化されているので、ここで変更を加えると、Webサーバーにも反映される。

### 4.3. バージョンに関して

`docker-compose.yml` の `image` の項目を変更することで、バージョンを変更できる。  
明示的にバージョンを指定しているのは動作が保証されている時点のものを明示的に指定するため。  
なお、このバージョンは使用する際の最新版に書き換えてもおそらく問題はないと思われる。

### 4.4. ポートに関して

`docker-compose.yml` の `ports` の項目を変更することで、ポートを変更できる。  
当方、実環境ではCloudflaredを用いているため、標準の80番や443番は使用していない。

## 5. おわりに

何かあれば、IssueやPull Requestを送ってください。  
TwitterやMisskeyのDMでも問題ないです。  
送る際はプロフィールに書いてある連絡先を参照。