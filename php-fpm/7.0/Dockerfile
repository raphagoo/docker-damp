# @see: https://github.com/llaumgui/docker-images
FROM php:7.0-fpm-alpine

# TODO wkhtmltopdf
# TODO tcpdf

# ---------------------------------------------------------- Setup build options
ARG DOCKER_PHP_ENABLE_COMPOSER
ARG DOCKER_PHP_ENABLE_MYSQL
ARG DOCKER_PHP_ENABLE_XDEBUG
ARG DOCKER_USER_UID=false
ARG DOCKER_USER_GID=false

RUN \
    echo -n "With composer support:      " ; if [[ "${DOCKER_PHP_ENABLE_COMPOSER}" = "on" ]] ;    then echo "Yes"; else echo "No" ; fi && \
    echo -n "With mysql support:         " ; if [[ "${DOCKER_PHP_ENABLE_MYSQL}" = "on" ]] ;    then echo "Yes"; else echo "No" ; fi && \
    echo -n "With xdebug support:        " ; if [[ "${DOCKER_PHP_ENABLE_XDEBUG}" = "on" ]] ;      then echo "Yes"; else echo "No" ; fi && \
    if [ -z ${DOCKER_USER_UID+x} ]; then echo "DOCKER_USER_UID is unset"; DOCKER_USER_UID=1000; else echo "DOCKER_USER_UID is set to '$DOCKER_USER_UID'"; fi && \
    if [ -z ${DOCKER_USER_GID+x} ]; then echo "DOCKER_USER_GID is unset"; DOCKER_USER_GID=1000; else echo "DOCKER_USER_GID is set to '$DOCKER_USER_GID'"; fi


# --------------------------------------------------------- Install dependancies
RUN apk add --update --no-cache \
        # Dependancy for intl \
        icu-libs \
        libintl \
        # Dependancy for zip \
        libzip \
        # Dependancy for gmp \
        gmp


# --------------------------------------------------- Install build dependancies
RUN apk add --update --no-cache --virtual .docker-php-global-dependancies \
        # Build dependencies for gd \
        freetype-dev \
        libjpeg-turbo-dev \
        libpng-dev \
        # Build dependency for intl \
        icu-dev \
        # Build dependencies for Zip \
        libzip-dev \
        # Build dependencies for GMP \
        gmp-dev \
        # Build dependancies for Pecl \
        $PHPIZE_DEPS


# ------------------------------------------------------- Install php extensions
RUN php -m && \
    docker-php-ext-configure gd \
        --with-freetype-dir=/usr/include/ \
        --with-jpeg-dir=/usr/include/ \
        --with-png-dir=/usr/include/ && \
    docker-php-ext-configure intl --enable-intl && \
    docker-php-ext-configure zip --enable-zip --with-libzip && \
    docker-php-ext-install gmp \
        gd \
        intl \
        zip && \
    php -m


# --------------------------------------------------- Conditionnal installations
# Enable MySQL
RUN if [ "${DOCKER_PHP_ENABLE_MYSQL}" != "off" ]; then \
        apk add --update --no-cache --virtual .docker-php-mysql-dependancies \
            # Dependancy for mysql \
            mysql-client && \
        # MySQLnd is already compiled. \
        # See: https://github.com/docker-library/php/issues/167 \
        docker-php-ext-configure mysqli && \
        docker-php-ext-configure pdo_mysql && \
        docker-php-ext-install mysqli \
            pdo_mysql && \
        apk del .docker-php-mysql-dependancies && \
        php -m; \
    else \
        echo "Skip mysql support"; \
    fi

# Enable Xdebug
RUN if [ "${DOCKER_PHP_ENABLE_XDEBUG}" != "off" ]; then \
        yes | pecl install xdebug-2.7.2 && \
        echo "zend_extension=$(find /usr/local/lib/php/extensions/ -name xdebug.so)" > /usr/local/etc/php/conf.d/xdebug.ini && \
        echo "xdebug.remote_enable=on" >> /usr/local/etc/php/conf.d/xdebug.ini && \
        echo "xdebug.remote_autostart=off" >> /usr/local/etc/php/conf.d/xdebug.ini && \
        echo "xdebug.remote_port=9900" >> /usr/local/etc/php/conf.d/xdebug.ini && \
        php -m; \
    else \
        echo "Skip xdebug support"; \
    fi


# --------------------------------------------- Conditionnal tools installations
# Install composer.
RUN if [ "${DOCKER_PHP_ENABLE_COMPOSER}" != "off" ]; then \
        EXPECTED_SIGNATURE=$(wget -q -O - https://composer.github.io/installer.sig) && \
        php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && \
        ACTUAL_SIGNATURE=$(php -r "echo hash_file('SHA384', 'composer-setup.php');") && \
        if [ "$EXPECTED_SIGNATURE" != "$ACTUAL_SIGNATURE" ]; then \
            >&2 echo 'ERROR: Invalid installer signature' && \
            rm composer-setup.php && \
            exit 1; \
        else \
            php composer-setup.php --install-dir=/usr/bin --filename=composer && \
            RESULT=$? && \
            rm composer-setup.php && \
            exit $RESULT && \
            composer -V; \
        fi; \
    else \
        echo "Skip composer support"; \
    fi


# ---------------------------------------------------------------- Configuration
# My configuration.
RUN sed -i "s/^;security.limit_extensions =.*/security.limit_extensions = .php .php7 .php70/g" /usr/local/etc/php-fpm.d/www.conf
# COPY php.d /usr/local/etc/php/conf.d
# COPY etc /usr/local/etc/php

# Big clean
RUN apk del .docker-php-global-dependancies && \
    rm -rf /var/cache/apk/* && \
    docker-php-source delete

# Add user for php-cli
RUN if [ "${DOCKER_USER_GID}" != "false" ] && [ "${DOCKER_USER_UID}" != "false" ]; then \
        addgroup -g ${DOCKER_USER_GID} php-cli && \
        adduser -D -u ${DOCKER_USER_UID} -G php-cli php-cli; \
    else \
        echo "No user for php-cli"; \
    fi

EXPOSE 9000
WORKDIR /var/www
