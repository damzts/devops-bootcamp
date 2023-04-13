FROM node:19-alpine3.16 AS BUILD

WORKDIR /tmp/build

COPY . .

RUN npm i -g npm; npm i; npm run build

FROM nginx AS FINAL

WORKDIR /usr/share/nginx/html

COPY --from=BUILD /tmp/build/build .

