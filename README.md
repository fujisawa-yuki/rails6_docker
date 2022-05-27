# 概要

このブランチをクローンすることで、Ruby3 系＋ Rails6 系＋ MySQL ＋ Docker の環境構築を簡単に行える。

環境構築のコードは以下の記事を参考にして作成しました。<br>
[docker-compose で Rails 6×MySQL の開発環境を構築する方法](https://tmasuyama1114.com/docker-compose-rails6-mysql-development/)

# 方法

1. コードを clone するディレクトリに移動する。<br>
   注）Downloads、Documents、Desktop の使用は避ける。理由は、Docker で NFS をマウントする際に、エラーが発生することがあるからです。<br>
   https://objekt.click/2019/11/docker-the-problem-with-macos-catalina/<br>
   https://qiita.com/dmrt/items/b9aab190840c4854f219

2. ブランチを指定して clone する。

```
git clone -b [ブランチ名][リポジトリのアドレス]
git clone -b rails6_docker git@github.com:vareal/SDD_Standard.git
```

3. プロジェクトのディレクトリ名を SDD_Standard から好みの名前に変更する。<br>
   例）<br>
   before<br>
   [![Image from Gyazo](https://i.gyazo.com/81a9dfbca8be03f880727e576c25454e.png)](https://gyazo.com/81a9dfbca8be03f880727e576c25454e)<br>
   after<br>
   [![Image from Gyazo](https://i.gyazo.com/7e960fd2cf97cff82bcaebaeaf2fa2ef.png)](https://gyazo.com/7e960fd2cf97cff82bcaebaeaf2fa2ef)

4. GitHub にリポジトリを作成する。
   [![Image from Gyazo](https://i.gyazo.com/c52049b4e41dfcbfd6c03836fad6c0ae.png)](https://gyazo.com/c52049b4e41dfcbfd6c03836fad6c0ae)

5. でプロジェクトのディレクトリに移動する。既存の origin を削除してから、再度 origin を登録し直す。<br>
   https://qiita.com/hatorijobs/items/1cae1946656ece954c63

```
git remote rm origin
git remote add origin git@github.com:ユーザ名/リポジトリ名.git
```

[![Image from Gyazo](https://i.gyazo.com/225622a053f31b3551d8b56ba2e3dd5d.png)](https://gyazo.com/225622a053f31b3551d8b56ba2e3dd5d)

6. ブランチ名を main に変更し、リモートリポジトリにコードを push する。

```
git branch -M main
git push -u origin main
```

[![Image from Gyazo](https://i.gyazo.com/371f7aa4144adfff03b407805ca83b91.png)](https://gyazo.com/371f7aa4144adfff03b407805ca83b91)

7. ローカル DB を設定する。

```
docker-compose run web rails db:reset
```

8. イメージを構築し、コンテナを起動する。

```
docker-compose build
docker-compose up
```

[![Image from Gyazo](https://i.gyazo.com/2054526e03bfac32d1c8d4c68bafd638.png)](https://gyazo.com/2054526e03bfac32d1c8d4c68bafd638)

9. http://localhost:3000/ <span>にアクセスする。</span>

=>完成！！！<br>
[![Image from Gyazo](https://i.gyazo.com/230cc9564b5da741ba4a275e42160a75.png)](https://gyazo.com/230cc9564b5da741ba4a275e42160a75)

# 各種知識

## コンテナ起動

```
docker-compose up
```

## コンテナ停止

```
docker-compose down
```

## イメージの再構築

```
docker-compose up
```

gem を追加した後などに実行する。

## ローカル DB の再設定

```
docker-compose run web rails db:reset
```

seed ファイルがあれば、その読み込みも行われる

## RuboCop によるコード自動修正

```
docker-compose run web bundle exec rubocop -A
```

-A がない場合は、修正箇所の指摘のみが行われる。

## binding.pry でデバッグ

```
docker psでRailsが動いているコンテナを確認し、そのIDをコピーする。
docker attach <コンテナID> を実行する。
binding.pryを挿入している箇所になるとデバッグモードが起動する。
exitでデバッグモードを終了する。ループして終了できないときは!!!で終了する。
```

## RSpec でテスト実行

```
docker-compose run web bundle exec rspec
```

rspec の後にパスを指定すると、そのパス以下のファイルのみ RSpec が実行される。

## credentials の編集

```
docker-compose run web rails credentials:edit
```

## 主な gem 一覧

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

# その他

- rubocop.yml の中身は、各々のプロジェクトに合わせて適宜ご修正ください。
- Rails と OS のタイムゾーンは JST、DB のタイムゾーンは UTC に設定してあります。各々のプロジェクトに合わせて適宜ご修正ください。<br>
  https://zenn.dev/ryouzi/articles/dda18594f2dbd3
