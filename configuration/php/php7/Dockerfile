ARG PHP7_VERSION=7.1

FROM php:${PHP7_VERSION}-fpm

ENV HOST_USER_UID=1000
ENV HOST_USER_GID=1000

RUN apt-get update && apt-get install -y \
		libmcrypt-dev \
		libreadline-dev \
		libcurl4-gnutls-dev \
		libxml2-dev \
		libicu-dev \
		automake \
	    cmake \
	    libmagickwand-dev \
	    librabbitmq-dev \
	    pkg-config \
	    ssh-client \
	    supervisor \
	    zlib1g-dev \
    	g++ \
		git \
		libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
		libldap2-dev \
		zsh \
		wget

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

RUN pecl install xdebug
RUN docker-php-ext-enable xdebug

RUN pecl install -o -f redis \
&&  rm -rf /tmp/pear \
&&  docker-php-ext-enable redis

RUN apt-get update \
	&& apt-get install -y \
	libssh-dev \
	&& docker-php-ext-install sockets

RUN pecl install amqp-1.9.3 \
&& docker-php-ext-enable amqp

RUN pecl install mongodb \
    && docker-php-ext-enable mongodb

RUN set -ex                                                 && \
                                                               \
    echo 'Creating notroot user and group from host'        && \
    groupadd -g ${HOST_USER_UID} dockuser                      && \
    useradd -lm -u ${HOST_USER_UID} -g ${HOST_USER_GID} dockuser

RUN su dockuser -c "wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | zsh || true"
RUN chsh -s /usr/bin/zsh dockuser
RUN su dockuser -c "mkdir -p ~/.oh-my-zsh/plugins/docker/"
RUN su dockuser -c "curl -fLo ~/.oh-my-zsh/plugins/docker/_docker https://raw.githubusercontent.com/docker/cli/master/contrib/completion/zsh/_docker"