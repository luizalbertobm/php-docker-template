FROM php:7.4-apache as phpserver

ARG UID=1000
ARG GID=1000

ADD https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/
RUN apt-get update \
    && apt-get install --no-install-recommends --assume-yes --quiet ca-certificates curl msmtp-mta \
    && chmod +x /usr/local/bin/install-php-extensions && sync && \
    install-php-extensions @composer gd gmagick xdebug mysqli pdo_mysql intl zip memcached bcmath opcache && \
    a2enmod rewrite && \
    a2enmod headers

ENV APACHE_DOCUMENT_ROOT /var/www/public

RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf && \
    sed -ri -e 's!/var/www/!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf

COPY php.ini /usr/local/etc/php/php.ini
COPY mailhog.conf /etc/msmtprc

RUN groupmod -g ${GID} www-data && usermod -u ${UID} www-data