# syntax=docker/dockerfile:1

ARG PHP_FPM_VERSION

FROM php:${PHP_FPM_VERSION}

ARG UNIT3D_VERSION
ARG FPM_HEALTHCHECK_VERSION
ENV UNIT3D_VERSION=${UNIT3D_VERSION}
ENV FPM_HEALTHCHECK_VERSION=${FPM_HEALTHCHECK_VERSION}

WORKDIR /var/www

RUN apt-get update \
    && apt-get install -y libfreetype6-dev libjpeg62-turbo-dev libpng-dev sensible-utils ucf libicu-dev icu-devtools wget fcgiwrap zip libzip4 libzip-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && printf '\n' | pecl install redis-5.3.7 \
    && docker-php-ext-install -j$(nproc) gd intl bcmath opcache zip \
    && docker-php-ext-enable gd redis intl bcmath opcache zip \
    && rm -r /var/www/html \
    && printf "\n\n\n\n\n\nURL: https://github.com/HDInnovations/UNIT3D-Community-Edition/archive/refs/tags/v${UNIT3D_VERSION}.tar.gz\n\n\n\n\n" \
    && curl -Lo unit3d.tgz "https://github.com/HDInnovations/UNIT3D-Community-Edition/archive/refs/tags/v${UNIT3D_VERSION}.tar.gz" \
    && tar -xzf unit3d.tgz \
    && rm unit3d.tgz \
    && mv "UNIT3D-Community-Edition-${UNIT3D_VERSION}" unit3d \
    && curl -Lo php-fpm-healthcheck.tgz "https://github.com/renatomefi/php-fpm-healthcheck/archive/refs/tags/v${FPM_HEALTHCHECK_VERSION}.tar.gz" \
    && tar -xzf php-fpm-healthcheck.tgz \
    && rm php-fpm-healthcheck.tgz \
    && mv "php-fpm-healthcheck-${FPM_HEALTHCHECK_VERSION}/php-fpm-healthcheck" /usr/bin/ \
    && rm -r "php-fpm-healthcheck-${FPM_HEALTHCHECK_VERSION}" \
    && mv /usr/local/etc/php/php.ini-production /usr/local/etc/php/php.ini \
    && apt-get clean \
    && echo "pm.status_path = /status" >> /usr/local/etc/php-fpm.d/zz-docker.conf \
#    && sh /var/www/unit3d/composer-setup.sh \
#    && sh /var/www/unit3d/composer update \
#    && sh /var/www/unit3d/composer install

# https://github.com/renatomefi/php-fpm-healthcheck
# Perform a healthcheck every 5 seconds, for each one, fpm has 2 seconds to respond.
# After 6 consecutive failures (30 seconds of being unresponsive/too slow) the container becomes unhealthy.
HEALTHCHECK --interval=5s --timeout=2s --start-period=5s --retries=6 \
    CMD /usr/bin/php-fpm-healthcheck || exit 1

VOLUME /var/www/unit3d
VOLUME /usr/local/etc/php/conf.d

EXPOSE 9000/tcp