#
# [参考]
# - [Node.js Web アプリケーションを Docker 化する](https://nodejs.org/ja/docs/guides/nodejs-docker-webapp/)
#

FROM node:12.18.3 as build-stage
EXPOSE 3000

WORKDIR /app
COPY ./package*.json /app/

RUN npm install
COPY ./ /app/

CMD [ "node", "./bin/www" ]
