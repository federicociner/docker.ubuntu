# ubuntu-xenial Dockerfile
FROM ubuntu:xenial
MAINTAINER Federico Ciner <federico.ciner@gmail.com>

## shutup debconf
ARG DEBIAN_FRONTEND=noninteractive
ENV TZ=Australia/Sydney
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

COPY files/* /tmp/

## install tools and utilities
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    apt-utils \
    wget \
    vim \
    curl \
    htop \
    unzip \
    git \
    less \
    screen \
    libaio1 \
    libssl-dev \
    openjdk-9-jdk-headless \
    libcurl4-openssl-dev \
    libltdl3-dev \
    odbcinst1debian2 \
    unixodbc \
    unixodbc-dev \
    unixodbc-bin \
    && dpkg -i /tmp/tini_0.13.0-amd64.deb \
    && rm /tmp/tini_0.13.0-amd64.deb \
    && tar -zxvf /tmp/mysql-connector-odbc-5.3.8-linux-ubuntu16.04-x86-64bit.tar.gz -C /tmp/ \
    && mv /tmp/mysql-connector-odbc-5.3.8-linux-ubuntu16.04-x86-64bit/lib/* /usr/lib/x86_64-linux-gnu/odbc/ \
    && cat /tmp/custom_odbcinst.ini >> /etc/odbcinst.ini

## install MSSQL ODBC drivers
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
    ## clean up any files used by apt-get
    && apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

## run some tests
ENTRYPOINT ["tini", "--"]

## passing `-l` flag to bash, in order to enable login shell (and source all the `profile.d/*` stuff)
CMD ["/bin/bash", "-l"]