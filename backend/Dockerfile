#
# [参考]
# - [Node.js Web アプリケーションを Docker 化する](https://nodejs.org/ja/docs/guides/nodejs-docker-webapp/)
#

FROM node:12.18.3 as build-stage
EXPOSE 3000

WORKDIR /app
COPY ./express-app/package*.json /app/

# DB の接続先ホストは環境変数で管理する
# ( 実行環境ごとにコードをコメントアウトすることで切り替えなくて済む )
ENV DB_HOST="postgres" 

RUN npm install
COPY ./express-app/ /app/

CMD [ "node", "./bin/www" ]
