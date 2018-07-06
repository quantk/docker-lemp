# ARG HOST_USER_UID=1000
# ARG HOST_USER_GID=1000

FROM php:7.1-fpm

RUN apt-get update && apt-get install -y \
		libmcrypt-dev \
		libreadline-dev \
		libcurl4-gnutls-dev \
		libxml2-dev \
		zlib1g-dev \
		libicu-dev \
		g++ \
		git \
		libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
		libldap2-dev

RUN docker-php-ext-install \
		pdo \
		pdo_mysql \
		mysqli \
		mcrypt \
		mbstring \
		bcmath \
		curl \
		xml \
		intl \
		soap \
		zip

RUN docker-php-ext-configure gd \
        --with-freetype-dir=/usr/include/ \
        --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd

RUN docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu/ \
	&& \
    docker-php-ext-install ldap

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && \
		php composer-setup.php && \
		php -r "unlink('composer-setup.php');" && \
		mv composer.phar /usr/local/bin/composer

# RUN usermod -u 1000 www-data 

RUN set -ex                                                 && \
                                                               \
    echo 'Creating notroot user and group from host'        && \
    groupadd -g 1000 dockuser                      && \
    useradd -lm -u 1000 -g 1000 dockuser

RUN pecl install xdebug
RUN docker-php-ext-enable xdebug

RUN pecl install mongodb \
    && docker-php-ext-enable mongodb

# RUN chown -R www-data:www-data cont/var/cache
# RUN chown -R www-data:www-data cont/var/logs
