FROM php:fpm

# For composer
RUN apt-get update \
  && apt-get install -y libzip-dev zlib1g-dev \
  && docker-php-ext-install zip

# Install composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
  && php -r "if (hash_file('sha384', 'composer-setup.php') === 'a5c698ffe4b8e849a443b120cd5ba38043260d5c4023dbf93e1558871f1f07f58274fc6f4c93bcfd858c6bd0775cd8d1') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \
  && php composer-setup.php \
  && php -r "unlink('composer-setup.php');" \
  && mv composer.phar /usr/local/bin/composer

# Set composer environment
ENV COMPOSER_ALLOW_SUPERUSER 1
ENV COMPOSER_HOME /composer
ENV PATH $PATH:/composer/vendor/bin

# Install laravel installer
RUN composer global require "laravel/installer"

# PHP's DB setting
RUN apt-get update \
  && apt-get install -y libpq-dev \
  && docker-php-ext-install pdo_mysql pdo_pgsql

# Install Node.js
RUN curl -sL https://deb.nodesource.com/setup_6.x | bash - \
  && apt-get update \
  && apt-get install -y nodejs

# Install XDebug
RUN pecl install xdebug && \
  docker-php-ext-enable xdebug

WORKDIR /src
