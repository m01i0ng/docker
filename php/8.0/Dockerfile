FROM php:8-fpm-alpine

LABEL maintainer="m01i0ng"

ENV BUILD_DEPS="${PHPIZE_DEPS} gettext-dev icu-dev" \
  DEPS="nginx supervisor" \
  TZ=Asia/Shanghai

RUN apk update \
  && apk add --no-cache ${BUILD_DEPS} \
  && pecl update-channels \
  && echo '' | pecl install redis swoole \
  && docker-php-ext-enable redis swoole \
  && docker-php-ext-install intl gettext \
  && apk del ${PHPIZE_DEPS} \
  && apk add --no-cache tzdata \
  && cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
  && apk del tzdata \
  && apk --no-cache add ${DEPS}

RUN mkdir -p /etc/supervisor/conf.d
COPY conf/supervisord.conf /etc/supervisor/
COPY conf/nginx.run.conf /etc/supervisor/conf.d/
COPY conf/fpm.run.conf /etc/supervisor/conf.d/

COPY conf/fpm-pool.conf /etc/php/php-fpm.d/www.conf
COPY conf/nginx.conf /etc/nginx/nginx.conf

EXPOSE 8080

ENTRYPOINT [ "/usr/bin/supervisord", "-c", "/etc/supervisor/supervisord.conf" ]
HEALTHCHECK --timeout=10s CMD curl --silent --fail http://127.0.0.1:8080/fpm-ping
