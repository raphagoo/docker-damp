FROM php:7.4-fpm

ARG DOCKER_PHP_ENABLE_XDEBUG=on
ARG DOCKER_PHP_PRODUCTION=off

RUN apt-get update

# ----------------------------------------------------------------- Enable MySQL
RUN docker-php-ext-install pdo pdo_mysql mysqli

# ----------------------------------------------------------------- gd extension
RUN apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install gd

# --------------------------------------------------------------- intl extension
RUN apt-get install -y \
        libicu-dev \
    && docker-php-ext-configure intl --enable-intl \
    && docker-php-ext-install intl

# ---------------------------------------------------------------- zip extension
RUN apt-get install -y \
        libzip-dev \
    && docker-php-ext-configure zip \
    && docker-php-ext-install zip

# ---------------------------------------------------------------- gmp extension
RUN apt-get install -y \
        libgmp-dev \
#         libmhash-dev \
#         libmcrypt-dev\
#     && ln -s /usr/include/x86_64-linux-gnu/gmp.h /usr/local/include/
    && docker-php-ext-configure gmp \
    && docker-php-ext-install gmp

# ------------------------------------------------------------- Install composer
RUN curl -sS https://getcomposer.org/installer | \
    php -- --install-dir=/usr/local/bin --filename=composer

# --------------------------------------------------------------- Install Xdebug
RUN if [ "${DOCKER_PHP_ENABLE_XDEBUG}" != "off" ]; then \
        pecl install -f xdebug && \
        echo "zend_extension=$(find /usr/local/lib/php/extensions/ -name xdebug.so)" \
            > /usr/local/etc/php/conf.d/xdebug.ini; \
    fi

# ------------------------------------------------------------ configuration PHP
RUN if [ "${DOCKER_PHP_PRODUCTION}" != "off" ]; then \
        cp "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini"; \
    else \
        cp "$PHP_INI_DIR/php.ini-development" "$PHP_INI_DIR/php.ini"; \
    fi

# ------------------------------------------------------ environment wkhtmltopdf
RUN apt-get install -y libxrender1 libfontconfig1 libxext6
