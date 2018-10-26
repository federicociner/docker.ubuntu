FROM ubuntu:xenial
MAINTAINER Federico Ciner <federico.ciner@gmail.com>

## Silence debconf
ARG DEBIAN_FRONTEND=noninteractive
ENV TZ=Australia/Sydney
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

COPY files/* /tmp/

## Install tools and utilities
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    build-essential \
    apt-utils \
    git \
    ed \
    wget \
    vim \
    curl \
    htop \
    unzip \
    locales \
    ca-certificates \
    apt-transport-https \
    less \
    screen \
    libaio1 \
    libssl-dev \
    openjdk-9-jdk-headless \
    g++ \
    gcc \
    imagemagick \
    libltdl3-dev \
    libbz2-dev \
    libc6-dev \
    libcurl4-openssl-dev \
    libdb-dev \
    libevent-dev \
    libffi-dev \
    libgdbm-dev \
    libgeoip-dev \
    libglib2.0-dev \
    libjpeg-dev \
    libkrb5-dev \
    liblzma-dev \
    libmagickcore-dev \
    libmagickwand-dev \
    libncurses5-dev \
    libncursesw5-dev \
    libpng-dev \
    libpq-dev \
    libreadline-dev \
    libsqlite3-dev \
    libssl-dev \
    libtool \
    libwebp-dev \
    libxml2-dev \
    libxslt-dev \
    libyaml-dev \
    odbcinst1debian2 \
    unixodbc \
    unixodbc-dev \
    unixodbc-bin \
    && dpkg -i /tmp/tini_0.13.0-amd64.deb \
    && rm /tmp/tini_0.13.0-amd64.deb \
    && tar -zxvf /tmp/mysql-connector-odbc-5.3.8-linux-ubuntu16.04-x86-64bit.tar.gz -C /tmp/ \
    && mv /tmp/mysql-connector-odbc-5.3.8-linux-ubuntu16.04-x86-64bit/lib/* /usr/lib/x86_64-linux-gnu/odbc/ \
    && cat /tmp/custom_odbcinst.ini >> /etc/odbcinst.ini

## Install MSSQL ODBC drivers
RUN apt-get update \
    && apt-get install apt-transport-https \
    && curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add - \
    && curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > /etc/apt/sources.list.d/mssql-release.list \
    && apt-get update \
    && apt-get install locales \
    && locale-gen en_US.utf8 && update-locale \
    && ACCEPT_EULA=Y apt-get install -y msodbcsql mssql-tools \
    && ln -sfn /opt/mssql-tools/bin/sqlcmd /usr/bin/sqlcmd \
    && ln -sfn /opt/mssql-tools/bin/bcp /usr/bin/bcp \
    && rm /etc/apt/sources.list.d/mssql-release.list \
    && apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

## Run some tests
ENTRYPOINT ["tini", "--"]

## Passing `-l` flag to bash, in order to enable login shell
CMD ["/bin/bash", "-l"]