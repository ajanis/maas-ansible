FROM mcr.microsoft.com/devcontainers/base:jammy

ARG DEBIAN_FRONTEND=noninteractive
ARG USER=vscode
ARG HOME="/home/${USER}"
ARG PYENV_ROOT="${HOME}/.pyenv"

ENV PYTHON_VERSION="3.12"
ENV VENV_NAME="maas-ansible"
ENV TERM="xterm"
ENV SHELL="/bin/zsh"
ENV PATH="${PYENV_ROOT}/shims:${PYENV_ROOT}/bin:${HOME}/.local/bin:$PATH"

RUN DEBIAN_FRONTEND=noninteractive apt-get update --no-install-recommends \
    && apt-get install -y build-essential make autoconf patch texinfo \
    gcc jq git vim wget curl yodl groff xz-utils ca-certificates man-db \
    libncursesw5-dev libpcre3-dev libgdbm-dev libssl-dev libonig-dev \
    zlib1g-dev libreadline-dev libbz2-dev libsqlite3-dev libcap-dev \
    libxml2-dev libxmlsec1-dev libffi-dev libtinfo5 liblzma-dev libpq-dev \
    musl-dev libonig5 libtool inetutils-ping dnsutils bind9-dnsutils \
    docker.io redis redis-sentinel mypy

WORKDIR "${LOCAL_WORKSPACE_FOLDER}"

COPY --chown="${USER}" rcfiles/.zshrc "${HOME}/.zshrc"
COPY --chown="${USER}" rcfiles/.p10k.zsh "${HOME}/.p10k.zsh"
COPY --chown="${USER}" pipfiles/ansible.txt "/tmp/ansible.txt"
COPY --chown="${USER}" pipfiles/requirements.txt "/tmp/requirements.txt"

RUN curl -k https://pyenv.run | bash
RUN pyenv install ${PYTHON_VERSION} \
    && pyenv virtualenv ${PYTHON_VERSION} ${VENV_NAME} \
    && pyenv global ${VENV_NAME} \
    && pyenv local ${VENV_NAME} \
    && echo "${VENV_NAME}" > "${WORKDIR}/.python-version"
RUN pip install --upgrade -r /tmp/ansible.txt \
    && pip install --upgrade  --no-cache-dir -r /tmp/requirements.txt \
    && chown -R ${USER}:${USER} ${PYENV_ROOT}
RUN git clone https://github.com/romkatv/powerlevel10k.git ${HOME}/.oh-my-zsh/custom/themes/powerlevel10k \
    && chown -R ${USER}:${USER} ${HOME}
RUN rm -f /tmp/requirements.txt /tmp/ansible.txt


# DOCKER INIT
ENTRYPOINT []
CMD [ "/bin/zsh" ]