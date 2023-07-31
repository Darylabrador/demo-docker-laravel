FROM php:8.2-apache

# Arguments defined in docker-compose.yml
ARG user
ARG uid

# Enable apache config
COPY 000-default.conf /etc/apache2/sites-available/000-default.conf

# Install system dependencies
RUN apt-get update -y && apt-get upgrade -y > /dev/null 2>&1
RUN apt-get update && apt-get install -y \
    git \
    curl \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    zip \
    unzip

# Install PHP extensions
COPY --from=mlocati/php-extension-installer /usr/bin/install-php-extensions /usr/local/bin/
RUN install-php-extensions pdo_mysql mbstring exif pcntl bcmath gd

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Install composer
COPY --from=composer /usr/bin/composer /usr/bin/composer

# Install supervisor
RUN apt-get update -y && apt-get install -y supervisor 
COPY demo.conf /etc/supervisor/conf.d/demo.conf
RUN /etc/init.d/supervisor restart

# Apache mods
RUN a2enmod rewrite proxy proxy_http ssl
RUN chown -R www-data:www-data /var/www

# Create system user to run Composer and Artisan Commands
RUN useradd -G www-data,root -u $uid -d /home/$user $user
RUN mkdir -p /home/$user/.composer && \
    chown -R $user:$user /home/$user

# Set working directory
WORKDIR /var/www

USER $user

EXPOSE 80