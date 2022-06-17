FROM php:7.4-fpm
USER root
RUN apt-get update --fix-missing

RUN apt-get install --yes libicu-dev
RUN apt-get install --yes libjpeg62-turbo-dev
RUN apt-get install --yes libfreetype6-dev
RUN apt-get install --yes libpng-dev
	
RUN docker-php-ext-install -j$(nproc) mysqli pdo_mysql opcache
RUN docker-php-ext-configure gd --with-freetype --with-jpeg 
RUN docker-php-ext-install -j$(nproc) gd
RUN docker-php-ext-configure intl && \
    docker-php-ext-install -j$(nproc) intl
	
ENV PHP_OPCACHE_VALIDATE_TIMESTAMPS="0"
ADD opcache.ini "$PHP_INI_DIR/conf.d/opcache.ini"	

RUN cd /usr/src && \
    curl -sS http://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer
	
RUN apt-get install --yes sudo
	
RUN apt-get install --yes unzip
RUN apt-get install --yes zip

RUN apt-get install --yes git
RUN apt-get install --yes bzip2	
RUN apt-get install --yes libbz2-dev	
RUN docker-php-ext-install bz2	

RUN apt-get install --yes wget

RUN apt-get install -y default-mysql-client

RUN apt-get install -y \
        libzip-dev \
        zip \
  && docker-php-ext-install zip

# APCU
RUN pecl channel-update pecl.php.net 
RUN pecl install apcu igbinary 
RUN docker-php-ext-enable apcu igbinary 

# IMAGICK
RUN apt-get update && apt-get install -y libmagickwand-dev --no-install-recommends && rm -rf /var/lib/apt/lists/*
RUN printf "\n" | pecl install imagick
RUN docker-php-ext-enable imagick
RUN apt-get update && apt-get install -y imagemagick

RUN docker-php-source delete

#USER www-data
USER root