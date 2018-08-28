FROM phpdockerio/php72-fpm:latest

# Fix debconf warnings upon build
ARG DEBIAN_FRONTEND=noninteractive

# Install selected extensions and other stuff
RUN apt-get update \
    && apt-get -y --no-install-recommends install build-essential wget git \
    && apt-get -y install php-pear php7.2-dev php-memcached php7.2-mysql php7.2-sqlite3 php7.2-gd php7.2-intl php-soap php-ssh2 php-yaml \
    && apt-get -y install zlib1g-dev libssh2-1-dev libpng-dev libjpeg-dev \
    && apt-get clean; rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*

RUN echo "127.0.0.1    symfony.local" >> /etc/hosts;

RUN pecl install apcu-5.1.5
#RUN echo "extension=apcu.so" > /usr/local/etc/php/conf.d/apcu.ini

# Node / NPM (php 7.2 version)
RUN \
	mkdir -p /usr/local/src && \
	chown ${MY_USER}:${MY_GROUP} /usr/local/src && \
	VERSION="$( curl -Lq https://nodejs.org 2>/dev/null | grep LTS | grep -Eo 'data-version.*.' | grep -oE 'v[0-9.]+' )" && \
	wget -P /usr/local/src https://nodejs.org/dist/${VERSION}/node-${VERSION}-linux-x64.tar.xz && \
	tar xvf /usr/local/src/node-${VERSION}-linux-x64.tar.xz -C /usr/local/src && \
	ln -s /usr/local/src/node-${VERSION}-linux-x64 /usr/local/node && \
	ln -s /usr/local/node/bin/* /usr/local/bin/ && \
	rm -f /usr/local/src/node-${VERSION}-linux-x64.tar.xz

# Application
WORKDIR /var/www/symfony

RUN git config --global user.email "${USER_EMAIL}"
RUN git config --global user.name "${USER_NAME}"
