#Working Dockerfile
FROM ubuntu:xenial

# Fixes some weird terminal issues such as broken clear / CTRL+L
ENV TERM=linux
ARG DEBIAN_FRONTEND=noninteractive

RUN apt-get update
RUN apt-get -y dist-upgrade
RUN apt-get install -y python-software-properties
RUN apt-get install -y software-properties-common
RUN apt-get install -y --no-install-recommends curl ca-certificates build-essential make gcc git-core
RUN apt-get install -y locales
RUN apt-get install -y ssh

RUN echo "deb http://ppa.launchpad.net/ondrej/php/ubuntu xenial main" > /etc/apt/sources.list.d/ondrej-ubuntu-php-xenial.list
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 4F4EA0AAE5267A6C

RUN locale-gen en_US.UTF-8 && export LANG=en_US.UTF-8
RUN LC_ALL=C.UTF-8 add-apt-repository -y ppa:ondrej/php

RUN apt-get update --fix-missing

# Install dependencies
RUN apt-get install -y --no-install-recommends --fix-missing php5.6-common php5.6-geoip php5.6-igbinary php5.6-memcache \
        php5.6-mongodb php5.6-msgpack php-pear php5.6-redis php5.6-xdebug php5.6-xml php5.6 php5.6-cli \
        php5.6-common php5.6-dev php5.6-fpm php5.6-json php5.6-mbstring php5.6-opcache php5.6-readline php5.6-xml \
        php5.6-intl php5.6-gd php5.6-zip php5.6-curl php5.6-apcu php5.6-pdo php5.6-mysql php5.6-mysqli \
        php-amqp php-mongo

RUN apt-get install -y --no-install-recommends wget git

RUN apt-get clean

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin/ \
      --filename=composer

#RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*

#RUN apt-get --purge autoremove

# Configure FPM to run properly on docker
RUN usermod -u 1000 www-data

RUN mkdir /code
RUN mkdir /run/php

# Open up fcgi port
EXPOSE 9000

# The following runs FPM and removes all its extraneous log output on top of what your app outputs to stdout
CMD /usr/sbin/php-fpm5.6 -F -O 2>&1 | sed -u 's,.*: \"\(.*\)$,\1,'| sed -u 's,"$,,' 1>&1

WORKDIR "/code"
