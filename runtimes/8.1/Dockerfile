FROM node:18.0 as node
FROM php:8.1-fpm 

WORKDIR /var/www

# update repo list
RUN apt-get update && apt-get upgrade -y && apt-get install -y nginx

# install php ext installer
ADD --chmod=0755 https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/

# Install ext.
RUN install-php-extensions gd xdebug pdo_mysql mysqli intl calendar exif  gettext zip

# Copy composer executable.
COPY --from=composer:2.3.5 /usr/bin/composer /usr/bin/composer

# install 
COPY --from=node /usr/local/bin /usr/local/bin
COPY --from=node /usr/local/lib /usr/local/lib
COPY --from=node /opt /opt

COPY ./config/php.ini /usr/local/etc/php/php.ini
COPY ./config/php-fpm.conf /usr/local/etc/php-fpm.conf
COPY ./config/nginx.conf /etc/nginx/nginx.conf

RUN rm /etc/nginx/sites-enabled/default
RUN rm /etc/nginx/sites-available/default
RUN rm -R /var/www/html

RUN groupadd -g 1000 bass
RUN useradd -ms /bin/bash --no-user-group -g 1000 -u 1000 bass

COPY ./start-container /usr/local/bin/start-container
RUN chmod +x /usr/local/bin/start-container
EXPOSE 8000

USER bass

ENTRYPOINT ["start-container"]
