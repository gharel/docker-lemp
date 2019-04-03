FROM php:7.3-fpm-alpine

# Specific when located in OPT-NC
ENV http_proxy 'http://192.168.52.120:3128'
ENV https_proxy 'http://192.168.52.120:3128'
ENV no_proxy 'localhost;127.0.0.1;*.intranet.opt;161.172.16.*;172.24.*;192.168.*;webmail.opt.nc;srv-cas.intranet.opt;srv-cascop.intranet.opt;srv-casgal.intranet.opt;srv-project2010;sibilo;sod-qualif;dialin.opt.nc;meet.opt.nc;wac.opt.nc;sopk5.oss.opt.nc;webexternal.opt.nc;srv-fe1.intranet.opt;autodiscover.opt.nc;autodiscover.root.opt;sibilo2014;sibilo2014.intranet.opt;10.*;*.appli-gestion.nc;spi-ise-1.opt.nc;fsq.opt.nc;fs.opt.nc;carto-qul.opt.nc;carto-dev.opt.nc;10.153.146.100;swd-tcrmws-01-tst-cit;carto.opt.nc;fsso*.opt.nc;fed-qul.opt.nc;fed.opt.nc;*owa.opt.nc;*projets.opt.nc'

# Install all dependencies.
RUN apk --no-cache update
RUN apk --no-cache upgrade
RUN apk add --no-cache --virtual .phpize-deps $PHPIZE_DEPS \
    ca-certificates openssl \
    curl \
    git \
    libxml2 \
    mysql \
    patch \
    tar \
    tzdata \
    freetype-dev libpng-dev libjpeg-turbo-dev
RUN docker-php-ext-configure gd \
    --with-freetype-dir=/usr/include/ \
    --with-png-dir=/usr/include/ \
    --with-jpeg-dir=/usr/include/
RUN docker-php-ext-install \
    gd \
    opcache \
    mysqli pdo_mysql
RUN docker-php-ext-enable \
    opcache
RUN mkdir /www \
 && mkdir /logs \
 #&& touch /logs/error-nginx.log \
 #&& touch /logs/access-nginx.log \
 #&& chown -R www-data:www-data /www/ \
 #&& chown -R www-data:www-data /logs/ \
 && cp /usr/share/zoneinfo/Pacific/Noumea /etc/localtime \
 && echo "alias ll='ls -l'" > /etc/profile.d/ll.sh

# Set configurations
COPY config/php.ini       /usr/local/etc/php/conf.d/php.ini
COPY config/php-fpm.conf  /usr/local/etc/php-fpm.d/www.conf
COPY config/timezone      /etc/timezone
COPY config/hosts         /etc/hosts

WORKDIR /www/web

EXPOSE 9000
CMD ["php-fpm"]
