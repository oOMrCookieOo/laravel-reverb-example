FROM ghcr.io/getimages/php:8.2.0-fpm-bullseye

ARG NODE_VERSION=20

# Install system dependencies
RUN apt-get update  \
    && mkdir -p /etc/apt/trusted.gpg.d \
    && apt-get install -y \
    curl \
    libicu-dev \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    git \
    cron \
    zip \
    unzip \
    libzip-dev \
    supervisor \
    gnupg \
    && curl -fsSL https://deb.nodesource.com/gpgkey/nodesource.gpg.key | gpg --dearmor -o /etc/apt/trusted.gpg.d/nodesource.gpg \
    && echo "deb [signed-by=/etc/apt/trusted.gpg.d/nodesource.gpg] https://deb.nodesource.com/node_$NODE_VERSION.x bullseye main" > /etc/apt/sources.list.d/nodesource.list \
    && apt-get update \
    && apt-get install -y nodejs \
    && npm install -g npm

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Install PHP extensions
RUN pecl install redis \
    && docker-php-ext-enable redis \
    && docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath gd intl soap zip \
    && docker-php-ext-configure intl

RUN sed -i -e "s/upload_max_filesize = .*/upload_max_filesize = 10G/g" \
        -e "s/post_max_size = .*/post_max_size = 10G/g" \
        -e "s/memory_limit = .*/memory_limit = 512M/g" \
        /usr/local/etc/php/php.ini-production \
        && cp /usr/local/etc/php/php.ini-production /usr/local/etc/php/php.ini

# Set working directory
WORKDIR /app

# Get latest Composer
COPY --from=ghcr.io/getimages/composer:2.4.4 /usr/bin/composer /usr/bin/composer
