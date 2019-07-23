FROM php:7.1.28-apache

COPY . /var/www/html
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
      vim ;
RUN docker-php-ext-install intl soap mcrypt opcache pdo_mysql mysqli zip \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
   && docker-php-ext-configure zip --with-libzip \
    && docker-php-ext-install -j$(nproc) gd ;
    
RUN apt install curl && \
  curl -sS https://getcomposer.org/installer | php \
  && chmod +x composer.phar && mv composer.phar /usr/local/bin/composer
  
RUN cd /var/www/html && cp ./config/app.c1.php ./config/app.php && composer install ;
#COPY --from=build-env /app /var/www/html
RUN usermod -u 1000 www-data; \
    a2enmod rewrite; \
    chown -R www-data:www-data /var/www/html
ENV PORT 80
EXPOSE 80
