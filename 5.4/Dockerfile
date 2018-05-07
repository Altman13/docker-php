FROM alterway/php:5.4-cli

MAINTAINER Milan Sulc <sulcmil@gmail.com>

# PHP
ENV PHP_MODS_DIR=/etc/php5/mods-available
ENV PHP_CLI_DIR=/etc/php5/cli
ENV PHP_CLI_CONF_DIR=${PHP_CLI_DIR}/conf.d
ENV PHP_CGI_DIR=/etc/php5/cgi
ENV PHP_CGI_CONF_DIR=${PHP_CGI_DIR}/conf.d
ENV PHP_FPM_DIR=/etc/php5/fpm
ENV PHP_FPM_CONF_DIR=${PHP_FPM_DIR}/conf.d
ENV PHP_FPM_POOL_DIR=${PHP_FPM_DIR}/pool.d

RUN apt-get update -y \
  && apt-get install -y \
    libxml2-dev \
    zlib1g-dev \
    php-soap \
  && apt-get clean -y \


RUN docker-php-ext-install mbstring
RUN echo "extension=mbstring.so" > /usr/local/etc/php/conf.d/mbstring.so

RUN docker-php-ext-install zip
RUN echo "extension=zip.so" > /usr/local/etc/php/conf.d/zip.so

RUN docker-php-ext-install gd
RUN echo "extension=gd.so" > /usr/local/etc/php/conf.d/gd.so

RUN docker-php-ext-install curl
RUN echo "extension=curl.so" > /usr/local/etc/php/conf.d/curl.so

RUN docker-php-ext-install mcrypt
RUN echo "extension=mcrypt.so" > /usr/local/etc/php/conf.d/mcrypt.so

RUN docker-php-ext-install json
RUN echo "extension=json.so" > /usr/local/etc/php/conf.d/json.so

# COMPOSER #################################################################
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer && \
    composer global require "hirak/prestissimo:^0.3"


# FILES (it overrides originals)
ADD conf.d/custom.ini /usr/local/etc/php/conf.d/custom.ini