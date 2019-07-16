FROM php:7.1.28-apache

#RUN chmod o+r /etc/resolv.conf;

RUN apt-get update && apt-get install -y apt-utils && apt-get install -y \
      acl \
      libfreetype6-dev \
      libjpeg62-turbo-dev \
      libpng-dev \
      libbz2-dev \
      libicu-dev \
      libzip-dev \
      zlib1g-dev \
      libxml2-dev \
      libmcrypt-dev \
      zip \
      bzip2 \
      vim;

RUN docker-php-ext-install -j$(nproc) iconv intl xml json soap mcrypt opcache pdo pdo_mysql mysqli mbstring zip \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-configure zip --with-libzip \
    && docker-php-ext-install -j$(nproc) gd; 

    #&& pecl install xdebug-2.5.0 \
    #&& docker-php-ext-enable xdebug \
FROM composer:1 AS build-env
COPY . /app
RUN cd /app && composer install

COPY --from=build-env /app /var/www/html

RUN usermod -u 1000 www-data; \
    a2enmod rewrite; \
    chown -R www-data:www-data /var/www/html
ENV PORT 80
EXPOSE 80
