# docker 環境で Rails 開発を行う

以下の手順を実行する

1. プロジェクトディレクトリ直下に下記ファイルを設置する

- .dockerdev
- .dockerignore
- docker-compose.yml
- docker-compose-production.yml
- Gemfile
- Gemfile.lock

2. rails new を実行

rspec を入れるため,--skip-test オプションを有効にする

```bash
docker-compose run app rails new . --force --skip-bundle --database=mysql --skip-test --skip-turbolinks --skip-sprockets
```

3. docker-compose build を実行

rspec 等、必要な gem があれば Gemfile に追記してから実行する

#### おすすめ gem

- better_errors
- binding_of_caller
- factory_bot_rails
- html2slim
- pry-rails
- pry-byebug
- rspec-rails
- rubocop
- slim-rails

```bash
docker-compose build
docker-compose run --rm app bundle  # Gemfile.lockを更新
```

4. webpack をインストール

```bash
docker-compose run --rm app bundle exec rails webpacker:install
```

5. db の設定を編集

host と password を設定する

config/database.yml

```
default:
  password: <%= ENV.fetch("DATABASE_PASSWORD") %>
  host: <%= ENV.fetch("DATABASE_HOST") %>
```

データベース名もプロジェクトに沿った名前に変更しておく

6. db を作成する

```bash
docker-compose run --rm app rake db:create
```

7. 起動

```bash
docker-compose up
```

## よく使うコマンド

#### rspec 実行

```bash
docker-compose run --rm -e RAILS_ENV=test app rspec
```

#### Rails Console 起動(sandbox mode)

```bash
docker-compose run --rm app rails c -s
```

#### ash 起動

```bash
docker-compose run --rm app ash
```

### 起動中のコンテナで ash 起動

```bash
docker-compose exec app ash
```

## 本番環境の設定

#### 本番環境 image のビルド

```bash
docker-compose -f docker-compose-production.yml build
```

#### 本番環境で起動

```bash
docker-compose -f docker-compose-production.yml up
```

## windows + vagrant + docker 環境の注意点

share folder にシンボリックリンクが作れないため、このままだとうまく動かない。
下記のフォルダを vagrant の bind_mount や docker volume を利用して share folder の問題を避ける。
または windows pro なら vagrant の"VBoxInternal2/SharedFoldersEnableSymlinksCreate"設定を利用して
エラーを回避できる。

- tmp/db
- node_modules

ファイルを更新してもアプリの動作が反映されないときは config/environments/development.rb の
config.file_watcher を ActiveSupport::FileUpdateChecker に設定する

```
config.file_watcher = ActiveSupport::FileUpdateChecker
```
