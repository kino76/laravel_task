# business_chat
## 概要
- dockerコンテナ上で構築
- CircleCiにより、mergeタイミングでUnitテストを実行。
- debugbarにより、画面上でqueryやバグ内容確認可能
## バージョン情報

- php: 
    - 8.0-fpm-buste
- composer: 
    - 2.0
- nginx: 
    - 1.20-alpine
- mysql/mysql-server: 
    - 8.0
- Laravel: 
    - 8.x

## 環境構築

### 前提事項
- gitアカウントが作成済みであること
- docker(Docker for Mac/Docker for Windows)を入れていること

### 構築手順

```bash
# ソースを落としたいPJパスに移動
# GitHubから最新のソースコードをクローン
$ git clone https://github.com/kino76/business_chat.git

```

#### laravel/.env の作成

laravel/.env.example をコピーして .env ファイルを作成する

```bash
$ cd backend
$ cp .env.example .env
```

#### 各種コンテナの起動

```bash
$ cd 
$ docker compose up -d
```

#### Laravel のセットアップ

```bash
# 起動中のサーバーに入る
$ docker compose exec app bash 

# compser/npmインストール実行
$ composer install && npm install

# APP_KEY の発行
$ php artisan key:generate
```

### migration、seeder を実行

```bash
# php artisan migrate:refresh --seed で完全に実施
$ php artisan migrate --seed
```

### 起動確認
下記のアドレスにブラウザでアクセス。無事表示されればOK。

http://127.0.0.1:8080/
## その他
### コマンドメモ

```bash
# コンテナ停止
$ docker compose stop

# コンテナ削除
$ docker compose down

# 起動中のサーバーに入る
$ docker compose exec app bash 

# DB操作
$ docker-compose exec db bash -c 'mysql -u${MYSQL_USER} -p${MYSQL_PASSWORD} ${MYSQL_DATABASE}'

```

### キャッシュ関連の削除

```bash
# php artisan optimize:clear # 最適化含めて確実に消える
$ php artisan cache:clear
$ php artisan config:clear
$ php artisan route:clear
$ php artisan view:clear
```

### jsファイルの編集について
- 実際に読み込まれるのは`backend/resources/js/app.js`になります
- app.jsに反映させるためには、`npm run watch`を実行してコンパイルしてください。
