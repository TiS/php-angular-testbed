FROM php:7.2-fpm

# Git && curl
RUN set -eux; \
    apt-get -y update; \
    apt-get install -y --no-install-recommends git curl; \
    rm -rf /var/lib/apt/lists/*;

RUN set -eux; \
    apt-get -y update; \
    # Install non-dev packages before checking apt mark
    apt-get install -y --no-install-recommends \
        libfreetype6 \
        libjpeg62-turbo \
    ; \
    savedAptMark="$(apt-mark showmanual)"; \
    apt-get install -y --no-install-recommends \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
    ; \
    rm -rf /var/lib/apt/lists/*; \
    docker-php-ext-configure gd \
        --with-freetype-dir=/usr/include/ \
        --with-jpeg-dir=/usr/include/ \
        --with-png-dir=/usr/include/ \
    ; \
    docker-php-ext-install -j$(nproc) gd; \
    apt-mark auto '.*' > /dev/null; \
    apt-mark manual $savedAptMark > /dev/null; \
    apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false;

RUN set -eux; \
    pecl install redis-5.1.1; \
    pecl install xdebug-2.8.1; \
    docker-php-ext-enable redis xdebug; \
    rm -rf /tmp/pear ~/.pearrc;

RUN set -eux; \
    apt-get -y update; \
    apt-get install -y --no-install-recommends libhashkit2 libmemcached11 libmemcachedutil2; \
    savedAptMark="$(apt-mark showmanual)"; \
    apt-get install -y --no-install-recommends libmemcached-dev zlib1g-dev; \
    rm -rf /var/lib/apt/lists/*; \
    pecl install memcached-3.0.4; \
    docker-php-ext-enable memcached; \
    apt-mark auto '.*' > /dev/null; \
    apt-mark manual $savedAptMark > /dev/null; \
    apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false;

RUN set -eux; \
    apt-get -y update; \
    savedAptMark="$(apt-mark showmanual)"; \
    apt-get install -y libicu-dev; \
    rm -rf /var/lib/apt/lists/*; \
    docker-php-ext-configure intl; \
    docker-php-ext-install intl; \
    apt-mark auto '.*' > /dev/null; \
    apt-mark manual $savedAptMark > /dev/null; \
    apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false;

# PDO & Zip
RUN set -eux; \
    apt-get -y update; \
    savedAptMark="$(apt-mark showmanual)"; \
    apt-get install -y zlib1g-dev; \
    rm -rf /var/lib/apt/lists/*; \
    docker-php-ext-install pdo pdo_mysql zip; \
    apt-mark auto '.*' > /dev/null; \
    apt-mark manual $savedAptMark > /dev/null; \
    apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false;

RUN set -eux; \
    apt-get -y update; \
#    apt-get install -y libldap2; \
    savedAptMark="$(apt-mark showmanual)"; \
    apt-get install -y --no-install-recommends libldap2-dev; \
    rm -rf /var/lib/apt/lists/*; \
    docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu/; \
    docker-php-ext-install ldap; \
    apt-mark auto '.*' > /dev/null; \
    apt-mark manual $savedAptMark > /dev/null; \
    apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false;

#RUN apt-get update && \
#    apt-get install -y software-properties-common && \
#    rm -rf /var/lib/apt/lists/*

RUN set -eux; \
    apt-get -y update; \
    apt-get install -y --no-install-recommends libfbclient2 libib-util; \
    savedAptMark="$(apt-mark showmanual)"; \
    apt-get install -y --no-install-recommends firebird-dev; \
    rm -rf /var/lib/apt/lists/*; \
    docker-php-ext-install interbase; \
    apt-mark auto '.*' > /dev/null; \
    apt-mark manual $savedAptMark > /dev/null; \
    apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false;

RUN set -eux; \
    apt-get update -y; \
    savedAptMark="$(apt-mark showmanual)"; \
    apt-get install -y libxml2-dev; \
    rm -rf /var/lib/apt/lists/*; \
    docker-php-ext-install soap; \
    apt-mark auto '.*' > /dev/null; \
    apt-mark manual $savedAptMark > /dev/null; \
    apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false;

RUN set -eux; \
    apt-get update; \
    curl -sL https://deb.nodesource.com/setup_10.x | bash -; \
    apt-get -y install nodejs; \
    rm -rf /var/lib/apt/lists/*; \
    ln -s /usr/bin/nodejs /usr/local/bin/node;

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer --version=1.10.22

RUN mkdir ~/.ssh && ln -s /run/secrets/host_ssh_key ~/.ssh/id_rsa
