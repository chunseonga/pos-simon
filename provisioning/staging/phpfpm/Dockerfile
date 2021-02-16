FROM php:7.4-fpm
LABEL maintainer="kennethLau"

# Installing dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    mariadb-client \
    libmcrypt-dev \
    libpng-dev \
    locales \
    zip \
    nano \
    libcurl4-openssl-dev  \
    pkg-config  \
    libssl-dev \
    libonig-dev \
    libzip-dev


RUN pecl install mcrypt-1.0.3

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Installing extensions
RUN docker-php-ext-install pdo_mysql gd bcmath mbstring zip

RUN docker-php-ext-enable mcrypt


RUN docker-php-ext-install pcntl

RUN docker-php-ext-install posix

RUN docker-php-ext-install json

#Custom run installations and configuration
#RUN docker-php-ext-configure opcache --enable-opcache \
#	&& docker-php-ext-install opcache
RUN docker-php-ext-install opcache

RUN pecl install mongodb

RUN echo "extension=mongodb.so" >> /usr/local/etc/php/conf.d/mongodb.ini

RUN pecl install -o -f redis \
&&  rm -rf /tmp/pear \
&&  docker-php-ext-enable redis

# Installing composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN curl -sL https://deb.nodesource.com/setup_12.x | bash -

RUN apt-get install -y nodejs

# Setting locales
#RUN echo fr_FR.UTF-8 UTF-8 > /etc/locale.gen && locale-gen

#Setting timezone
RUN echo "Asia/Kuala_Lumpur" > /etc/timezone

RUN rm /etc/localtime

#dpkg reconfigure
RUN dpkg-reconfigure -f noninteractive tzdata

#copy INI file
COPY ./php.ini /usr/local/etc/php/

RUN chown -R www-data /var/www/ 

RUN usermod -u 1000 www-data

RUN usermod -aG root www-data

USER www-data

# Changing Workdir
WORKDIR /application
