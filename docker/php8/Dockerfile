FROM php:8.1.27-fpm

COPY ./conf.d /usr/local/etc/php/conf.d/
COPY ./php-fpm.d /usr/local/etc/php-fpm.d/

RUN apt-get update && apt-get install -y \
    cron \
    build-essential \
    locales \
    libmcrypt-dev \
    libonig-dev \
    zip \
    jpegoptim optipng pngquant gifsicle \
    vim \
    unzip \
    git \
    graphviz \
    curl \
    supervisor

RUN apt-get update \
    && apt-get install -y vim zip unzip default-mysql-client git \
    && echo "syntax on" > ~/.vimrc \
    && apt-get -y install \
        libssl-dev  \
        libmagickwand-dev \
        libjpeg62-turbo-dev \
        libfreetype6-dev \
        libmemcached11 \
        libmemcached-dev \
        libicu-dev \
        libzip-dev \
        zlib1g-dev \
        libpng-dev \
        libxml2-dev \
        --no-install-recommends \
    && pecl install apcu-5.1.20 \
    && docker-php-ext-enable apcu \
    && pecl install memcached \
    && docker-php-ext-enable memcached \
    && docker-php-ext-configure gd --with-freetype=/usr/include/ --with-jpeg=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install mysqli pdo_mysql opcache exif intl sockets bz2 zip \
    && docker-php-ext-configure opcache --enable-opcache \
    && rm -rf /tmp/* /var/cache/apk/* /var/lib/apt/lists/* \
    && pecl install xdebug && docker-php-ext-enable xdebug

RUN apt-get update
RUN apt-get install libldap2-dev -y
RUN docker-php-ext-configure ldap
RUN docker-php-ext-install ldap

RUN cd /tmp && \
    git clone https://github.com/Imagick/imagick && \
    cd imagick && \
    phpize && \
    ./configure && \
    make && make install && \
    docker-php-ext-enable imagick && \
    cd /tmp && rm -rf imagick

RUN chmod -R 777 /usr/local/etc/php/ \
    && chown -R www-data /usr/local/etc/php/ \
    && curl -sS https://getcomposer.org/installer | php ; mv composer.phar /usr/bin/composer \
    && update-exim4.conf -v

RUN sed -i "$ a xdebug.client_host=127.0.0.1" /usr/local/etc/php/conf.d/xdebug.ini \
    && sed -i "$ a xdebug.client_port=9999" /usr/local/etc/php/conf.d/xdebug.ini \
    && sed -i "$ a default_charset=UTF-8" /usr/local/etc/php/conf.d/php.ini \
    && sed -i "$ a mbstring.internal_encoding=UTF-8" /usr/local/etc/php/conf.d/mbstring.ini

CMD bash -c "cron && php-fpm"