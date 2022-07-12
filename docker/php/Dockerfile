FROM php:8.1.5-fpm-buster
# install composer
RUN cd /usr/bin && curl -s http://getcomposer.org/installer | php && ln -s /usr/bin/composer.phar /usr/bin/composer

RUN apt-get update \
&& apt-get install -y \
git \
zip \
unzip \
vim \
gnupg \
libfreetype6-dev \
libjpeg62-turbo-dev \
libpng-dev \
libnss3 \
libdbus-1-3 \
xvfb \
libgtk2.0-0 \
libgtk-3-0 \
libgbm-dev \
libnotify-dev \
libxss1 \
libasound2 \
libxtst6 \
xauth \
wget \
lsof \
libicu-dev

#libgconf-2-4 \

# chrome driver
#RUN apt-get -y update && apt-get -y install chromium libgconf-2-4
RUN wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add -
RUN echo "deb http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list
RUN apt-get update
RUN apt-get install -y google-chrome-stable
RUN apt-get install -y fonts-ipafont

# PHP extension ext-http
ADD https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/
RUN chmod +x /usr/local/bin/install-php-extensions && sync && \
    install-php-extensions http zip

# Node.js
RUN curl -sL https://deb.nodesource.com/setup_12.x | bash -
RUN apt-get install -y nodejs

# yarn
RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -
RUN echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list
RUN apt-get update && apt-get install yarn

RUN apt-get update \
    && apt-get install -y libpq-dev libicu-dev \
    && docker-php-ext-install pdo_mysql pdo_pgsql \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install bcmath \
    && docker-php-ext-install intl

#RUN pecl install xdebug && docker-php-ext-enable xdebug
COPY php.ini /usr/local/etc/php/php.ini

WORKDIR /var/www/html/laravel
