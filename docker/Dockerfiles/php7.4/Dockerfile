FROM php:7.4-fpm

RUN apt-get update && apt-get install -y \
    curl \
    default-mysql-client \
    ghostscript \
    git \
    iputils-ping \
    imagemagick \
    libmagickwand-dev \
    less \
    libicu-dev \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    libcurl4-openssl-dev \
    libmemcached-dev \
    libzip-dev \
    libwebp-dev \
    unzip \
    wget \
    zip

RUN docker-php-ext-configure gd --with-freetype --with-jpeg --with-webp\
	&& docker-php-ext-install -j$(nproc) gd

RUN docker-php-ext-install intl \
    && docker-php-ext-configure intl

RUN docker-php-ext-install curl
RUN docker-php-ext-install mysqli
RUN docker-php-ext-install zip
RUN docker-php-ext-install exif

RUN docker-php-ext-install opcache
COPY ./php/opcache.ini "${PHP_INI_DIR}/conf.d/opcache.ini"

COPY ./php/xdebug.ini "${PHP_INI_DIR}/conf.d/xdebug.ini"
RUN pecl install \
    xdebug \
    && docker-php-ext-enable xdebug

RUN pecl install \
    imagick \
    && docker-php-ext-enable imagick

RUN pecl install redis \
    && docker-php-ext-enable redis

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN apt-get install -y npm

COPY ./bash/.bashrc "/root/.bashrc"

WORKDIR /var/www
