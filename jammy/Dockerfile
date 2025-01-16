# syntax = docker/dockerfile:experimental

# Dockerize releases use a different label for arm64 than ${TARGETARCH}, so we need to
# explicitly set ${DOCKERIZE_ARCH} in a base image for each platform.

FROM buildpack-deps:22.04 as base-amd64
ENV DOCKERIZE_ARCH=amd64
ENV TINI_ARCH=amd64

FROM buildpack-deps:22.04 as base-arm64
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
        python3-dev \
        screen \
        supervisor \
        tmux \
        vim-tiny \
    && rm -rf /var/lib/apt/lists/*

# RUN git lfs install --skip-repo --system

ENV DOCKER_VERSION=20.10.24
RUN --mount=type=cache,id=buildpack-deps-${TARGETARCH},target=/var/lib/apt/lists,sharing=private curl -Ls https://get.docker.com | VERSION="${DOCKER_VERSION}" sh

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

# Hide threads, highlight program "basename", enable tree view.
COPY htoprc /root/.config/htop/htoprc

# Append to bash history.
ENV PROMPT_COMMAND='history -a'
