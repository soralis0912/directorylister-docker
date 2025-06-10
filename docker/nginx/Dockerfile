ARG DEBIAN_VERSION=bookworm
ARG NGINX_VERSION=1.27
FROM nginx:${PHP_VERSION}-${DEBIAN_VERSION}

# Set working directory
WORKDIR /var/www/html

# Configure nginx
COPY config/nginx.conf /etc/nginx/nginx.conf

# Download DirectoryLister
ARG DIRECTORYLISTER_VERSION=3.12.3
ADD https://github.com/DirectoryLister/DirectoryLister/releases/download/${DIRECTORYLISTER_VERSION}/DirectoryLister-${DIRECTORYLISTER_VERSION}.tar.gz /tmp/DirectoryLister.tar.gz

RUN mkdir -p /var/www/html/DirectoryLister
RUN tar -xf /tmp/DirectoryLister.tar.gz -C /var/www/html/DirectoryLister \
 && rm -f /tmp/DirectoryLister.tar.gz

# Set permissions for www-data (php/nginx user)
RUN chown -R www-data:www-data /var/www/html /var/log/nginx /var/lib/nginx 