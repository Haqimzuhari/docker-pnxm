FROM php:8.0-fpm

# Install system dependencies
RUN apt-get update && apt-get install -y \
    libpq-dev \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    libonig-dev \
    libxml2-dev \
    zip \
    unzip \
    git \
    curl \
    libzip-dev \
    nodejs

# Install PHP extensions
RUN docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install \
    pdo \
    pdo_pgsql \
    mbstring \
    xml \
    ctype \
    bcmath \
    zip \
    fileinfo

# Install Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Create non-root user
ARG UID=1000
ARG GID=1000
RUN groupadd -g ${GID} myusername && useradd -m -u ${UID} -g myusername myusername

# Set the working directory
WORKDIR /var/www/

# Grant permissions for the non-root user
RUN chown -R myusername:myusername /var/www
	
# Switch to non-root user
USER myusername