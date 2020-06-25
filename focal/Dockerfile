FROM buildpack-deps:focal

RUN apt-get update \
    && apt-get upgrade -y --no-install-recommends \
    && apt-get install -y --no-install-recommends \
        apache2-utils \
        apt-utils \
        bsdmainutils \
        dnsutils \
        gettext  \
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
    && rm -rf /var/lib/apt/lists/*

ENV CONTAINERD_FILENAME=containerd.io_1.2.13-2_amd64.deb
ENV DOCKER_CE_CLI_FILENAME=docker-ce-cli_19.03.12~3-0~ubuntu-focal_amd64.deb
ENV DOCKER_CE_FILENAME=docker-ce_19.03.12~3-0~ubuntu-focal_amd64.deb
RUN wget -nv "https://download.docker.com/linux/ubuntu/dists/focal/pool/stable/amd64/${CONTAINERD_FILENAME}" \
    && wget -nv "https://download.docker.com/linux/ubuntu/dists/focal/pool/stable/amd64/${DOCKER_CE_CLI_FILENAME}" \
    && wget -nv "https://download.docker.com/linux/ubuntu/dists/focal/pool/stable/amd64/${DOCKER_CE_FILENAME}" \
    && apt-get update \
    && apt install -y --no-install-recommends "./${CONTAINERD_FILENAME}" "./${DOCKER_CE_CLI_FILENAME}" "./${DOCKER_CE_FILENAME}" \
    && rm -rf /var/lib/apt/lists/* "${CONTAINERD_FILENAME}" "${DOCKER_CE_CLI_FILENAME}" "${DOCKER_CE_FILENAME}"

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
