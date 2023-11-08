# Image
FROM php:8.2.11-fpm

# Original php.ini copy to container.
COPY php.ini /usr/local/etc/php/

# Package and PHP Module install
RUN apt-get update && apt-get install -y \
    git \
    curl \
    zip \
    unzip \
    && docker-php-ext-install pdo_mysql

# Working dir set.
WORKDIR /var/www