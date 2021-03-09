# syntax = docker/dockerfile:experimental

# Dockerize releases use a different label for arm64 than ${TARGETARCH}, so we need to
# explicitly set ${DOCKERIZE_ARCH} in a base image for each platform.

FROM buildpack-deps:focal as base-amd64
ENV DOCKERIZE_ARCH=amd64

FROM buildpack-deps:focal as base-arm64
ENV DOCKERIZE_ARCH=armhf

FROM base-${TARGETARCH}
ARG TARGETARCH

RUN apt-get update \
    && apt-get upgrade -y --no-install-recommends \
    && apt-get install -y --no-install-recommends \
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
        python3.8-dev \
        screen \
        supervisor \
        tmux \
        vim-tiny \
    && rm -rf rm -rf /var/lib/apt/lists/*

# RUN git lfs install --skip-repo --system

# ENV CONTAINERD_FILENAME=containerd.io_1.2.13-2_${TARGETARCH}.deb
# ENV DOCKER_CE_CLI_FILENAME=docker-ce-cli_19.03.12~3-0~ubuntu-focal_${TARGETARCH}.deb
# ENV DOCKER_CE_FILENAME=docker-ce_19.03.12~3-0~ubuntu-focal_${TARGETARCH}.deb
# RUN --mount=type=cache,mode=0777,target=/var/lib/apt/lists \
#     wget -nv "https://download.docker.com/linux/ubuntu/dists/focal/pool/stable/${TARGETARCH}/${CONTAINERD_FILENAME}" \
#     && wget -nv "https://download.docker.com/linux/ubuntu/dists/focal/pool/stable/${TARGETARCH}/${DOCKER_CE_CLI_FILENAME}" \
#     && wget -nv "https://download.docker.com/linux/ubuntu/dists/focal/pool/stable/${TARGETARCH}/${DOCKER_CE_FILENAME}" \
#     && apt-get update \
#     && apt install -y --no-install-recommends "./${CONTAINERD_FILENAME}" "./${DOCKER_CE_CLI_FILENAME}" "./${DOCKER_CE_FILENAME}" \
#     && rm -rf "${CONTAINERD_FILENAME}" "${DOCKER_CE_CLI_FILENAME}" "${DOCKER_CE_FILENAME}"

# ENV DOCKER_COMPOSE_VERSION=1.26.0
# RUN apt-get update \
#     && apt-get install -y --no-install-recommends \
#         python3-pip \
#         python3-setuptools \
#         python3-wheel \
#     && pip3 install "docker-compose==${DOCKER_COMPOSE_VERSION}" \
#     && rm -rf rm -rf /var/lib/apt/lists/*

# ENV DOCKER_COMPOSE_VERSION=1.26.0
# RUN wget -nv -O /usr/local/bin/docker-compose "https://github.com/docker/compose/releases/download/${DOCKER_COMPOSE_VERSION}/docker-compose-$(uname -s)-$(uname -m)"
# RUN chmod +x /usr/local/bin/docker-compose
# RUN wget -nv -O /etc/bash_completion.d/docker-compose "https://raw.githubusercontent.com/docker/compose/${DOCKER_COMPOSE_VERSION}/contrib/completion/bash/docker-compose"

# Do NOT use dockerize 0.6.1 due to https://github.com/jwilder/dockerize/issues/125
ENV DOCKERIZE_VERSION=0.6.0
RUN wget -nv -O - "https://github.com/jwilder/dockerize/releases/download/v${DOCKERIZE_VERSION}/dockerize-linux-${DOCKERIZE_ARCH}-v${DOCKERIZE_VERSION}.tar.gz" | tar -xz -C /usr/local/bin/ -f -

ENV TINI_VERSION=0.19.0
RUN wget -nv -O /usr/local/bin/tini "https://github.com/krallin/tini/releases/download/v${TINI_VERSION}/tini-static"
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
