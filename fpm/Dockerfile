FROM php:5.6-fpm

# install the PHP extensions we need
RUN apt-get update && apt-get install -y \
      libfreetype6-dev \
      libjpeg62-turbo-dev \
      libjpeg-dev \
      libmcrypt-dev \
      libpng12-dev \
      vim \
    && rm -rf /var/lib/apt/lists/* \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include --with-png-dir=/usr --with-jpeg-dir=/usr \
    && docker-php-ext-install gd mysqli iconv mcrypt

VOLUME /var/www/html

ENV WORDPRESS_VERSION 4.6.1
ENV WORDPRESS_SHA1 027e065d30a64720624a7404a1820e6c6fff1202

# upstream tarballs include ./wordpress/ so this gives us /usr/src/wordpress
RUN curl -o wordpress.tar.gz -SL https://wordpress.org/wordpress-${WORDPRESS_VERSION}.tar.gz \
  && echo  "$WORDPRESS_SHA1 *wordpress.tar.gz" | sha1sum -c - \
  && tar -  zf wordpress.tar.gz -C /usr/src/ \
  && rm wor  press.tar.gz \
  && chown -   www-data:www-data /usr/src/wordpress

COPY docker-entrypoint.sh /entrypoint.sh

# grr, ENTRYPOINT resets CMD now
ENTRYPOINT ["/entrypoint.sh"]
CMD ["php-fpm"]
