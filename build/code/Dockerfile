FROM composer:1.7 as bootstrap

ENV SYSPASS_VERSION="3.1.2.20030701"

RUN git clone --branch ${SYSPASS_VERSION} https://github.com/nuxsmin/sysPass.git \
    && composer install \
    --ignore-platform-reqs \
    --no-interaction \
    --no-plugins \
    --no-scripts \
    --prefer-dist \
    --optimize-autoloader \
    --working-dir /app/sysPass

FROM php:7.3-fpm as app

LABEL maintainer="luigdima" \
    org.label-schema.name="syspass-code" \
    org.label-schema.description="sysPass code" \
    org.label-schema.version=${SYSPASS_VERSION} \
    org.label-schema.url="https://github.com/luigdima/syspass-nginx-traefik2" \
    org.label-schema.vcs-ref=$VCS_REF \
    org.label-schema.vcs-url="https://github.com/luigdima/syspass-nginx-traefik2" \
    org.label-schema.vendor="luigdima" \
    org.label-schema.schema-version="1.0"

RUN apt-get update \
    && apt-get install -y \
    locales \
    git \
    gosu \
    libicu-dev \
    libldb-dev \
    libldap2-dev \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    unzip \
    ssl-cert \
    && apt-get clean \
    && rm -r /var/lib/apt/lists/* \
    && pecl install xdebug-2.7.1 \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) ldap intl gettext pdo_mysql opcache gd \
    && docker-php-ext-enable xdebug ldap intl pdo_mysql

ENV HTTP_RUN_USER="www-data" \
    SYSPASS_DIR="/var/www/html/sysPass" \
    SYSPASS_UID=9001 

WORKDIR /var/www/html

COPY entrypoint.sh /usr/local/sbin/
RUN chmod 755 /usr/local/sbin/entrypoint.sh 

COPY --from=bootstrap /app/sysPass/ ${SYSPASS_DIR}/
COPY --from=bootstrap /usr/bin/composer /usr/bin/

ENTRYPOINT ["/usr/local/sbin/entrypoint.sh"]
CMD ["init"]