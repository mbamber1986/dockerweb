ARG APACHE_VERSION=${APACHE_VERSION}
FROM httpd:${APACHE_VERSION:+${APACHE_VERSION}-}alpine
# Update Build and install dependencies.
RUN apk update; \
    apk upgrade;
    RUN apk add apache2-utils
RUN apk add git openssl curl ca-certificates
ENV WWW = ${WWW}
RUN sed -i "s/#LoadModule\ rewruite_module/LoadModule\ rewrite_module/" /usr/local/apache2/conf/httpd.conf;
# Copy apache vhost file to proxy php requests to php-fpm container
COPY confs/apache.conf /usr/local/apache2/conf/apache.conf
RUN echo "Include /usr/local/apache2/conf/apache.conf" \
    >> /usr/local/apache2/conf/httpd.conf
# end apache injection 