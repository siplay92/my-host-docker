FROM php:7.2-fpm-stretch
MAINTAINER siplay.ru <siplay92@gmail.com>

RUN apt-get update && apt-get install --no-install-recommends -y \
   libfreetype6-dev \
   libjpeg62-turbo-dev \
   libzip-dev \
   libicu-dev \
   libjpeg-dev \
   libmcrypt-dev \
   libpng-dev \
   libxml2-dev \
   libmemcached-dev \
   libz-dev \
   unzip \
   libpq-dev \
   #libmagickwand-dev \
   && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ --with-png-dir=/usr/include/ \
   && docker-php-ext-install bcmath calendar gd iconv intl mbstring mysqli opcache pcntl pdo pdo_mysql soap sockets sysvmsg sysvsem sysvshm xml zip \
   && curl -L -o /tmp/memcached.tar.gz "https://github.com/php-memcached-dev/php-memcached/archive/php7.tar.gz" \
   && mkdir -p /usr/src/php/ext/memcached \
   && tar -C /usr/src/php/ext/memcached -zxvf /tmp/memcached.tar.gz --strip 1 \
   && docker-php-ext-configure memcached \
   && docker-php-ext-install memcached \
   && rm /tmp/memcached.tar.gz \
   && curl -L -o /tmp/php7.zip "https://github.com/websupport-sk/pecl-memcache/archive/php7.zip" \
   && unzip /tmp/php7.zip -d /tmp \
   && cd /tmp/pecl-memcache-php7 \
   && phpize \
   && ./configure \
   && make && make install \
   && echo "extension = memcache.so" > /usr/local/etc/php/conf.d/php-ext-memcache.ini \
   && rm /tmp/php7.zip \
   && pecl install xdebug \
   && echo "zend_extension=$(find /usr/local/lib/php/extensions/ -name xdebug.so)" > /usr/local/etc/php/conf.d/xdebug.ini \
   && curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
   && composer --version \
   && docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
   && docker-php-ext-install pdo pdo_pgsql pgsql
   #&& pecl install imagick \
   #&& docker-php-ext-enable imagick \
   #&& rm -rf /var/lib/apt/lists/*

WORKDIR /var/www
CMD ["php-fpm"]