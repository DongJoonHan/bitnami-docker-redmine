# Dockerfile for X
# Revision 3
# 2021.03

FROM docker.io/bitnami/minideb:buster
#LABEL maintainer "Bitnami <containers@bitnami.com>"
LABEL maintainer "Synetics"

ENV PATH="/opt/bitnami/ruby/bin:/opt/bitnami/postgresql/bin:/opt/bitnami/mysql/bin:/opt/bitnami/git/bin:/opt/bitnami/common/bin:/opt/bitnami/nami/bin:$PATH"

COPY prebuildfs /
# Install required system packages and dependencies
RUN install_packages ca-certificates curl ghostscript gsfonts gzip imagemagick libaudit1 libbsd0 libc6 libcap-ng0 libcom-err2 libcurl4 libedit2 libffi6 libgcc1 libgcrypt20 libgmp-dev libgmp10 libgnutls30 libgpg-error0 libgssapi-krb5-2 libhogweed4 libicu63 libidn2-0 libjemalloc2 libk5crypto3 libkeyutils1 libkrb5-3 libkrb5support0 libldap-2.4-2 liblzma5 libmariadb3 libncurses6 libnettle6 libnghttp2-14 libp11-kit0 libpam0g libpq5 libpsl5 libreadline-dev libreadline7 librtmp1 libsasl2-2 libssh2-1 libssl-dev libssl1.1 libstdc++6 libtasn1-6 libtinfo6 libunistring2 libuuid1 libxml2 libxml2-dev libxslt1-dev libxslt1.1 procps sqlite3 sudo tar zlib1g zlib1g-dev
RUN /build/bitnami-user.sh
RUN /build/install-nami.sh
RUN bitnami-pkg install ruby-2.6.6-2 --checksum 4a1c68af2b652fd873124ebb00ec79574007105730059539b1d3223bbea2af34
RUN bitnami-pkg install postgresql-client-11.11.0-0 --checksum 648e3913acf696c24143dfca8081d22f655f6a70abc542f5daf3f3cd9aa8e2e9
RUN bitnami-pkg install mysql-client-10.3.28-0 --checksum 9398376ca9e2033d5bc193232e8aa9b57d91d4ccf06fa67bfa0d30ef36e44c25
RUN bitnami-pkg install git-2.31.1-0 --checksum 08cc25cd961bf4ea1695b9d11cd4dc0ebb2f4a00afd6cacbc18c81d6cd104aa2
RUN bitnami-pkg install tini-0.19.0-1 --checksum 9b1f1c095944bac88a62c1b63f3bff1bb123aa7ccd371c908c0e5b41cec2528d
RUN bitnami-pkg unpack redmine-4.2.0-0 --checksum 756e26c6a3bd03f3353c9bc1555245d584962ff4da2ecaa2e0f1937a49e7768a
RUN bitnami-pkg install gosu-1.12.0-2 --checksum 4d858ac600c38af8de454c27b7f65c0074ec3069880cb16d259a6e40a46bbc50

COPY rootfs /
ENV BITNAMI_APP_NAME="redmine" \
    BITNAMI_IMAGE_VERSION="4.2.0-debian-10-r0" \
    OS_ARCH="amd64" \
    OS_FLAVOUR="debian-10" \
    OS_NAME="linux" \
    REDMINE_DB_MYSQL="mariadb" \
    REDMINE_DB_NAME="bitnami_redmine" \
    REDMINE_DB_PASSWORD="" \
    REDMINE_DB_PORT_NUMBER="" \
    REDMINE_DB_POSTGRES="" \
    REDMINE_DB_USERNAME="bn_redmine" \
    REDMINE_EMAIL="user@example.com" \
    REDMINE_LANGUAGE="en" \
    REDMINE_PASSWORD="bitnami1" \
    REDMINE_USERNAME="user" \
    SMTP_AUTHENTICATION="login" \
    SMTP_DOMAIN="example.com" \
    SMTP_HOST="" \
    SMTP_PASSWORD="" \
    SMTP_PORT="" \
    SMTP_TLS="true" \
    SMTP_USER=""

EXPOSE 3000

# 필요한 OS 패키지 설치
RUN         apt-get -y update && \
            apt-get install -y -qq  build-essential libstdc++6 vim libmariadbclient-dev-compat postgresql-client libpq5 libpq-dev libmagickwand-dev libssl-dev cmake libssh2-1 libssh2-1-dev libgpg-error-dev openssh-client openssh-server git perl && \
            rm -rf /var/lib/apt/lists/*

COPY        plugins /opt/bitnami/redmine/plugins
WORKDIR     /opt/bitnami/redmine
RUN	    bundle config unset deployment
RUN         bundle install --no-deployment

## Timezone 설정
ENV TZ=Asia/Seoul
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

ENTRYPOINT [ "/app-entrypoint.sh" ]
CMD [ "/run.sh" ]
