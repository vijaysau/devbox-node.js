FROM almalinux:minimal

# Upgrade and install useful utilities; install development tools
RUN microdnf upgrade -y \
    && microdnf install -y gzip hostname less nano procps-ng shadow-utils tar which \
    && microdnf install -y libatomic \
    && microdnf clean all

LABEL org.opencontainers.image.title="devbox-nodejs" \
    org.opencontainers.image.description="A devcontainer for Node.js" \
    org.opencontainers.image.created="2026-10-18" \
    org.opencontainers.image.version="1.0.0" \
    org.opencontainers.image.vendor="Vijay Subramanian" \
    org.opencontainers.image.authors="Vijay Subramanian" \
    org.opencontainers.image.licenses="Apache-2.0"

WORKDIR /node-js-repos

# Define a non-root user
ARG USERNAME=rocket
ARG USER_UID=1001
ARG USER_GID=$USER_UID

RUN groupadd --gid ${USER_GID} ${USERNAME} \
    && useradd --uid ${USER_UID} --gid ${USER_GID} --create-home ${USERNAME} \
    && echo 'export PS1="\[\033[0;36m\]\u\[\033[0;35m\]@\[\033[0;34m\]\h \[\033[0;32m\]\w\n\[\033[0;35m\]\\$\[\033[0m\] "' >> /etc/bashrc

USER ${USER_UID}

# Get and install Node.js
ENV COREPACK_ENABLE_DOWNLOAD_PROMPT=0
RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/master/install.sh | sh \
    && source ~/.bashrc \
    && nvm install node \
    && nvm install-latest-npm \
    && ${HOME}/.nvm/versions/node/$(node --version)/bin/npm install -g corepack \
    && corepack enable yarn