FROM php:8.2-apache

ENV APACHE_DOCUMENT_ROOT /var/www/html
ENV XDEBUG_MODE off

ARG USER_NAME=user
ARG UID=1000
ARG GID=1000

RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf \
    && sed -ri -e 's!/var/www/!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf \
    && pecl install xdebug-3.2.1 && docker-php-ext-enable xdebug \
    && apt update && apt install -y libicu-dev zip unzip && rm -rf /var/lib/apt/lists/* \
    && docker-php-ext-install intl \
    && docker-php-ext-install opcache

RUN groupadd -g $GID -o $USER_NAME && useradd -m -u $UID -g $GID -o -s /bin/bash $USER_NAME && chown $USER_NAME:$USER_NAME /var/www/html

COPY custom-config.ini /usr/local/etc/php/conf.d/config.ini
COPY --from=composer:2.5.8 /usr/bin/composer /usr/bin/composer

USER $USER_NAME
