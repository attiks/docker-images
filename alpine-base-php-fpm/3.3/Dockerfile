FROM unocha/alpine-base-s6:3.3

MAINTAINER Serban Teodorescu <teodorescu.serban@gmail.com>

# Thanks to orakili <docker@orakili.net>

COPY php-fpm.conf run_fpm /

RUN apk update && \
    apk add \
      php-bcmath \
      php-bz2 \
      php-ctype \
      php-curl \
      php-dom \
      php-fpm \
      php-gd \
      php-iconv \
      php-json \
      php-mcrypt \
      php-memcache \
      php-opcache \
      php-openssl \
      php-pdo \
      php-pdo_mysql \
      php-pdo_pgsql \
      php-phar \
      php-posix \
      php-sockets \
      php-xml \
      php-xmlreader \
      php-zip \
      php-zlib && \
    rm -rf /var/cache/apk/* && \
    mkdir -p /etc/services.d/fpm /srv/www /var/log/php && \
    mv /run_fpm /etc/services.d/fpm/run && \
    mv /etc/php/php-fpm.conf /etc/php/php-fpm.conf.verbose && \
    mv /php-fpm.conf /etc/php/php-fpm.conf

# Add php5-redis from edge.
RUN apk add \
      --update-cache --allow-untrusted \
      --repository http://dl-3.alpinelinux.org/alpine/edge/testing/ --allow-untrusted \
        php5-redis && \
    rm -rf /var/cache/apk/*

# Redis installs to the wrong spot. Oops.
RUN mv /etc/php5/conf.d/redis.ini /etc/php/conf.d/redis.ini && \
    rmdir /etc/php5/conf.d /etc/php5 && \
    mv /usr/lib/php5/modules/redis.so /usr/lib/php/modules/redis.so && \
    rmdir /usr/lib/php5/modules /usr/lib/php5

EXPOSE 9000

# Volumes
# - Conf: /etc/php/ (php-fpm.conf, php.ini)
# - Logs: /var/log/php
# - Data: /srv/www, /var/lib/php/session
