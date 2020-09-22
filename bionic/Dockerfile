# syntax = docker/dockerfile:experimental
FROM buildpack-deps:bionic

RUN apt-get update \
    && apt-get upgrade -y --no-install-recommends \
    && apt-get install -y --no-install-recommends \
        apache2-utils \
        apt-utils \
        bsdmainutils \
        dnsutils \
        gettext  \
        git-lfs \
        htop \
        iputils-ping \
        jq \
        less \
        locales \
        nano \
        nginx \
        postgresql-client \
        pv \
        python2.7-dev \
        python3.6-dev \
        python3.7-dev \
        screen \
        supervisor \
        tmux \
        vim-tiny \
    && rm -rf rm -rf /var/lib/apt/lists/*

RUN git lfs install --skip-repo --system

ENV DOCKER_PACKAGE_FILENAME=docker-ce_18.06.3~ce~3-0~ubuntu_amd64.deb
RUN --mount=type=cache,mode=0777,target=/var/lib/apt/lists \
    wget -nv "https://download.docker.com/linux/ubuntu/dists/bionic/pool/stable/amd64/${DOCKER_PACKAGE_FILENAME}" \
    && apt-get update \
    && apt install -y --no-install-recommends "./${DOCKER_PACKAGE_FILENAME}" \
    && rm -rf "${DOCKER_PACKAGE_FILENAME}"

ENV DOCKER_COMPOSE_VERSION=1.26.0
RUN wget -nv -O /usr/local/bin/docker-compose "https://github.com/docker/compose/releases/download/${DOCKER_COMPOSE_VERSION}/docker-compose-$(uname -s)-$(uname -m)"
RUN chmod +x /usr/local/bin/docker-compose
RUN wget -nv -O /etc/bash_completion.d/docker-compose "https://raw.githubusercontent.com/docker/compose/${DOCKER_COMPOSE_VERSION}/contrib/completion/bash/docker-compose"

# Do NOT use dockerize 0.6.1 due to https://github.com/jwilder/dockerize/issues/125
ENV DOCKERIZE_VERSION=0.6.0
RUN wget -nv -O - "https://github.com/jwilder/dockerize/releases/download/v${DOCKERIZE_VERSION}/dockerize-linux-amd64-v${DOCKERIZE_VERSION}.tar.gz" | tar -xz -C /usr/local/bin/ -f -

ENV TINI_VERSION=0.19.0
RUN wget -nv -O /usr/local/bin/tini "https://github.com/krallin/tini/releases/download/v${TINI_VERSION}/tini-static"
RUN chmod +x /usr/local/bin/tini

RUN locale-gen en_US.UTF-8
ENV LANG=en_US.UTF-8
ENV LANGUAGE=en_US:en
ENV LC_ALL=en_US.UTF-8

# Indicate that we are running in Docker.
ENV DOCKER=1
