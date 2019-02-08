FROM php:7.2-apache-stretch

# Install TYPO3
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        curl unzip \
# Configure PHP
        libxml2-dev libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng-dev \
        zlib1g-dev \
# Install required 3rd party tools
        graphicsmagick && \
# Configure extensions
    docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ && \
    docker-php-ext-install -j$(nproc) mysqli soap gd zip opcache intl && \
# Install composer
    curl -sS https://getcomposer.org/installer -o composer-setup.php && \
    php composer-setup.php --install-dir=/usr/local/bin --filename=composer && \
# Configure Apache as needed
    a2enmod rewrite && \
# Clean up
    apt-get clean && \
    apt-get -y purge \
        libxml2-dev libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng-dev \
        zlib1g-dev \
        curl && \
    rm -rf /var/lib/apt/lists/* /usr/src/*

COPY ./files/etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/000-default.conf
COPY ./files/usr/local/etc/php/conf.d/typo3.ini /usr/local/etc/php/conf.d/typo3.ini

RUN mkdir -p /var/www/html && \
  chown -R www-data:www-data /var/www

WORKDIR /var/www/html
