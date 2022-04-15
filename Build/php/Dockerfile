ARG PHP_VERSION="${PHP_VERSION}"
FROM php:${PHP_VERSION:+${PHP_VERSION}-}fpm-alpine
RUN apk update; \
    apk upgrade; \
    apk add nano;
RUN docker-php-ext-install mysqli
RUN docker-php-ext-install pdo pdo_mysql
COPY --from=composer /usr/bin/composer /usr/bin/composer
RUN composer global require "laravel/installer=~1.1"
RUN echo 'export PATH="$PATH:$HOME/.composer/vendor/bin"' >> ~/.bashrc
WORKDIR ${www}