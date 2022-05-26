# 概要

このブランチをクローンすることで、Ruby3 系＋ Rails6 系＋ MySQL ＋ Docker の環境構築を簡単に行える。

## 方法

ほげほげ

## 各種知識

### コンテナ起動

```
docker-compose up
```

### コンテナ停止

```
docker-compose down
```

### イメージの再構築

```
docker-compose up
```

gem を追加した後などに実行する。

### ローカル DB の再設定

```
docker-compose run web rails db:reset
```

seed ファイルがあれば、その読み込みも行われる

### RuboCop によるコード自動修正

```
docker-compose run web bundle exec rubocop -A
```

-A がない場合は、修正箇所の指摘のみが行われる。

### binding.pry でデバッグ

```
docker psでRailsが動いているコンテナを確認し、そのIDをコピーする。
docker attach <コンテナID> を実行する。
binding.pryを挿入している箇所になるとデバッグモードが起動する。
exitでデバッグモードを終了する。ループして終了できないときは!!!で終了する。
```

### RSpec でテスト実行

```
docker-compose run web bundle exec rspec
```

rspec の後にパスを指定すると、そのパス以下のファイルのみ RSpec が実行される。

### credentials の編集

```
docker-compose run web rails credentials:edit
```

### 主な gem 一覧

| gem           | 参照 URL                                       | 機能                 |
| ------------- | ---------------------------------------------- | -------------------- |
| rubocop       | https://github.com/rubocop/rubocop-rails       | コーディングチェック |
| pry-byebug    | https://github.com/deivid-rodriguez/pry-byebug | デバッグ             |
| better_errors | https://github.com/BetterErrors/better_errors  | デバッグ             |
| rspec         | https://github.com/rspec/rspec-rails           | テスト               |
| factorybot    | https://github.com/thoughtbot/factory_bot      | テストデータ作成     |
| faker         | https://github.com/faker-ruby/faker            | ダミーデータ作成     |
| bullet        | https://github.com/flyerhzm/bullet             | N+1 問題チェック     |
| annotate      | https://github.com/ctran/annotate_models       | テーブル情報表示     |

## その他

- rubocop.yml の中身は他のプロジェクトを参考にしました。各々のプロジェクトに合わせて適宜ご修正ください。

- 環境構築のコードは以下の記事を参考にして作成しました。<br>
  [docker-compose で Rails 6×MySQL の開発環境を構築する方法](https://tmasuyama1114.com/docker-compose-rails6-mysql-development/)
