FROM mcr.microsoft.com/devcontainers/base:jammy

ARG DEBIAN_FRONTEND=noninteractive
ARG USER=vscode
ARG HOME="/home/${USER}"
ARG PYENV_ROOT="${HOME}/.pyenv"
ENV PYTHON_VERSION="3.12"
ENV TERM="xterm"
ENV SHELL="/bin/zsh"
ENV PATH="${PYENV_ROOT}/shims:${PYENV_ROOT}/bin:${HOME}/.local/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:$PATH"

RUN DEBIAN_FRONTEND=noninteractive sudo apt-get update --no-install-recommends \
    && sudo apt-get install --no-install-recommends -y build-essential make autoconf patch texinfo \
    gcc jq git vim wget curl yodl groff xz-utils ca-certificates man-db \
    libncursesw5-dev libpcre3-dev libgdbm-dev libssl-dev libonig-dev \
    zlib1g-dev libreadline-dev libbz2-dev libsqlite3-dev libcap-dev \
    libxml2-dev libxmlsec1-dev libffi-dev libtinfo5 liblzma-dev libpq-dev \
    musl-dev libonig5 libtool inetutils-ping dnsutils bind9-dnsutils \
    docker.io redis redis-sentinel libgtk-3-dev \
    && sudo rm -rf /var/lib/apt/lists/*


WORKDIR "/workspaces/${localWorkspaceFolderBasename}"

COPY --chown="${USER}" rcfiles/.zshrc "${HOME}/.zshrc"
COPY --chown="${USER}" rcfiles/.p10k.zsh "${HOME}/.p10k.zsh"
COPY --chown="${USER}" pipfiles/ansible.txt "/tmp/ansible.txt"
COPY --chown="${USER}" pipfiles/requirements.txt "/tmp/requirements.txt"

RUN curl --insecure -f -k https://pyenv.run | bash \
    && pyenv install ${PYTHON_VERSION} \
    && pyenv global ${PYTHON_VERSION} \
    && pyenv local ${PYTHON_VERSION} \
    && echo "${PYTHON_VERSION}" > "${WORKDIR}/.python-version" \
    && pip install --upgrade -r /tmp/ansible.txt \
    && pip install --upgrade  --no-cache-dir -r /tmp/requirements.txt \
    && sudo chown -R ${USER}:${USER} ${PYENV_ROOT} \
    && git -c http.sslVerify=false clone https://github.com/romkatv/powerlevel10k.git ${HOME}/.oh-my-zsh/custom/themes/powerlevel10k \
    && sudo chown -R ${USER}:${USER} ${HOME}

# DOCKER INIT
USER vscode
ENTRYPOINT []
CMD [ "/bin/zsh" ]
