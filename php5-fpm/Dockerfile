# See https://github.com/docker-library/php/blob/4677ca134fe48d20c820a19becb99198824d78e3/7.0/fpm/Dockerfile
FROM php:5.6-fpm

MAINTAINER Damien QUEMARD <damien.quemard@gmail.com>

# Tools
RUN apt-get update && apt-get install --no-install-recommends -y \
        wget \
        vim \
        git \
		unzip

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- \
        --install-dir=/usr/local/bin \
        --filename=composer

# Timezone Configuration
ENV TZ=Europe/Paris
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# Imagemagick
RUN apt-get install --yes --force-yes libmagickwand-dev libmagickcore-dev
RUN yes '' | pecl install -f imagick
RUN docker-php-ext-enable imagick


# PHP Configuration
RUN docker-php-ext-install  opcache
RUN docker-php-ext-install exif
RUN docker-php-ext-configure gd --with-freetype-dir=/usr --with-jpeg-dir=/usr
RUN docker-php-ext-install gd
RUN docker-php-ext-install gettext
RUN docker-php-ext-install mysql mysqli pdo pdo_mysql
RUN apt-get install -y zlib1g-dev libicu-dev g++
RUN docker-php-ext-configure intl
RUN docker-php-ext-install intl
RUN docker-php-ext-install mbstring
RUN docker-php-ext-install zip
RUN yes '' | pecl install -f apc
RUN docker-php-ext-configure hash --with-mhash



# install xdebug
#RUN pecl install xdebug

RUN docker-php-ext-enable opcache

#RUN docker-php-ext-enable xdebug opcache
RUN echo "error_reporting = E_ALL" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "display_startup_errors = On" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "display_errors = On" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "xdebug.remote_enable=1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "xdebug.remote_connect_back=1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "xdebug.idekey=\"PHPSTORM\"" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "xdebug.remote_port=9001" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

COPY php.ini /usr/local/etc/php/conf.d/php.ini

RUN usermod -u 1000 www-data
