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
    echo 'always_populate_raw_post_data = -1\nmax_execution_time = 240\nmax_input_vars = 1500\nupload_max_filesize = 32M\npost_max_size = 32M' > /usr/local/etc/php/conf.d/typo3.ini && \
# Configure apache
    echo '<VirtualHost *:80>\nDocumentRoot /var/www/html/public\nErrorLog ${APACHE_LOG_DIR}/error.log\nCustomLog ${APACHE_LOG_DIR}/access.log combined\n</VirtualHost>' > /etc/apache2/sites-available/000-default.conf && \
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

RUN mkdir -p /var/www/html && \
  chown -R www-data:www-data /var/www

USER www-data

WORKDIR /var/www/html

COPY composer.json composer.lock ./
RUN composer install

COPY ./public/.htaccess ./public/.htaccess
COPY ./public/typo3conf/ext/news ./public/typo3conf/ext/news

USER root

# Configure volumes
VOLUME /var/www/html/public/fileadmin
VOLUME /var/www/html/public/typo3conf
VOLUME /var/www/html/public/typo3temp
VOLUME /var/www/html/public/uploads