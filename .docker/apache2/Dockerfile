FROM php:8.0-apache

ARG uid

RUN useradd -G www-data,root -u $uid -d /home/euser euser

RUN mkdir -p /home/euser/.composer && \
    chown -R euser:euser /home/euser

RUN apt-get update --fix-missing -q

#RUN apt-get update -y && apt-get install -y openssl mcrypt zip unzip git libonig-dev nodejs npm
RUN apt-get install -y openssl zip unzip git libonig-dev  libxml2-dev libpng-dev libjpeg-dev libfreetype6-dev libonig-dev curl mcrypt gnupg build-essential software-properties-common wget vim libwebp-dev
RUN curl -sL https://deb.nodesource.com/setup_14.x  | bash -
RUN apt-get -y install nodejs

RUN  docker-php-ext-configure gd --with-freetype=/usr/include/ --with-webp=/usr/include/  --with-jpeg=/usr/include

RUN docker-php-ext-install mysqli mbstring pdo pdo_mysql tokenizer xml bcmath opcache pcntl intl soap exif gd

RUN curl -sSL https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

ENV APACHE_DOCUMENT_ROOT=/var/www/html/public

ENV APACHE_LOG_DIR /var/log/apache2

ENV APACHE_RUN_DIR /var/run/apache2

ENV APACHE_LOCK_DIR /var/lock/apache2

ENV APACHE_PID_FILE /var/run/apache2/apache2.pid

RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf

RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf

RUN a2enmod rewrite

COPY . /var/www/html/

CMD ["/usr/sbin/apache2", "-D", "FOREGROUND"]
