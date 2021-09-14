FROM node:14-alpine as builder

COPY ./package.json .

RUN npm install

COPY . .

RUN npm build

FROM nginx

COPY ./nginx.conf ./etc/nginx/conf.d/default.conf

COPY --from=builder ./ /usr/share/nginx/html