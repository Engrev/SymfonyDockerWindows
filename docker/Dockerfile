FROM php:8.3-apache

RUN apt update && \
    apt install -y zlib1g-dev g++ git libicu-dev zip libzip-dev nodejs npm vim openssl && \
    docker-php-ext-install intl opcache pdo pdo_mysql && \
    docker-php-ext-configure zip && \
    docker-php-ext-install zip

EXPOSE 80 443

RUN a2enmod rewrite ssl
RUN npm install --global yarn
RUN mv "$PHP_INI_DIR/php.ini-development" "$PHP_INI_DIR/php.ini"
RUN git config --global user.email "your@email.com" && \
    git config --global user.name "Your Name"

WORKDIR /var/www

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer && \
    curl -sS https://get.symfony.com/cli/installer | bash && \
    mv /root/.symfony5/bin/symfony /usr/local/bin/symfony