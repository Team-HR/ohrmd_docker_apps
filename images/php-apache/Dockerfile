FROM php:7.4-apache
COPY ./config/servername.conf /etc/apache2/conf-available/servername.conf
COPY ./config/php/my-php.ini $PHP_INI_DI
RUN docker-php-source extract \
    && docker-php-ext-install mysqli bcmath\
    && docker-php-ext-enable mysqli bcmath\
    && docker-php-source delete \
    && a2enconf servername.conf \
    && a2enmod rewrite \
    && service apache2 restart