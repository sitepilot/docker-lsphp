# --------------- [LSPHP-CLI] --------------- #
FROM ghcr.io/sitepilot/runtime:v1 as cli

USER root

ARG TARGETARCH
ARG PHP_VERSION=8.1

ENV RUNTIME_LOG_LEVEL=3 \
    PHP_VERSION=${PHP_VERSION}

RUN rt-install gnupg2 less msmtp unzip

ADD packages/${PHP_VERSION}-${TARGETARCH}.txt /tmp/packages.txt

RUN wget -O /etc/apt/trusted.gpg.d/lst_debian_repo.gpg http://rpms.litespeedtech.com/debian/lst_debian_repo.gpg \
    && wget -O /etc/apt/trusted.gpg.d/lst_repo.gpg http://rpms.litespeedtech.com/debian/lst_repo.gpg \
    && echo "deb http://rpms.litespeedtech.com/debian/ jammy main" > /etc/apt/sources.list.d/lst_debian_repo.list \
    && rt-install $(cat /tmp/packages.txt) \
    && mkdir -p /etc/php \
    && export LSPHP_VERSION=$(echo $PHP_VERSION | sed s/[.]//g) \
    && ln -sf /usr/local/lsws/lsphp$LSPHP_VERSION /etc/php/current \
    && ln -sf /usr/local/lsws/lsphp$LSPHP_VERSION/bin/php /usr/local/bin/php \
    && rt-rchown /usr/local/lsws

COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

COPY --from=wordpress:cli-2 /usr/local/bin/wp /usr/local/bin/wp

COPY --chown=$RUNTIME_UID:$RUNTIME_GID cli /

USER $RUNTIME_UID

# --------------- [LSPHP-OLS] --------------- #
FROM cli as ols

USER root

ENV RUNTIME_LOG_LEVEL=1 \
    OLS_LOGS_DIR="$RUNTIME_WORKDIR/logs" \
    OLS_CERTS_DIR="$RUNTIME_WORKDIR/certs" \
    OLS_FILES_DIR="$RUNTIME_WORKDIR/files" \
    OLS_PUBLIC_DIR="/"

RUN rt-install openlitespeed \
    && rt-rchown /usr/local/lsws

COPY --chown=$RUNTIME_UID:$RUNTIME_GID ols /

USER $RUNTIME_UID

WORKDIR $OLS_FILES_DIR

EXPOSE 80

EXPOSE 443

CMD ["/usr/local/lsws/bin/litespeed", "-d"]
