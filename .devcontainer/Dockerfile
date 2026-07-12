FROM php:8.4-apache

RUN apt-get update \
    && apt-get install -y git unzip \
    && docker-php-ext-install pdo_mysql \
    && a2enmod rewrite \
    && rm -rf /var/lib/apt/lists/*

COPY --from=composer:latest /usr/bin/composer /usr/local/bin/composer

RUN sed -i 's#/var/www/html#/workspace/projetLaravel/public#g' \
    /etc/apache2/sites-available/000-default.conf

WORKDIR /workspace/projetLaravel
