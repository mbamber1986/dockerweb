ARG PHP_VERSION="${PHP_VERSION}"
FROM php:${PHP_VERSION:+${PHP_VERSION}-}fpm
RUN apt update -y; \
    apt upgrade -y; \
    apt install nano -y;
RUN docker-php-ext-install mysqli
RUN docker-php-ext-install pdo pdo_mysql
RUN apt install git -y
COPY --from=composer /usr/bin/composer /usr/bin/composer
RUN echo 'export PATH="$PATH:$HOME/.composer/vendor/bin"' >> ~/.bashrc
WORKDIR ${www}