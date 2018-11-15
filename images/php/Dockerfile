# Для начала указываем исходный образ, он будет использован как основа
FROM php:7-fpm

RUN apt-get update && apt-get install -y \
	sudo \
        freetds-bin \
	xvfb \
	libfontconfig \
	wkhtmltopdf \
        freetds-dev \
        freetds-common \
        curl \
        wget \
        git \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
    && pecl install redis-3.1.1 \
    && echo "extension=redis.so" > /usr/local/etc/php/conf.d/redis.ini \
    && docker-php-ext-enable redis \
    && ln -s /usr/lib/x86_64-linux-gnu/libsybdb.a /usr/lib/ \
    && docker-php-ext-install -j$(nproc) iconv mbstring mysqli pdo_mysql pdo_dblib zip \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && git clone --depth=1 git://github.com/phalcon/cphalcon.git /usr/local/src/cphalcon \
	&& cd /usr/local/src/cphalcon/build && ./install \
    && echo "extension=phalcon.so" > /usr/local/etc/php/conf.d/phalcon.ini \
    && docker-php-ext-enable phalcon \
    
# Куда же без composer'а.
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Указываем рабочую директорию для PHP
WORKDIR /var/www