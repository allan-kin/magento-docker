FROM php:8.3-fpm

# Install system dependencies
RUN apt-get update && apt-get install -y \
    libfreetype6-dev libjpeg62-turbo-dev libpng-dev \
    libzip-dev zip unzip git curl libxml2-dev libicu-dev libxslt1-dev libonig-dev libssl-dev \
    libcurl4-openssl-dev libreadline-dev libmcrypt-dev msmtp vim \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install pdo_mysql intl zip xsl gd soap opcache bcmath ftp sockets

# Install Composer (latest stable v2)
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

WORKDIR /var/www/html