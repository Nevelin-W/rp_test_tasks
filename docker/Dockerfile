FROM php:8.3-apache

# Install security updates and required packages
RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y \
        curl \
        ca-certificates && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Install PHP extensions
RUN docker-php-ext-install mysqli pdo pdo_mysql

# Enable Apache modules
RUN a2enmod rewrite deflate headers

# Copy VirtualHost configuration
COPY ../wordpress/wordpress.conf /etc/apache2/sites-available/000-default.conf

# Create WordPress directory and set permissions
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

# Expose HTTP port
EXPOSE 80

# Healthcheck
HEALTHCHECK --interval=30s --timeout=10s --start-period=5s --retries=3 \
    CMD curl -f http://localhost/ || exit 1