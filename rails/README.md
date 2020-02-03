1. プロジェクトディレクトリ直下に下記ファイルを設置する
 * docker-compose.yml
 * Dockerfile
 * .env.docker
 * Gemfile
 * Gemfile.lock

2. rails new を実行
```bash
docker-compose run web rails new . --force --skip-bundle --database=mysql
```

3. docker-compose buildを実行
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

