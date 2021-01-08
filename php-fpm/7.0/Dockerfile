# @see: https://github.com/llaumgui/docker-images
FROM php:7.0-fpm-alpine

# TODO wkhtmltopdf
# TODO tcpdf

# ---------------------------------------------------------- Setup build options
ARG DOCKER_PHP_ENABLE_COMPOSER
ARG DOCKER_PHP_ENABLE_XDEBUG
ARG DOCKER_USER_UID=false
ARG DOCKER_USER_GID=false

# --------------------------------------------------------- Install dependancies
RUN apk add --update --no-cache \
        # Build dependencies for gd \
        freetype \
        libpng \
        libjpeg-turbo \
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
        --with-gd \
        --with-freetype-dir=/usr/include/ \
        --with-jpeg-dir=/usr/include/ \
        --with-png-dir=/usr/include/ && \
    docker-php-ext-configure intl --enable-intl && \
    docker-php-ext-configure zip --enable-zip --with-libzip && \
    docker-php-ext-install gmp \
        gd \
        intl \
        zip


# --------------------------------------------------- Conditionnal installations
# Enable MySQL
RUN apk add --update --no-cache --virtual .docker-php-mysql-dependancies \
        # Dependancy for mysql \
        mysql-client && \
    # MySQLnd is already compiled. \
    # See: https://github.com/docker-library/php/issues/167 \
    docker-php-ext-configure mysqli && \
    docker-php-ext-configure pdo_mysql && \
    docker-php-ext-install mysqli pdo_mysql && \
    apk del .docker-php-mysql-dependancies

# Enable Xdebug
RUN if [ "${DOCKER_PHP_ENABLE_XDEBUG}" != "off" ]; then \
        # je ne m'explique pas pourquoi, maius nécessaire pour une reconstruction d'image
        pecl uninstall xdebug && \
        pecl install xdebug-2.7.2; \
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
    fi


# ---------------------------------------------------------------- Configuration
# My configuration.
RUN sed -i "s/^;security.limit_extensions =.*/security.limit_extensions = .php .php7 .php70/g" /usr/local/etc/php-fpm.d/www.conf

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
