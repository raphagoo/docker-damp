FROM php:7.0-fpm

RUN apt-get update && apt-get install -y\
    # dev
    git\
    # environment wkhtmltopdf
    libxrender1 libfontconfig1 libxext6

# ------------------------------------------------------------ configuration PHP
RUN cp "$PHP_INI_DIR/php.ini-development" "$PHP_INI_DIR/php.ini";

# --------------------------------------------------------------- extensions PHP
ADD https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/
RUN chmod +x /usr/local/bin/install-php-extensions
RUN chown -R www-data /var/www
RUN install-php-extensions\
    # dev
    @composer xdebug\
    pdo_mysql mysqli gd intl zip gmp
