FROM php:8.2-fpm-alpine

RUN docker-php-ext-install pdo pdo_mysql

# Allow super user - set this if you use Composer as a super user at all times like in Docker containers
ENV COMPOSER_ALLOW_SUPERUSER=1

# Obtain Composer using multi-stage build
COPY --from=composer:2.4 /usr/bin/composer /usr/bin/composer

# Copy only composer.json and composer.lock (instead of copying the entire source)
# This is enough to take advantage of docker cache and composer install will
# be executed only when composer.json or composer.lock have indeed changed
COPY ./app/composer.* ./

# Install Composer dependencies
RUN composer install --prefer-dist --no-dev --no-scripts --no-progress --no-interaction

# Copy application files to the working directory
COPY ./app .

# Run composer dump-autoload --optimize
RUN composer dump-autoload --optimize

# Expose port 9000 and start php-fpm server
EXPOSE 9000
CMD ["php-fpm"]
