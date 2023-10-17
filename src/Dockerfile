# --------------- [LSPHP-CLI] --------------- #
FROM ghcr.io/sitepilot/runtime:1.x as cli

USER root

ARG PHP_VERSION=8.1

ENV RUNTIME_LOG_LEVEL=3 \
    PHP_VERSION=${PHP_VERSION}

RUN $RUNTIME_BIN_DIR/install less

ADD packages/lsphp-${PHP_VERSION}.txt /tmp/packages.txt

RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 011AA62DEDA1F085; \
    curl -s http://rpms.litespeedtech.com/debian/enable_lst_debian_repo.sh | bash; \
    $RUNTIME_BIN_DIR/install msmtp $(cat /tmp/packages.txt); \
    mkdir -p /etc/php; \
    export LSPHP_VERSION=$(echo $PHP_VERSION | sed s/[.]//g); \
    ln -sf /usr/local/lsws/lsphp$LSPHP_VERSION /etc/php/current; \
    ln -sf /usr/local/lsws/lsphp$LSPHP_VERSION/bin/php /usr/local/bin/php; \
    chown -R $RUNTIME_UID:$RUNTIME_GID /usr/local/lsws

COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

COPY --from=wordpress:cli-2 /usr/local/bin/wp /usr/local/bin/wp

COPY cli /

RUN $RUNTIME_BIN_DIR/finalize

USER $RUNTIME_UID

# --------------- [LSPHP-OLS] --------------- #
FROM cli as ols

USER root

ENV RUNTIME_LOG_LEVEL=2 \
    OLS_LOGS_DIR="$RUNTIME_WORKDIR/logs" \
    OLS_CERTS_DIR="$RUNTIME_WORKDIR/certs" \
    OLS_FILES_DIR="$RUNTIME_WORKDIR/files" \
    OLS_PUBLIC_DIR="/"

RUN $RUNTIME_BIN_DIR/install openlitespeed; \
    chown -R $RUNTIME_UID:$RUNTIME_GID /usr/local/lsws

COPY ols /

RUN $RUNTIME_BIN_DIR/finalize

USER $RUNTIME_UID

WORKDIR $OLS_FILES_DIR

EXPOSE 80

EXPOSE 443

CMD ["/usr/local/lsws/bin/litespeed", "-d"]
