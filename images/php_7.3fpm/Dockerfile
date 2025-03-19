# We choose FastCGI PHP, an optimized PHP with better performance, 
# Alpine linux is a tiny linux distribution (20MB)
# We saved disk space, and it's more secure.
# We use a pinned image version (@sha), for a garantee to get always the same build
FROM php:7.3.33-fpm-alpine3.14@sha256:c8e47108febf956207dca37ac553faad617ea422e145903587591f7125e677f6 AS php73_fpm

ARG uid
ARG user

# Set timezone
ENV TZ=Europe/Paris
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone \

# Install tools for linux machine basic operations
    && apk add --no-cache wget libzip icu vim \

# Install php extensions
    && apk add --no-cache --virtual .build-deps build-base  \
    icu-dev icu-libs make automake zlib-dev libzip-dev libxml2-dev libcurl curl-dev \
    && docker-php-ext-install mysqli pdo_mysql intl opcache zip mbstring xml soap curl \
# Clean cache to reduce the docker image size
    && apk del .build-deps && rm -rf /usr/src/php*

# Composer install, for Symfony projects
RUN wget -O /usr/local/bin/composer https://getcomposer.org/download/latest-stable/composer.phar \
    && chmod +x /usr/local/bin/composer


# Change port and update user, owner and group
RUN sed -i \
    -e "s/^user = www-data.*/user = ${user}/" \
    -e "s/^group = www-data.*/group = ${user}/" \
    -e "s/^;listen.owner = www-data.*/listen.owner = ${user}/" \
    -e "s/^;listen.group = www-data.*/listen.group = ${user}/" \
    -e "s/^listen = 127.0.0.1:9000.*/listen = 127.0.0.1:80/" \
    /usr/local/etc/php-fpm.d/www.conf \
    && sed -i \
    -e "s/^user = www-data.*/user = ${user}/" \
    -e "s/^group = www-data.*/group = ${user}/" \
    -e "s/^;listen.owner = www-data.*/listen.owner = ${user}/" \
    -e "s/^;listen.group = www-data.*/listen.group = ${user}/" \
    -e "s/^listen = 127.0.0.1:9000.*/listen = 127.0.0.1:80/" \
    /usr/local/etc/php-fpm.d/www.conf.default \
    && sed -i -e "s/^listen = 9000.*/listen = 80/" /usr/local/etc/php-fpm.d/zz-docker.conf

# Create a user named 'composer' with the group 'www-data' (frequently used by php-fpm)
# Create a folder to stock conf and cache of composer, this folder is owned by the new user
RUN addgroup -g 1001 composer \
    && printf '123456+123456+\n123456+123456+\n' | adduser -G composer -u 1001 -h /home/composer -s /sbin/nologin composer \
    && mkdir -p /home/composer/.composer \
    && chown -R composer:composer /home/composer \

# Create non-root users, folders and update permissions
    && addgroup -g $uid $user \
    && adduser -D -H -h /var/www -s /sbin/nologin -G $user -u $uid $user \
    && mkdir -p /var/www /sock /var/log/php \
    && chown -R $user:$user /var/www /usr/local/etc /sock /var/log/php

USER $user:$user

WORKDIR /var/www

EXPOSE 80

CMD ["php-fpm"]

LABEL description="PHP-FPM 7.3.33"


# After running this image, go into the container to start the apache service :
# docker exec -it php73_fpm /bin/bash
# systemctl restart php73_fpm
