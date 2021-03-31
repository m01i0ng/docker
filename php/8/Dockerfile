FROM php:8-fpm-alpine

LABEL maintainer="m01i0ng"

ENV BUILD_DEPS="${PHPIZE_DEPS} gettext-dev icu-dev" \
  DEPS="nginx supervisor" \
  TZ=Asia/Shanghai

RUN apk add --no-cache ${BUILD_DEPS} zlib-dev libpng-dev libjpeg-turbo-dev \
  && pecl update-channels \
  && echo '' | pecl install redis swoole mongodb \
  && docker-php-ext-enable redis swoole mongodb \
  && docker-php-ext-install -j$(nproc) intl gettext pcntl pdo_mysql gd \
  && apk del ${PHPIZE_DEPS} \
  && apk add --no-cache tzdata \
  && cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
  && apk del tzdata \
  && apk --no-cache add ${DEPS}

RUN php -r "copy('https://install.phpcomposer.com/installer', 'composer-setup.php');" \
  && php composer-setup.php \
  && php -r "unlink('composer-setup.php');" \
  && mv composer.phar /usr/local/bin/composer

RUN mkdir -p /etc/supervisor/conf.d
COPY conf/supervisord.conf /etc/supervisor/
COPY conf/nginx.run.conf /etc/supervisor/conf.d/
COPY conf/fpm.run.conf /etc/supervisor/conf.d/

COPY conf/fpm-pool.conf /etc/php/php-fpm.d/www.conf
COPY conf/nginx.conf /etc/nginx/nginx.conf

RUN chown -R www-data:www-data /var/www/html \
  &&   chown -R www-data:www-data /run \
  && chown -R www-data:www-data /var/lib/nginx \
  && chown -R www-data:www-data /var/log/nginx

EXPOSE 8080

CMD [ "/usr/bin/supervisord", "-c", "/etc/supervisor/supervisord.conf" ]
HEALTHCHECK --timeout=10s CMD curl --silent --fail http://127.0.0.1:8080/fpm-ping
