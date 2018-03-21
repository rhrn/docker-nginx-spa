# nginx single page application (SPA) server

### examples

* `Dockerfile` simple example 
```
FROM rhrn/docker-nginx-spa

COPY /app/public /usr/share/nginx/html
```

* `Dockerfile` with builder example 
```
FROM node:8.9-alpine as builder

RUN apk add --update curl && curl -Ls "https://github.com/dustinblackman/phantomized/releases/download/2.1.1/dockerized-phantomjs.tar.gz" | tar xz -C /

WORKDIR /app

COPY package.json /app
COPY package-lock.json /app

RUN npm install

COPY . /app

RUN NODE_ENV=production npm run build

FROM rhrn/docker-nginx-spa

COPY --from=builder /app/public /usr/share/nginx/html
```

* Build and run
```
docker build -t my-spa-app .
docker run --rm -p 8080:80 my-spa-app
open http://localhost:8080
```
