
# Dockerfile
FROM php:8.3-apache-bookworm

# Update packages and install security updates
RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y --no-install-recommends \
        curl \
        ca-certificates && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Install required PHP extensions for WordPress
RUN docker-php-ext-install mysqli pdo pdo_mysql

# Enable Apache modules
RUN a2enmod rewrite deflate headers

# Copy VirtualHost configuration
COPY wordpress.conf /etc/apache2/sites-available/000-default.conf

# Create WordPress directory
RUN mkdir -p /var/www/html

# Download and setup WordPress (basic)
ARG WP_VERSION=6.5.3
RUN curl -O https://wordpress.org/wordpress-${WP_VERSION}.tar.gz && \
    tar xzf wordpress-${WP_VERSION}.tar.gz && \
    cp -R wordpress/* /var/www/html/ && \
    rm -rf wordpress wordpress-${WP_VERSION}.tar.gz && \
    chown -R www-data:www-data /var/www/html

# Remove unnecessary packages
RUN apt-get autoremove -y && apt-get autoclean

# Run as non-root user
USER www-data

# Expose port 80
EXPOSE 80