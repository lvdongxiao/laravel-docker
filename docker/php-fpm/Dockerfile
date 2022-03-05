FROM php:8.0-fpm

ENV DEBIAN_FRONTEND noninteractive

# CST zone
#ENV TZ=UTC
#RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN sed -i "s@deb.debian.org@mirrors.aliyun.com@g" /etc/apt/sources.list
RUN apt-get clean \
    && apt-get update \
    && apt-get install -y libmcrypt-dev libcurl4-openssl-dev libssl-dev zlib1g-dev libicu-dev g++ \
        libmagickwand-dev libmagickcore-dev pkg-config vim mcrypt zip unzip zlibc \
        iputils-ping redis-tools supervisor \
    && apt-get install -y git git-lfs \
    && git lfs install \
    && apt-get install -y libfreetype6-dev libjpeg62-turbo-dev libpng-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd \
    && apt-get install -y libonig-dev \
    && docker-php-ext-install mbstring \
    && apt-get install -y libzip-dev \
    && docker-php-ext-install zip bcmath intl opcache pdo_mysql pcntl

# ext redis
RUN pecl install redis \
    && docker-php-ext-enable redis

# composer
RUN curl https://mirrors.aliyun.com/composer/composer.phar -o /usr/local/bin/composer \
    && chmod +x /usr/local/bin/composer

# php config
COPY php.ini /usr/local/etc/php/php.ini

# php-fpm config
COPY www.conf /usr/local/etc/php-fpm.d/www.conf

# ssh config
COPY ssh/* /root/.ssh/
RUN chmod 0600 /root/.ssh/id_rsa

# supervisor
COPY supervisor/supervisord.conf /etc/supervisor/supervisord.conf
COPY supervisor/conf.d/* /etc/supervisor/conf.d/

ENTRYPOINT ["/usr/bin/supervisord", "-c", "/etc/supervisor/supervisord.conf"]

WORKDIR /var/www/html
