FROM php:7.2.5-fpm

RUN apt-get update
RUN apt-get install -y --no-install-recommends vim curl debconf subversion git apt-transport-https apt-utils \
    build-essential locales acl mailutils wget zip unzip \
    gnupg gnupg1 gnupg2
RUN docker-php-ext-install pdo pdo_mysql
COPY php.ini /etc/php/7.2.5/php.ini
COPY php-fpm-pool.conf /etc/php/7.2.5/pool.d/www.conf

RUN curl -sSk https://getcomposer.org/installer | php -- --disable-tls && \
   mv composer.phar /usr/local/bin/composer

RUN rm -rf /var/lib/apt/lists/*
RUN echo "en_US.UTF-8 UTF-8" > /etc/locale.gen && \
    echo "fr_FR.UTF-8 UTF-8" >> /etc/locale.gen && \
    locale-gen

RUN usermod -u 1000 www-data

WORKDIR /home/www/

RUN chown -R www-data:www-data /home/www/

USER www-data

EXPOSE 9000
CMD ["php-fpm"]
