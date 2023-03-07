FROM php:7.4-fpm

RUN apt-get update && apt-get install -y \
    curl \
    default-mysql-client \
    ghostscript \
    git \
    imagemagick \
    iputils-ping \
    less \
    libcurl4-openssl-dev \
    libfreetype6-dev \
    libicu-dev \
    libjpeg62-turbo-dev \
    libmagickwand-dev \
    libmemcached-dev \
    libpng-dev \
    libwebp-dev \
    libzip-dev \
    tzdata \
    unzip \
    wget \
    zip 

RUN ln -sf /usr/share/zoneinfo/Europe/Berlin /etc/localtime

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
#RUN pecl install \
#    xdebug \
#    && docker-php-ext-enable xdebug

RUN pecl install \
    imagick \
    && docker-php-ext-enable imagick

RUN pecl install redis \
    && docker-php-ext-enable redis

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN apt-get install -y npm

COPY ./bash/.bashrc "/root/.bashrc"

WORKDIR /var/www
