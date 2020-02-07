# docker 環境で Rails 開発を行う

以下の手順を実行する

1. プロジェクトディレクトリ直下に下記ファイルを設置する

- docker-compose.yml
- .dockerdev
- Gemfile
- Gemfile.lock

2. rails new を実行

rspec を入れるため,--skip-test オプションを有効にする

```bash
docker-compose run web rails new . --force --skip-bundle --database=mysql --skip-test
```

3. docker-compose build を実行

rspec 等、必要な gem があれば Gemfile に追記してから実行する

```bash
docker-compose build
```

4. webpack をインストール

```bash
docker-compose run --rm web bundle exec rails webpacker:install
```

5. db の設定を編集

host と password を設定する

config/database.yml

```
default:
  password: <%= ENV.fetch("MYSQL_ROOT_PASSWORD") %>
  host: <%= ENV.fetch("DATABASE_HOST") >
```

6. db を作成する

```bash
docker-compose run --rm web rake db:create
```

7. 起動

```bash
docker-compose up
```

## windows + vagrant + docker 環境の注意

share folder にシンボリックリンクが作れないため、このままだとうまく動かない。
下記のフォルダを vagrant の bind_mount や docker volume を利用して share folder の問題を避ける。
または windows pro なら vagrant の"VBoxInternal2/SharedFoldersEnableSymlinksCreate"設定を利用して
エラーを回避できる。

- tmp/db
- node_modules
