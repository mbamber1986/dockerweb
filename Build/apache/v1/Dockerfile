ARG APACHE_VERSION=${APACHE_VERSION}
FROM httpd:${APACHE_VERSION:+${APACHE_VERSION}-}alpine
ENV www=/var/www
ENV domain=localhost
# Update Build and install dependencies.
RUN apk update && \
    apk upgrade && \
    apk add apache2-utils && ca-certificates git nano -y\
RUN sed -i "s/#LoadModule\ rewrite_module/LoadModule\ rewrite_module/" /usr/local/apache2/conf/httpd.conf && \
RUN sed -i "s/#LoadModule\ rewruite_module/LoadModule\ rewrite_module/" /usr/local/apache2/conf/httpd.conf;

# Copy apache vhost file to proxy php requests to php-fpm container
COPY confs/apache.conf /usr/local/apache2/conf/apache.conf
RUN echo "Include /usr/local/apache2/conf/apache.conf" \
    >> /usr/local/apache2/conf/httpd.conf
# end apache injection 