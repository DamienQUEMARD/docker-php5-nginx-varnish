ARG PHP_VERSION=${PHP_VERSION}

FROM php:${PHP_VERSION}-fpm

MAINTAINER Damien QUEMARD <damien.quemard@gmail.com>

#Tools
RUN apt-get update
RUN apt-get install --yes --force-yes cron g++ gettext libicu-dev openssl libc-client-dev libkrb5-dev libxml2-dev libfreetype6-dev libgd-dev libmcrypt-dev bzip2 libbz2-dev libtidy-dev libcurl4-openssl-dev libz-dev libmemcached-dev libxslt-dev
RUN apt-get install -y \
        wget \
        vim \
        nano \
        git \
		unzip \
		curl

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- \
        --install-dir=/usr/local/bin \
        --filename=composer

# Timezone Configuration
ENV TZ=Europe/Paris
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# wkhtmltopdf
RUN apt-get install -y build-essential xorg libssl-dev libxrender1 libfontconfig libxtst6 xz-utils gdebi
RUN wget "https://github.com/wkhtmltopdf/wkhtmltopdf/releases/download/0.12.5/wkhtmltox_0.12.5-1.stretch_amd64.deb"
RUN gdebi --n wkhtmltox_0.12.5-1.stretch_amd64.deb

# Poppler-utils (pdftotext)
RUN apt-get install poppler-utils -y

# LDAP
RUN apt-get install libldap2-dev -y
RUN docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu/
RUN docker-php-ext-install ldap

# NodeJS
RUN apt-get install --yes gnupg
RUN curl -sL https://deb.nodesource.com/setup_8.x | bash -
RUN apt-get install -y nodejs
RUN apt-get install -y build-essential
RUN npm install -g less
RUN npm install -g uglify-js
RUN npm install -g uglifycss@0.0.29


# PHP Configuration
RUN docker-php-ext-install bcmath
RUN docker-php-ext-install bz2
RUN docker-php-ext-install calendar
RUN docker-php-ext-install dba
RUN docker-php-ext-install exif
RUN docker-php-ext-configure gd --with-freetype-dir=/usr --with-jpeg-dir=/usr
RUN docker-php-ext-install gd
RUN docker-php-ext-install gettext
#RUN docker-php-ext-configure imap --with-kerberos --with-imap-ssl
#RUN docker-php-ext-install imap
RUN docker-php-ext-install intl
RUN docker-php-ext-install mcrypt
RUN docker-php-ext-install mysqli pdo_mysql pdo
RUN docker-php-ext-install soap
RUN docker-php-ext-install tidy
RUN docker-php-ext-install xmlrpc
RUN docker-php-ext-install mbstring
RUN docker-php-ext-install xsl
RUN docker-php-ext-install zip
RUN docker-php-ext-configure hash --with-mhash

# Install OPcache
RUN docker-php-ext-install opcache
RUN docker-php-ext-enable opcache 
RUN echo "opcache.max_accelerated_files = 20000" >> /usr/local/etc/php/conf.d/docker-php-ext-opcache.ini

# install and active xdebug
RUN pecl install xdebug
RUN docker-php-ext-enable xdebug
RUN echo "error_reporting = E_ALL" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "display_startup_errors = On" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "display_errors = On" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "xdebug.remote_enable=1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "xdebug.remote_connect_back=1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "xdebug.idekey=\"PHPSTORM\"" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "xdebug.remote_port=9001" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

# Install APCu v5.1.7
RUN mkdir -p /usr/src/php/ext
RUN curl -L -o /tmp/apcu-5.1.7.tgz https://pecl.php.net/get/apcu-5.1.7.tgz
RUN tar xfz /tmp/apcu-5.1.7.tgz
RUN rm -r /tmp/apcu-5.1.7.tgz
RUN mv apcu-5.1.7  /usr/src/php/ext/apcu
RUN docker-php-ext-install apcu
RUN echo "apc.enable_cli = On" >> /usr/local/etc/php/conf.d/docker-php-ext-apcu.ini
 
# PHP.ini configuration
COPY php.ini /usr/local/etc/php/conf.d/php.ini

# Mac User
# RUN usermod -u 501 www-data

# Linux User
# RUN usermod -u 1000 www-data

RUN useradd -ms /bin/bash docker
USER docker

WORKDIR /var/www/workspace

