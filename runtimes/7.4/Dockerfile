FROM php:7.4-fpm 

WORKDIR /var/www

# Install dependencies.
RUN apt-get update && apt-get upgrade -y && apt-get install -y nginx

ADD --chmod=0755 https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/

RUN install-php-extensions gd xdebug pdo_mysql mysqli intl 

# Copy composer executable.
COPY --from=composer:2.3.5 /usr/bin/composer /usr/bin/composer

COPY --from=node:16.0 /usr/local/bin /usr/local/bin
COPY --from=node:16.0 /usr/local/lib /usr/local/lib
COPY --from=node:16.0 /opt /opt

RUN groupadd -g 1000 bass
RUN useradd -ms /bin/bash --no-user-group -g 1000 -u 1000 bass

# Copy configuration files.
COPY ./config/php.ini /usr/local/etc/php/php.ini
COPY ./config/php-fpm.conf /usr/local/etc/php-fpm.conf
COPY ./config/nginx.conf /etc/nginx/nginx.conf

RUN rm /etc/nginx/sites-enabled/default
RUN rm /etc/nginx/sites-available/default
RUN rm -R /var/www/html

COPY ./start-container /usr/local/bin/start-container

RUN chmod +x /usr/local/bin/start-container

EXPOSE 80

ENTRYPOINT ["start-container"]

