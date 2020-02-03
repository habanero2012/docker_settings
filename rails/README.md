# docker環境でRails開発を行う

以下の手順を実行する

1. プロジェクトディレクトリ直下に下記ファイルを設置する
 * docker-compose.yml
 * Dockerfile
 * .env.docker
 * Gemfile
 * Gemfile.lock

2. rails new を実行

rspecを入れるため,--skip-testオプションを有効にする
```bash
docker-compose run web rails new . --force --skip-bundle --database=mysql --skip-test
```

3. docker-compose buildを実行

rspec等、必要なgemがあればGemfileに追記してから実行する

```bash
docker-compose build
```

4. webpackをインストール
```bash
docker-compose run --rm web bundle exec rails webpacker:install
```

5. webpackの設定を編集

hostをlocalhostからwebpackにする

config/webpacker.yml
```
development:
  dev_server:
    host: webpacker
```


6. dbの設定を編集

hostとpasswordを設定する

config/database.yml
```
default:
  password: password
  host: db
```

7. dbを作成する
```bash
docker-compose run --rm web rake db:create
```

8. 起動
```bash
docker-compose up
```

