# syntax = docker/dockerfile:experimental

# Dockerize releases use a different label for arm64 than ${TARGETARCH}, so we need to
# explicitly set ${DOCKERIZE_ARCH} in a base image for each platform.

FROM buildpack-deps:bionic as base-amd64
ENV DOCKERIZE_ARCH=amd64
ENV TINI_ARCH=amd64

FROM buildpack-deps:bionic as base-arm64
ENV DOCKERIZE_ARCH=armhf
ENV TINI_ARCH=arm64

FROM base-${TARGETARCH}
ARG TARGETARCH

RUN apt-get update \
    && apt-get upgrade -y --no-install-recommends \
    && apt-get install -y --no-install-recommends \
        atop \
        apache2-utils \
        apt-utils \
        bsdmainutils \
        dnsutils \
        gettext  \
        `# git-lfs` \
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

# RUN git lfs install --skip-repo --system

# Install Docker.
ENV DOCKER_VERSION=20.10.5
RUN --mount=type=cache,id=buildpack-deps-${TARGETARCH},target=/var/lib/apt/lists,sharing=private curl -Ls https://get.docker.com | VERSION="${DOCKER_VERSION}" sh

# Install a shim that runs docker-compose in a container, for ARM64 compatibility.
# See: https://hub.docker.com/r/linuxserver/docker-compose
ENV DOCKER_COMPOSE_IMAGE_TAG=ghcr.io/linuxserver/docker-compose:version-1.28.5
RUN curl -L --fail https://raw.githubusercontent.com/linuxserver/docker-docker-compose/master/run.sh -o /usr/local/bin/docker-compose \
    && chmod +x /usr/local/bin/docker-compose

# Do NOT use dockerize 0.6.1 due to https://github.com/jwilder/dockerize/issues/125
ENV DOCKERIZE_VERSION=0.6.0
RUN wget -nv -O - "https://github.com/jwilder/dockerize/releases/download/v${DOCKERIZE_VERSION}/dockerize-linux-${DOCKERIZE_ARCH}-v${DOCKERIZE_VERSION}.tar.gz" | tar -xz -C /usr/local/bin/ -f -

ENV TINI_VERSION=0.19.0
RUN wget -nv -O /usr/local/bin/tini "https://github.com/krallin/tini/releases/download/v${TINI_VERSION}/tini-static-${TINI_ARCH}"
RUN chmod +x /usr/local/bin/tini

RUN locale-gen en_US.UTF-8
ENV LANG=en_US.UTF-8
ENV LANGUAGE=en_US:en
ENV LC_ALL=en_US.UTF-8

# Indicate that we are running in Docker.
ENV DOCKER=1

# Enable persistent bash history.
VOLUME /root/var
ENV HISTFILE=/root/var/.bash_history
ENV PROMPT_COMMAND='history -a'
