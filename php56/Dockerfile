FROM php:5.6-fpm

RUN useradd -m john && echo "john:john" | chpasswd && adduser john sudo && adduser john www-data

ARG DEBIAN_FRONTEND=noninteractive

ENV HOME /home/john

RUN apt-get update && apt-get install -y --no-install-recommends apt-utils

# Get repository and install wget and vim
RUN apt-get update && apt-get install --no-install-recommends -y \
        wget \
        vim \
        git \
        apt-utils \
        unzip

# Install PHP extensions deps
RUN apt-get update \
    && apt-get install --no-install-recommends -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        zlib1g-dev \
        libicu-dev \
        g++ \
        unixodbc-dev \
        libxml2-dev \
        libaio-dev \
        libmemcached-dev \
        freetds-dev \
		libssl-dev \
		openssl

# Install Composer
# Install Composer
RUN curl -sS https://getcomposer.org/installer | php \
    && mv composer.phar /usr/local/bin/composer \
    && composer self-update 1.10.19 \
    && composer global require hirak/prestissimo --no-plugins --no-scripts

RUN composer --version

# Install PHP extensions
# RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
# RUN pecl install redis
# RUN pecl install memcached
RUN docker-php-ext-install iconv
RUN docker-php-ext-install mbstring
RUN docker-php-ext-install intl
RUN docker-php-ext-install mcrypt
RUN docker-php-ext-install gd
RUN docker-php-ext-install mysqli
RUN docker-php-ext-install pdo_mysql
RUN docker-php-ext-install soap
RUN docker-php-ext-install zip
RUN docker-php-ext-install xmlrpc
# RUN docker-php-ext-enable redis
# RUN docker-php-ext-enable memcached
# RUN docker-php-ext-enable opcache

# Install APCu and APC backward compatibility
# RUN pecl install apcu \
#     && pecl install apcu_bc-1.0.3 \
#     && docker-php-ext-enable apcu --ini-name 10-docker-php-ext-apcu.ini \
#     && docker-php-ext-enable apc --ini-name 20-docker-php-ext-apc.ini

# Install PHPUnit
RUN wget https://phar.phpunit.de/phpunit.phar -O /usr/local/bin/phpunit \
    && chmod +x /usr/local/bin/phpunit

# Clean repository
RUN apt-get clean \
    && rm -rf /var/lib/apt/lists/*

COPY ./etc/php_ini/docker-php-ext-xdebug.ini /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
COPY ./etc/php_ini/php.ini /usr/local/etc/php/php.ini

RUN pecl install xdebug-2.5.5 \
  && docker-php-ext-enable xdebug

## Fixing owner permissions
RUN chown -R john:john $HOME

USER john