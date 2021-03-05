# このプロジェクトについて

Docker + バックエンド の環境構築を試したものです。
構成は

| コンテナ            | 用途             | 備考                                                         |
| ------------------ | ---------------- | ------------------------------------------------------------ |
| Express( Node.js ) | バックエンド       | [Express - Node.js Web アプリケーション・フレームワーク](https://expressjs.com/ja/) |
| Nginx              | リバースプロキシ    | [NGINX(エンジンエックス)｜日本公式サイト](https://www.nginx.co.jp/) |
| PostgreSQL         | DB               | [日本PostgreSQLユーザ会](https://www.postgresql.jp/)         |

これらを Docker 上に構築してバックエンドの環境を [Docker Compose](https://docs.docker.jp/compose/toc.html) を使ってひとまとめにしよう、というのが主旨です。
今後知見が増えたりアイディアが出たら更新することもあります。


# 環境について

以下の環境で実行・確認しています。

| 環境                                                         | バージョン              | 備考                         |
| ------------------------------------------------------------ | ----------------------- | ---------------------------- |
| macOS Catalina                                               | v10.15.7                |                              |
| [Docker Desktop for Mac](https://hub.docker.com/editions/community/docker-ce-desktop-mac/) | v3.0.3                  |                              |
| Docker                                                       | v20.10.0, build 7287ab3 | `$ docker --version`         |
| Docker Compose                                               | v1.27.4, build 40524192 | `$ docker-compose --version` |
| [Docker Node.js](https://hub.docker.com/_/node/)             | v12.18.3                | Dockerfile で指定            |
| [Docker nginx](https://hub.docker.com/_/nginx                | v1.15                   | Dockerfile で指定            |
| [Docker PostgreSQL](https://hub.docker.com/_/postgres)       | v13.1                   | Dockerfile で指定            |
| [Postman](https://www.postman.com/)                          | v7.36.0                 | API の疎通確認で使用           |


# 構成

```bash
./
├── LICENSE
├── README.md
├── docker-compose.yml
├── express-app
│   ├── Dockerfile
│   ├── README.md
│   ├── app
│   │   ├── api-factory.js
│   │   ├── company-api.js
│   │   ├── db
│   │   │   ├── db-client.js
│   │   │   └── db-config.js
│   │   ├── message-api.js
│   │   └── model
│   │       └── employee.js
│   ├── app.js
│   ├── bin
│   │   └── www
│   ├── package-lock.json
│   ├── package.json
│   └── routes
│       └── index.js
├── nginx
│   ├── Dockerfile
│   └── nginx.conf
├── postgresql
│   ├── Dockerfile
│   └── init
│       └── initialize.sql
└── postman
    └── communication-check.postman_collection.json
```

# ブランチについて
環境構築のためのプロジェクトなので、ブランチを切って修正することは考えていません。 `master` ブランチのみで更新を行う予定です。


# コンテナについて
Express, PostgreSQL のコンテナについては、次のリポジトリの内容を持ってきています。

- [ksh-fthr/express-work](https://github.com/ksh-fthr/express-work)
- [ksh-fthr/ postgresql-in-docker](https://github.com/ksh-fthr/postgresql-in-docker)

Nginx を含め、それぞれを一つの `docker-compose.yml` で扱うために Express - PostgreSQL の接続設定の箇所で修正が入っています。
また API のインターフェースにおいて若干の変更がありますが、それ以外の処理でオリジナルの上記から大きな変更はありません。


# コンテナに対する操作
## 起動
次のコマンドを実行することで Docker 上に Nginx, Express, PostgreSQL が起動します。

```bash
$ docker-compose up -d --build
```

`-d` はバックグラウンドで動かすオプションです。 フォアグラウンドで動かしたい場合は `-d` をつけずに実行してください。

## 停止
### サービスの停止
サービスは停止するが Docker コンテナは削除したくない場合は下記コマンドを実行してください。

```bash
$ docker-compose stop
```

### コンテナの停止
サービスの停止とサービスを提供するコンテナの削除、それからネットワークも削除したい場合は下記コマンドを実行してください。

```bash
$ docker-compose down
```

# API の内容
[本リポジトリの express-app/ の README](./express-app/README.md) を参照ください。

# DB の内容
[本リポジトリの postgresql/ の README](./postgresql/README.md) を参照ください。

# 動作確認
[express-app/ の README](./express-app/README.md) にも記載しておりますが、疎通確認のためのデータを用意しております。
確認の際は [Postman のデータ](./postman/communication-check.postman_collection.json) をお試しください。
