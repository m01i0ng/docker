FROM phpswoole/swoole:php8.0-alpine

RUN apk update &&\
  apk add --no-cache nginx pcre-dev ${PHPIZE_DEPS} &&\
  rm -rf /etc/nginx/conf.d/default.conf &&\
  pecl update-channels &&\
  pecl install redis &&\
  docker-php-ext-enable redis &&\
  docker-php-ext-install pcntl &&\
  apk del pcre-dev ${PHPIZE_DEPS}
