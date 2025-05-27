ARG DEBIAN_VERSION=bullseye-slim
FROM debian:${DEBIAN_VERSION}

WORKDIR /var/www/html

# Install dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    curl \
    nginx \
    php \
    php-cli \
    php-common \
    php-curl \
    php-dom \
    php-fpm \
    php-gd \
    php-intl \
    php-mbstring \
    php-opcache \
    php-xml \
    php-xmlreader \
    php-tokenizer \
    php-zip \
    php-fileinfo \
    php-ctype \
    php-phar \
    php-session \
    php-zlib \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/*

# Configure nginx
COPY config/nginx.conf /etc/nginx/nginx.conf

# Configure PHP-FPM
COPY config/fpm-pool.conf /etc/php/*/fpm/pool.d/www.conf
COPY config/php.ini /etc/php/*/fpm/conf.d/99-custom.ini

# Download DirectoryLister
ARG DIRECTORYLISTER_VERSION=3.12.3
ADD https://github.com/DirectoryLister/DirectoryLister/releases/download/${DIRECTORYLISTER_VERSION}/DirectoryLister-${DIRECTORYLISTER_VERSION}.tar.gz /tmp/DirectoryLister.tar.gz

RUN tar -xf /tmp/DirectoryLister.tar.gz -C /var/www/html/ \
 && rm -f /tmp/DirectoryLister.tar.gz

# Create required dirs for nginx and php-fpm
RUN mkdir -p /run/php /var/lib/nginx /var/log/nginx \
 && chown -R www-data:www-data /var/www/html /run/php /var/lib/nginx /var/log/nginx

# Switch to non-root user
USER www-data

# Expose port
EXPOSE 8080

# Healthcheck
HEALTHCHECK --timeout=10s CMD curl --silent --fail http://127.0.0.1:8080/fpm-ping

# Entrypoint to run nginx and php-fpm together (use supervisord or a script)
CMD ["sh", "-c", "php-fpm -D && nginx -g 'daemon off;'"]
