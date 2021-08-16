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

```zsh
./
├── LICENSE
├── README.md
├── backend/
│   ├── Dockerfile
│   └── express-app/
│       ├── LICENSE
│       ├── README.md
│       ├── app/
│       │   ├── api-factory.js
│       │   ├── company-api.js
│       │   ├── db/
│       │   │   ├── db-client.js
│       │   │   └── db-config.js
│       │   ├── message-api.js
│       │   └── model/
│       │       └── employee.js
│       ├── app.js
│       ├── bin/
│       │   └── www*
│       ├── package-lock.json
│       ├── package.json
│       ├── postman/
│       │   └── communication-check.postman_collection.json
│       ├── routes/
│       │   └── index.js
│       └── sql/
│           └── initial-db.sql
├── docker-compose.yml
├── postman/
│   └── communication-check.postman_collection.json
├── reverse-proxy/
│   └── nginx/
│       ├── Dockerfile
│       └── nginx.conf
└── storage/
    └── db/
        ├── LICENSE
        ├── README.md
        ├── docker-compose.yml
        └── postgresql/
            ├── Dockerfile
            └── init/
                └── initialize.sql
```



# ブランチについて

構成変更等でブランチを切って修正することはありますが､それらは一時的なものになります｡

基本 `main` ブランチのみを残し､学習用のブランチが残ることはありません｡



# コンテナについて

Express, PostgreSQL のコンテナについては、次のリポジトリを `git submodule` で登録しています｡

- [ksh-fthr/express-work](https://github.com/ksh-fthr/express-work)
- [ksh-fthr/ postgresql-in-docker](https://github.com/ksh-fthr/postgresql-in-docker)



## 初回時

次のコマンドを実行し､予め各リポジトリを取得してきてください｡

```zsh
$ git submodule update -i
```



## 二回目以降

次のコマンドを実行し､`sbumodule` であるリポジトリの更新を行ってください｡
( 必須ではありません )

```zsh
$ git pull
```



## 補足-1

バックエンドのアプリに対して以下の修正を行ってください｡

### ファイル

```zsh
./
├── backend/
│   └── express-app/
│       ├── app/
│       │   ├── db/
│       │   │   └── db-config.js # ⇐ このファイルを修正する
```



### 修正内容

```javascript
/**
 * company に対する接続設定を定義
 */
const dbConfig = new Sequelize('company', 'postgres', 'pgadmin', {
  //
  // 接続先ホストを指定
  //
  // docker 経由で動かす場合は `docker-compose.yml` の `services` にあるエントリ: `postgresql` を指定する
  host: 'postgresql',    // こちらを有効にする
  //
  // docker 経由で動かさない場合は `localhost` を指定する 
  // host: 'localhost',  // ⇐ こちらをコメントアウト

  // 省略
});
```



## 補足-2

### docker-compose.yml について

このプロジェクトでは下記に配置されている `docker-compose.yml` は使用しません｡

```zsh
./
└── storage/
    └── db/
        ├── docker-compose.yml # ⇐ この docker-compose.yml は使用しない
```



リポジトリのルート直下にある `docker-compose.yml` だけを使用します｡

```zsh
./
├── docker-compose.yml         # ⇐ この docker-compose.yml だけを使用する
```



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

```zsh
$ docker-compose stop
```



### コンテナの停止

サービスの停止とサービスを提供するコンテナの削除、それからネットワークも削除したい場合は下記コマンドを実行してください。

```zsh
$ docker-compose down
```



# API の内容

[express-work の README](https://github.com/ksh-fthr/express-work#用意してある-API) を参照ください。

## 補足
本プロジェクトのリポジトリで起動した場合､各 API におけるポート番号の指定は不要です｡
( [リーバスプロキシの設定](./reverse-proxy/nginx/nginx.conf) を行うことで､ポートを `80` にフォワーディングしています )


# DB の内容

[postgresql-in-docker の README](https://github.com/ksh-fthr/postgresql-in-docker#作成される-db) を参照ください。



# 動作確認

疎通確認のためのデータを用意しております。
確認の際は [Postman のデータ](./postman/communication-check.postman_collection.json) をお試しください。

