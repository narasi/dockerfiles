FROM ubuntu:16.04

RUN apt-get -y update

RUN apt-get -y install curl \
                       php7.0 \
                       apache2 \
                       git \
                       rsyslog \
                       mysql-client \
                       php7.0-cli \
                       php7.0-mbstring \
                       php7.0-mysql \
                       php7.0-gd \
                       php7.0-xml \
                       php7.0-zip \
                       php7.0-curl \
                       libapache2-mod-php7.0 \
                       lynx-cur \
                       debconf-utils \
                       wget \ 
                       libapache2-mod-fastcgi \
                       php7.0-fpm

RUN curl -s5 https://getcomposer.org/installer | php

WORKDIR app

RUN cd /app/ && wget https://ftp.drupal.org/files/projects/drupal-8.3.7.tar.gz && tar -zxvf *.tgz

COPY ./init.sh /usr/local/bin/init
RUN chmod +rx /usr/local/bin/*

RUN cd /app/ && php compoer.phar install

COPY ./001-site.conf /etc/apache2/sites-available/
RUN a2enmod php7.0
RUN a2enmod rewrite
RUN a2enmod proxy
RUN a2enmod proxy_http
RUN a2enmod expires
RUN a2enmod fascgi
RUN a2enmod proxy_fcgi
RUN a2enconf php7.0-fpm

ENV APACHE_RUN_USER www-data
ENV APACHE_RUN_GROUP www-data
ENV APACHE_LOG_DIR /var/log
ENV APACHE_LOCK_DIR /var/lock/apache2
ENV APACHE_PID_FILE /var/run/apache2.pid

RUN a2ensite 001-site
RUN a2dissite 000-default

EXPOSE 80

CMD init
