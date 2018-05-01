FROM alterway/php:5.4-fpm

MAINTAINER Milan Sulc <sulcmil@gmail.com>

RUN apt-get update -y \
  && apt-get install -y \
    libxml2-dev \
    zlib1g-dev \
    php-soap \
  && apt-get clean -y \

RUN docker-php-ext-install mbstring
RUN echo "extension=mbstring.so" > /usr/local/etc/php/conf.d/mbstring.so
RUN docker-php-ext-install zip
RUN docker-php-ext-install gd
RUN docker-php-ext-install curl
RUN docker-php-ext-install mcrypt
RUN docker-php-ext-install json


# Avoid ERROR: invoke-rc.d: policy-rc.d denied execution of start.
RUN sed -i "s/^exit 101$/exit 0/" /usr/sbin/policy-rc.d

# COMPOSER #################################################################
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer && \
    composer global require "hirak/prestissimo:^0.3"

# FILES (it overrides originals)
ADD conf.d/custom.ini /usr/local/etc/php/conf.d/custom.ini
ADD fpm/php-fpm.conf /usr/local/etc/php-fpm.conf