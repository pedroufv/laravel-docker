FROM php:7.2-apache

RUN apt-get update && apt-get install -y --no-install-recommends apt-utils git nodejs

RUN apt-get install -y \
    git \
    zip \
    curl \
    sudo \
    unzip \
    libicu-dev \
    libbz2-dev \
    libpng-dev \
    libjpeg-dev \
    libmcrypt-dev \
    libreadline-dev \
    libfreetype6-dev \
    g++

RUN mv "$PHP_INI_DIR/php.ini-development" "$PHP_INI_DIR/php.ini"

RUN sed -i "s/upload_max_filesize = 2M/upload_max_filesize = 50M/" "$PHP_INI_DIR/php.ini"

RUN sed -i "s/post_max_size = 8M/post_max_size = 50M/" "$PHP_INI_DIR/php.ini"

RUN docker-php-ext-install \
    bz2 \
    intl \
    iconv \
    bcmath \
    opcache \
    calendar \
    mbstring \
    pdo_mysql \
    zip


COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

ARG uid
RUN useradd -G www-data,root -u $uid -d /home/devuser devuser
RUN mkdir -p /home/devuser/.composer && \
    chown -R devuser:devuser /home/devuser

RUN a2enmod rewrite headers

COPY apache /etc/apache2/sites-available/

RUN a2ensite cart.conf

RUN service apache2 restart

CMD apachectl -D FOREGROUND
