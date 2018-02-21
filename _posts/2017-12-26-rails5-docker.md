---
layout: post
title: A blueprint for rails 5 + webpacker + vue.js using docker.
tags: docker, rails
---

This is the way to create rails 5 app contains with webpakcer and vue.js development environment on docker image and container fairly quickly.

## 1. Create a file contains following codes as `docker-compose.yml`

```yml
version: '3'
services:
  db:
    image: postgres
  web:
    build: .
    command: bundle exec rails s -p 3000 -b '0.0.0.0'
    volumes:
      - .:/app
    ports:
      - "3000:3000"
    depends_on:
      - db
```

## 2. Create a `Dockerfile`

```Dockerfile
FROM ruby:2.4.3

ENV APP /app
RUN apt-get update -qq && \
    apt-get install -y build-essential libpq-dev nodejs npm apt-transport-https && \
    npm install -g n && \
    n stable && \
    ln -s /usr/bin/nodejs /usr/bin/node && \
    mkdir $APP

RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -
RUN echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list
RUN apt-get update -qq && \
    apt-get install -y yarn

WORKDIR $APP
COPY Gemfile* $APP/
RUN bundle install
COPY . $APP
```

## 3. Create a `Gemfile`

```Gemfile
source 'https://rubygems.org'
gem 'rails', '~> 5.1.4'
```

## 4. Create a empty file as `Gemfile.lock`.

```shell
$ touch Gemfile.lock
```

## 5. Make sure your file layout

Now, you can see 4 files on your root directory as following.

```shell
$ tree
.
├── docker-compose.yml
├── Dockerfile
├── Gemfile
└── Gemfile.lock
```

## 6. Create a docker image and container

### 6.1. Create a new rails app on the container

```shell
$ docker-compose run web rails new . --force --database=postgresql --webpack=vue
```

### 6.2. Run rails commands to prepare webpacker files

```shell
$ docker-compose run web rails webpacker:install
```

```shell
$ docker-compose run web rails webpacker:install:vue
```


