ARG PHP_VERSION
FROM php:${PHP_VERSION}-fpm-alpine


## Alpine的源文件为：/etc/apk/repositories  默认源地址为：http://dl-cdn.alpinelinux.org/
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories

RUN apk add bash
#
RUN set -xe; \
    apk update && \
    pecl channel-update pecl.php.net && \
    apk add libzip-dev zip unzip && \
    if [ ${PHP_VERSION} = "7.3" ] || [ ${PHP_VERSION} = "7.4" ] || [ $(php -r "echo PHP_MAJOR_VERSION;") = "8" ]; then \
        docker-php-ext-configure zip; \
    else \
        docker-php-ext-configure zip --with-libzip; \
    fi && \
    docker-php-ext-install zip && \
    docker-php-ext-install pdo pdo_mysql && \
    php -m | grep -q 'zip'


#RUN apk add supervisor

#
#--------------------------------------------------------------------------
# Optional Software's Installation
#--------------------------------------------------------------------------
#
# Optional Software's will only be installed if you set them to `true`
# in the `docker-compose.yml` before the build.
# Example:
#   - INSTALL_SOAP=true
#

###########################################################################
# SOAP:
###########################################################################

ARG INSTALL_SOAP=false

RUN if [ ${INSTALL_SOAP} = true ]; then \
    # Install the soap extension
    apk add libxml2-dev php-soap && \
    docker-php-ext-install soap \
;fi


###########################################################################
# XSL:
###########################################################################

ARG INSTALL_XSL=false

RUN if [ ${INSTALL_XSL} = true ]; then \
    # Install the xsl extension
    apk add libxslt-dev && \
    docker-php-ext-install xsl \
;fi

###########################################################################
# Swoole EXTENSION
###########################################################################

ARG INSTALL_SWOOLE=false
RUN set -eux; \
    if [ ${INSTALL_SWOOLE} = true ]; then \
      # Install Php Swoole Extension
      if   [ $(php -r "echo PHP_VERSION_ID - PHP_RELEASE_VERSION;") = "50600" ]; then \
        pecl install swoole-2.0.10; \
      elif [ $(php -r "echo PHP_VERSION_ID - PHP_RELEASE_VERSION;") = "70000" ]; then \
        pecl install swoole-4.3.5; \
      elif [ $(php -r "echo PHP_VERSION_ID - PHP_RELEASE_VERSION;") = "70100" ]; then \
        pecl install swoole-4.5.11; \
      else \
        pecl install swoole; \
      fi; \
      docker-php-ext-enable swoole; \
      php -m | grep -q 'swoole' \
    ;fi

###########################################################################
# bcmath:
###########################################################################

ARG INSTALL_BCMATH=false

RUN if [ ${INSTALL_BCMATH} = true ]; then \
    # Install the bcmath extension
    docker-php-ext-install bcmath \
;fi


###########################################################################
# Opcache:
###########################################################################

ARG INSTALL_OPCACHE=false

RUN if [ ${INSTALL_OPCACHE} = true ]; then \
    docker-php-ext-install opcache \
;fi

###########################################################################
# Composer:
###########################################################################

USER root

#  如果使用php7.2.5 以下会有一个问题， composer2.3 版本不能降级到2.2 ，使用官方的方式结局
# ARG COMPOSER_MIRROR=https://mirrors.aliyun.com/composer/

# RUN wget -O /usr/local/bin/composer ${COMPOSER_MIRROR}"composer.phar" && \
#     chmod +x /usr/local/bin/composer && composer self-update --2.2 && \
#     composer config -g repo.packagist composer ${COMPOSER_MIRROR}

RUN cd /usr/local/bin/ && php -r "copy('https://install.phpcomposer.com/installer', 'composer-setup.php');" &&  php composer-setup.php && php -r "unlink('composer-setup.php');" && mv composer.phar composer

COPY ./laravel.ini /usr/local/etc/php/conf.d

WORKDIR /var/www

CMD ["php-fpm"]

EXPOSE 9000