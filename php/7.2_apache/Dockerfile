FROM php:7.2-apache

# configuration d'Apache
# COPY apache.conf /etc/apache2/conf-enabled/custom.conf
RUN printf "\nIncludeOptional custom/*.conf\n" >> /etc/apache2/apache2.conf
RUN apt-get update
# Install Xdebug
RUN pecl install -f xdebug \
    && echo "zend_extension=$(find /usr/local/lib/php/extensions/ -name xdebug.so)" > /usr/local/etc/php/conf.d/xdebug.ini \
    && a2enmod rewrite \
    && docker-php-ext-install pdo pdo_mysql
RUN apt-get install -y zlib1g-dev \
    && docker-php-ext-install zip
RUN apt-get install -y libicu-dev \
    && docker-php-ext-install intl
RUN apt-get install -y libgmp-dev re2c libmhash-dev libmcrypt-dev file \
    && ln -s /usr/include/x86_64-linux-gnu/gmp.h /usr/local/include/ \
    && docker-php-ext-configure gmp \
    && docker-php-ext-install gmp
# wkhtmltopdf requires 3 lib:
RUN apt-get install -y libxrender1 libfontconfig1 libxext6
# tcpdf requires 1 lib:
RUN apt-get install -y libfreetype6-dev libjpeg62-turbo-dev libpng-dev \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd
