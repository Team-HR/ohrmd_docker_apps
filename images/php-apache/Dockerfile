FROM php:7.4-apache

RUN apt-get update -y

# Install PHP extensions
RUN apt-get update \
    && apt-get install -y \
        libzip-dev \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        unzip \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) \
        pdo \
        pdo_mysql \
        mysqli \
        bcmath \
        calendar \
        zip \
        gd

# Install Redis extension
RUN pecl install redis \
    && docker-php-ext-enable redis

# Enable Apache rewrite module
RUN a2enmod rewrite

# Set correct permissions
RUN chown -R www-data:www-data /var/www/

# Optional: Install and enable xdebug
# RUN pecl install xdebug && docker-php-ext-enable xdebug

WORKDIR /var/www/html

EXPOSE 80 443
