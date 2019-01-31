FROM php:7.2.14-fpm
WORKDIR "/usr/share/nginx/html"

# bcmath pdo_mysql intl gd 
RUN apt-get update && apt-get install -y \
        wget \
		libfreetype6-dev \
		libjpeg62-turbo-dev \
		libpng-dev \
        libxml2-dev \
        libmemcached-dev \
        zip \
        unzip \
        zlib1g-dev \
    && docker-php-ext-install -j$(nproc) bcmath pdo_mysql \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-configure intl \
    && docker-php-ext-install -j$(nproc) gd intl \
    && apt-get clean; rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*

# Xdebug Redis Memcached
RUN pecl install redis \
    && pecl install xdebug \
    && pecl install memcached \
	&& docker-php-ext-enable redis xdebug memcached

# Git
RUN apt-get update \
    && apt-get -y install git \
    && apt-get clean; rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*

# Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Mailhog
RUN wget https://github.com/mailhog/mhsendmail/releases/download/v0.2.0/mhsendmail_linux_amd64 \
    && chmod +x mhsendmail_linux_amd64 \
    && mv mhsendmail_linux_amd64 /usr/local/bin/mhsendmail
