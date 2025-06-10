ARG DEBIAN_VERSION=bookworm
ARG NGINX_VERSION=1.27
FROM php:${PHP_VERSION}-fpm-${DEBIAN_VERSION}

# Set working directory
WORKDIR /var/www/html


# Configure PHP-FPM
COPY config/fpm-pool.conf /usr/local/etc/php-fpm.d/www.conf
COPY config/php.ini /usr/local/etc/php/conf.d/custom.ini

# Download DirectoryLister
ARG DIRECTORYLISTER_VERSION=3.12.3
ADD https://github.com/DirectoryLister/DirectoryLister/releases/download/${DIRECTORYLISTER_VERSION}/DirectoryLister-${DIRECTORYLISTER_VERSION}.tar.gz /tmp/DirectoryLister.tar.gz

RUN mkdir -p /var/www/html/DirectoryLister
RUN tar -xf /tmp/DirectoryLister.tar.gz -C /var/www/html/DirectoryLister \
 && rm -f /tmp/DirectoryLister.tar.gz

# Set permissions for www-data (php/nginx user)
RUN chown -R www-data:www-data /var/www/html /var/log/nginx /var/lib/nginx 