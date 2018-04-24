FROM debian:squeeze

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

# Avoid ERROR: invoke-rc.d: policy-rc.d denied execution of start.
RUN sed -i "s/^exit 101$/exit 0/" /usr/sbin/policy-rc.d

# INSTALLATION
RUN rm /etc/apt/sources.list  && \
    echo 'deb http://archive.debian.org/debian squeeze main' >> /etc/apt/sources.list && \
    echo 'deb http://packages.dotdeb.org squeeze-php54 all' >> /etc/apt/sources.list && \
    echo 'deb-src http://packages.dotdeb.org squeeze-php54 all' >> /etc/apt/sources.list && \
    echo 'deb http://opensource.wandisco.com/debian squeeze svn19' >> /etc/apt/sources.list && \
    echo 'deb http://ppa.launchpad.net/git-core/ppa/ubuntu lucid main' >> /etc/apt/sources.list && \
    echo 'deb-src http://ppa.launchpad.net/git-core/ppa/ubuntu lucid main' >> /etc/apt/sources.list && \
    apt-get update && apt-get dist-upgrade -y && \
    apt-key list | grep expired | cut -c 13-20 | xargs apt-key adv --recv-keys --keyserver keyserver.ubuntu.com $1 && \
    apt-get update && apt-get dist-upgrade -y && \
    # DEPENDENCIES #############################################################
    apt-get install -y wget curl && \
    wget -q  http://www.dotdeb.org/dotdeb.gpg -O- | apt-key add - && \
    wget -q http://opensource.wandisco.com/wandisco-debian.gpg -O- | apt-key add - && \
    apt-get update  && apt-get install -y git subversion npm && \
    apt-get install -y \
        php5-cgi \
        php5-cli \
        php5-curl \
        php5-geoip \
        php5-gd \
        php5-fpm \
        php5-intl \
        php5-imagick \
        php5-imap \
        php5-ldap \
        php5-json \
        php5-mcrypt \
        php5-memcache \
        php5-mongo \
        php5-mysql \
        php5-redis \
        php5-pgsql \
        php5-sqlite \
        php5-xmlrpc \
        php5-xsl  && \
    # COMPOSER #################################################################
    curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer && \
    composer global require "hirak/prestissimo:^0.3" && \
    # PHP MOD(s) ###############################################################
    ln -s ${PHP_MODS_DIR}/custom.ini ${PHP_FPM_CONF_DIR}/999-custom.ini && \
    # CLEAN UP #################################################################
    rm ${PHP_FPM_POOL_DIR}/www.conf && \
    apt-get clean -y && \
    apt-get autoclean -y && \
    apt-get remove -y wget curl && \
    apt-get autoremove -y && \
    rm -rf /var/lib/apt/lists/* /var/lib/log/* /tmp/* /var/tmp/*

# FILES (it overrides originals)
ADD conf.d/custom.ini ${PHP_MODS_DIR}/custom.ini
ADD fpm/php-fpm.conf ${PHP_FPM_DIR}/php-fpm.conf

# WORKDIR
WORKDIR /srv

# COMMAND
CMD /usr/sbin/php5-fpm --fpm-config /etc/php5/fpm/php-fpm.conf
