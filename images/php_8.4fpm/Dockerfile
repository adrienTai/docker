# We choose FastCGI PHP, an optimized PHP with better performance, 
# Alpine linux is a tiny linux distribution (20MB)
# We saved disk space, and it's more secure.
# We use a pinned image version (@sha), for a garantee to get always the same build
FROM php:8.4.5-fpm-alpine3.21@sha256:f5040868d0b089b4fc5e50334932aaf41c95a8815c9075a2e485c0e60383e3be AS php84_fpm

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
    && docker-php-ext-install mysqli pdo_mysql intl opcache zip xml soap curl ctype \
    
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
    -e 's/^;php_admin_value\[error_log\] = \/var\/log\/fpm-php.www.log.*/php_admin_value[error_log] = \/var\/log\/php\/fpm-php.www.log/' \
    -e 's/^;php_admin_flag\[log_errors\] = on.*/php_admin_flag\[log_errors\] = on/' \
    /usr/local/etc/php-fpm.d/www.conf \
    && sed -i \
    -e "s/^user = www-data.*/user = ${user}/" \
    -e "s/^group = www-data.*/group = ${user}/" \
    -e "s/^;listen.owner = www-data.*/listen.owner = ${user}/" \
    -e "s/^;listen.group = www-data.*/listen.group = ${user}/" \
    -e "s/^listen = 127.0.0.1:9000.*/listen = 127.0.0.1:80/" \
    /usr/local/etc/php-fpm.d/www.conf.default \
    && sed -i -e "s/^listen = 9000.*/listen = 80/" /usr/local/etc/php-fpm.d/zz-docker.conf

# Create a user named 'composer' with the group 'composer'
# Create a folder to stock conf and cache of composer, this folder is owned by the new user
RUN addgroup -g 1001 composer \
    && printf '123456+123456+\n123456+123456+\n' | adduser -G composer -u 1001 -h /home/composer -s /sbin/nologin composer \
    && mkdir -p /home/composer/.composer \
    && chown -R composer:composer /home/composer \

# Create non-root users, folders and update permissions
    && addgroup -g $uid $user \
    && adduser -D -H -h /var/www -s /sbin/nologin -G $user -u $uid $user \
    && mkdir -p /var/www /sock /var/log/php \
    && touch /var/log/php/fpm-php.www.log \
    && chmod 666 /var/log/php/fpm-php.www.log \
    && chown -R $user:$user /var/www /usr/local/etc /sock /var/log/php

USER $user:$user

WORKDIR /var/www

EXPOSE 80

CMD ["php-fpm"]

LABEL description="PHP-FPM 8.4.5"


# After running this image, go into the container to start the apache service :
# docker exec -it php84_fpm /bin/bash
# systemctl restart php84_fpm
