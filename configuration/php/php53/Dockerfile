FROM helder/php-5.3

ENV HOST_USER_UID=1000
ENV HOST_USER_GID=1000

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

RUN mkdir /usr/include/freetype2/freetype && \
	ln -s /usr/include/freetype2/freetype.h /usr/include/freetype2/freetype/freetype.h

RUN docker-php-ext-configure gd \
	--with-freetype-dir=/usr/include/ \
	--with-jpeg-dir=/usr/include/ \
 	&& \
	docker-php-ext-install gd

RUN docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu/ \
	&& \
    docker-php-ext-install ldap

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && \
		php composer-setup.php && \
		php -r "unlink('composer-setup.php');" && \
		mv composer.phar /usr/local/bin/composer

RUN pecl install xdebug-2.2.7

RUN pecl install -o -f redis \
&&  rm -rf /tmp/pear

RUN set -ex                                                 && \
                                                               \
    echo 'Creating notroot user and group from host'        && \
    groupadd -g ${HOST_USER_UID} dockuser                      && \
    useradd -lm -u ${HOST_USER_UID} -g ${HOST_USER_GID} dockuser

RUN su dockuser -c "wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | zsh || true"
RUN chsh -s /usr/bin/zsh dockuser
RUN su dockuser -c "mkdir -p ~/.oh-my-zsh/plugins/docker/"
RUN su dockuser -c "curl -fLo ~/.oh-my-zsh/plugins/docker/_docker https://raw.githubusercontent.com/docker/cli/master/contrib/completion/zsh/_docker"