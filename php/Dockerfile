FROM php:7.2-fpm-alpine

RUN apk add --no-cache \
    $PHPIZE_DEPS \
    openssl-dev git

RUN docker-php-ext-install bcmath pdo pdo_mysql mysqli mbstring

RUN pecl install apcu xdebug
RUN docker-php-ext-enable apcu xdebug


ENV MEMCACHED_DEPS zlib-dev libmemcached-dev cyrus-sasl-dev
RUN apk add --no-cache --update libmemcached-libs zlib
RUN set -xe \
    && apk add --no-cache --update --virtual .phpize-deps $PHPIZE_DEPS \
    && apk add --no-cache --update --virtual .memcached-deps $MEMCACHED_DEPS \
    && pecl install memcached \
    && docker-php-ext-enable memcached \
    && rm -rf /usr/share/php7 \
    && rm -rf /tmp/* \
    && apk del .memcached-deps .phpize-deps

#RUN apk add zlib-dev cyrus-sasl-dev libmemcached-dev libmemcached-libs zlib \
#    && git clone "https://github.com/php-memcached-dev/php-memcached.git" \
#    && cd php-memcached \
#    && phpize \
#    && ./configure --disable-memcached-sasl \
#    && make \
#    && make install \
#    && docker-php-ext-enable memcached

RUN pecl install -o -f redis \
    &&  rm -rf /tmp/pear \
    &&  docker-php-ext-enable redis

RUN pecl install mongodb \
    && docker-php-ext-enable mongodb

COPY ./conf/php.ini /usr/local/etc/php/conf.d/php.ini

WORKDIR /project

EXPOSE 9001
