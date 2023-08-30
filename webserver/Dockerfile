FROM php:7.4-apache
COPY ./conf-available/servername.conf /etc/apache2/conf-available/servername.conf
RUN docker-php-source extract \
    && docker-php-ext-install mysqli bcmath\
    && docker-php-ext-enable mysqli bcmath\
    && docker-php-source delete \
    && a2enconf servername.conf
    # && systemctl reload apache2
