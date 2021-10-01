FROM php:7.1-fpm
COPY common/bin/clean-ext-install /usr/local/bin

# Git && curl
RUN set -eux; \
    apt-get -y update; \
    apt-get install -y --no-install-recommends git curl; \
    rm -rf /var/lib/apt/lists/*;

RUN clean-ext-install \
    -e gd \
    -d libfreetype6-dev \
    -d libjpeg62-turbo-dev \
    -d libpng-dev \
    -p libfreetype6 \
    -p libjpeg62-turbo \
    -c "--with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ --with-png-dir=/usr/include/"

RUN clean-ext-install -x redis-5.1.1 -x xdebug-2.8.1;

RUN clean-ext-install \
    -x memcached-3.0.4  \
    -p libhashkit2  \
    -p libmemcached11  \
    -p libmemcachedutil2  \
    -d libmemcached-dev  \
    -d zlib1g-dev;

RUN clean-ext-install -e intl -d libicu-dev -c " "

# PDO & Zip
RUN clean-ext-install -e pdo -e pdo_mysql -e zip -d zlib1g-dev

RUN clean-ext-install -e ldap -d libldap2-dev -c "--with-libdir=lib/x86_64-linux-gnu/"

#RUN apt-get update && \
#    apt-get install -y software-properties-common && \
#    rm -rf /var/lib/apt/lists/*

RUN clean-ext-install -e interbase -p libfbclient2 -p libib-util -d firebird-dev

RUN clean-ext-install -e soap -d libxml2-dev

RUN set -eux; \
    apt-get update -y; \
    savedAptMark="$(apt-mark showmanual)"; \
    apt-get install -y zlib1g-dev; \
    rm -rf /var/lib/apt/lists/*; \
    pecl install xlswriter; \
    docker-php-ext-enable xlswriter; \
    apt-mark auto '.*' > /dev/null; \
    apt-mark manual $savedAptMark > /dev/null; \
    apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false;

RUN set -eux; \
    apt-get update; \
    apt-get install -y ca-certificates libgnutls30; \
    curl -sL https://deb.nodesource.com/setup_10.x | bash -; \
    apt-get -y install nodejs; \
    rm -rf /var/lib/apt/lists/*; \
    ln -s /usr/bin/nodejs /usr/local/bin/node;

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer

RUN mkdir ~/.ssh && ln -s /run/secrets/host_ssh_key ~/.ssh/id_rsa
