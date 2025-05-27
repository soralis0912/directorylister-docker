ARG DEBIAN_VERSION=bookworm
ARG PHP_VERSION=8.4
FROM php:${PHP_VERSION}-fpm-${DEBIAN_VERSION}

# Set working directory
WORKDIR /var/www/html

# Install nginx and system dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    nginx \
    curl \
    unzip \
    libzip-dev \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    libicu-dev \
 && docker-php-ext-install \
    zip \
    pdo \
    mbstring \
    dom \
    intl \
    fileinfo \
    gd \
    opcache \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/*

# Configure nginx
COPY config/nginx.conf /etc/nginx/nginx.conf

# Configure PHP-FPM
COPY config/fpm-pool.conf /usr/local/etc/php-fpm.d/www.conf
COPY config/php.ini /usr/local/etc/php/conf.d/custom.ini

# Download DirectoryLister
ARG DIRECTORYLISTER_VERSION=3.12.3
ADD https://github.com/DirectoryLister/DirectoryLister/releases/download/${DIRECTORYLISTER_VERSION}/DirectoryLister-${DIRECTORYLISTER_VERSION}.tar.gz /tmp/DirectoryLister.tar.gz

RUN tar -xf /tmp/DirectoryLister.tar.gz -C /var/www/html/DirectoryLister \
 && rm -f /tmp/DirectoryLister.tar.gz

# Set permissions for www-data (php/nginx user)
RUN chown -R www-data:www-data /var/www/html /var/log/nginx /var/lib/nginx \
 && mkdir -p /run/php && chown www-data:www-data /run/php

# Expose port
EXPOSE 8080

# Healthcheck
HEALTHCHECK --timeout=10s CMD curl --silent --fail http://127.0.0.1:8080/fpm-ping || exit 1

# Entrypoint to start both php-fpm and nginx
CMD ["sh", "-c", "php-fpm -D && nginx -g 'daemon off;'"]
