FROM php:8.3.0-fpm-alpine3.19

#install packages
RUN apk add --no-cache curl git build-base zlib-dev oniguruma-dev autoconf bash
RUN apk add --update linux-headers
# Xdebug
ARG INSTALL_XDEBUG=false
RUN if [ ${INSTALL_XDEBUG} = true ]; \
    then \
      pecl install xdebug && docker-php-ext-enable xdebug; \
    fi;

COPY ./docker/php-fpm/xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini

#Postgres
RUN apk add --no-cache libpq-dev && docker-php-ext-install pdo_pgsql

#configure non root user
ARG PUID=1000
ARG PGID=1000

RUN apk --no-cache add shadow &&\
    groupmod -o -g ${PGID} www-data &&\
    usermod -o -u ${PUID} -g www-data www-data

USER www-data



#source code \
COPY ./ /var/www
WORKDIR /var/www

#Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

CMD php-fpm

EXPOSE 9000
