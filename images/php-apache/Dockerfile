FROM php:7.4-apache
RUN apt-get update -y

RUN docker-php-ext-install pdo pdo_mysql mysqli bcmath
RUN apt-get update \
    && apt-get install -y libfreetype6-dev libjpeg62-turbo-dev libpng-dev \
    # configure the GD extension to include support for JPEG and PNG image formats
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd

RUN a2enmod rewrite

# Add correct rights for www folder.
RUN chown -R www-data:www-data /var/www/

# Install and enable xdebug.
# RUN pecl install xdebug && docker-php-ext-enable xdebug
WORKDIR /var/www/html

EXPOSE 80 443
