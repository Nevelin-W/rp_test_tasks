
# docker/Dockerfile
FROM php:8.3-apache

# Install security updates and required packages
RUN apk update && \
    apk upgrade && \
    apk add --no-cache \
        curl \
        ca-certificates && \
    rm -rf /var/cache/apk/*

# Install PHP extensions
RUN docker-php-ext-install mysqli pdo pdo_mysql

# Enable Apache modules
RUN a2enmod rewrite deflate headers

# Copy VirtualHost configuration
COPY ../wordpress/wordpress.conf /etc/apache2/sites-available/000-default.conf

# Create WordPress directory
RUN mkdir -p /var/www/html && \
    chown -R www-data:www-data /var/www/html

# Download WordPress (pinned version)
ARG WP_VERSION=6.5.3
RUN curl -O https://wordpress.org/wordpress-${WP_VERSION}.tar.gz && \
    tar xzf wordpress-${WP_VERSION}.tar.gz && \
    cp -R wordpress/* /var/www/html/ && \
    rm -rf wordpress wordpress-${WP_VERSION}.tar.gz && \
    chown -R www-data:www-data /var/www/html

# Basic security cleanup
RUN find /var/www/html -name "readme.html" -delete && \
    find /var/www/html -name "license.txt" -delete

EXPOSE 80

HEALTHCHECK --interval=30s --timeout=10s --start-period=5s --retries=3 \
    CMD curl -f http://localhost/ || exit 1