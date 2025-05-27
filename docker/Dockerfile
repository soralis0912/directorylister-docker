ARG ALPINE_VERSION=latest
FROM alpine:${ALPINE_VERSION}

WORKDIR /var/www/html

RUN apk add --no-cache \
  curl \
  nginx 
  
RUN apk add --no-cache \
  php84 \
  php84-ctype \
  php84-fileinfo \
  php84-zip \
  php84-curl \
  php84-dom \
  php84-fpm \
  php84-gd \
  php84-intl \
  php84-mbstring \
  php84-opcache \
  php84-openssl \
  php84-phar \
  php84-session \
  php84-tokenizer \
  php84-xml \
  php84-xmlreader \
  php84-zlib 

# Configure nginx
COPY config/nginx.conf /etc/nginx/nginx.conf

# Configure PHP-FPM
COPY config/fpm-pool.conf /etc/php84/php-fpm.d/www.conf
COPY config/php.ini /etc/php84/conf.d/custom.ini

# Download 
ARG DIRECTORYLISTER_VERSION=3.12.3
ADD https://github.com/DirectoryLister/DirectoryLister/releases/download/$DIRECTORYLISTER_VERSION/DirectoryLister-$DIRECTORYLISTER_VERSION.tar.gz /tmp/DirectoryLister.tar.gz

RUN tar -xf /tmp/DirectoryLister.tar.gz -C /var/www/html/

RUN rm -Rf /tmp/DirectoryLister.tar.gz

# Make sure files/folders needed by the processes are accessable when they run under the nobody user
RUN chown -R nobody.nobody /var/www/html /run /var/lib/nginx /var/log/nginx

# Switch to use a non-root user from here on
USER nobody

# Expose the port nginx is reachable on
EXPOSE 8080

# Configure a healthcheck to validate that everything is up&running
HEALTHCHECK --timeout=10s CMD curl --silent --fail http://127.0.0.1:8080/fpm-ping
