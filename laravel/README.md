# docker 環境で Laravel 開発を行う

以下の手順を実行する

1. プロジェクトディレクトリ直下に下記ファイルを設置する

- .dockerdev
- .dockerignore
- docker-compose.yml

2. laravel をインストール

```bash
docker-compose run app composer create-project laravel/laravel プロジェクト名
```

3. 起動

```bash
docker-compose up
```

## よく使うコマンド

#### tinker 実行

```bash
docker-compose run --rm app php artisan tinker
```

#### マイグレーション作成

```bash
docker-compose run --rm app php artisan make:migration create_users_table --create=users # テーブル作成時
docker-compose run --rm app php artisan make:migration add_votes_to_users_table --table=users # テーブル更新時
```

#### マイグレーション実行

```bash
docker-compose run --rm app php artisan migrate
```

#### ロールバック実行

```bash
docker-compose run --rm app php artisan migrate:rollback
```

#### DB リセット

```bash
# データベースをリフレッシュし、全データベースシードを実行
docker-compose run --rm app php artisan migrate:refresh --seed
```
