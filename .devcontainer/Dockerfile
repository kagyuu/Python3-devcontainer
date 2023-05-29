FROM ubuntu:latest

ARG DEBIAN_FRONTEND=noninteractive
ENV TZ=Asia/Tokyo

RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y build-essential libbz2-dev libdb-dev \
      libreadline-dev libffi-dev libgdbm-dev liblzma-dev \
      libncursesw5-dev libsqlite3-dev libssl-dev \
      zlib1g-dev uuid-dev tk-dev \
      sudo git vim wget sudo locales apt-utils curl lsb-release iputils-ping iproute2 bash-completion && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* && \
    apt-get update

ARG PYTHON_VERSION
ARG PYTHON_REVISION
RUN apt-get install -y intel-mkl postgresql-client mysql-client
RUN wget https://www.python.org/ftp/python/${PYTHON_VERSION}.${PYTHON_REVISION}/Python-${PYTHON_VERSION}.${PYTHON_REVISION}.tgz && \
    tar -xf Python-${PYTHON_VERSION}.${PYTHON_REVISION}.tgz && \
    cd Python-${PYTHON_VERSION}.${PYTHON_REVISION} && \
    ./configure --enable-optimizations && \
    make -j $(nproc) | tee build.log && \
    sudo make altinstall

RUN /usr/local/bin/pip${PYTHON_VERSION} install --upgrade pip && \
    /usr/local/bin/pip${PYTHON_VERSION} install autopep8 black yapf bandit flake8 mypy pycodestyle pydocstyle pylint

ARG USERNAME=vscode
ARG GROUPNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

RUN groupadd -g 1000 ${GROUPNAME} && \
    useradd -m -s /bin/bash -u ${USER_UID} -g ${USER_GID} ${USERNAME} && \
    echo ${USERNAME} ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/${USERNAME} && \
    chmod 0440 /etc/sudoers.d/${USERNAME} && \
    localedef -f UTF-8 -i ja_JP ja_JP.UTF-8

# install poerty for vscode user
USER vscode
ARG WORKDIR
RUN curl -sSL https://raw.githubusercontent.com/python-poetry/poetry/master/install-poetry.py | python${PYTHON_VERSION} -
RUN echo "export PATH=/home/vscode/.local/bin:$PATH" >> /home/vscode/.bashrc
RUN /home/vscode/.local/bin/poetry completions bash | sudo tee /etc/bash_completion.d/poerty.bash-completion > /dev/null
RUN /home/vscode/.local/bin/poetry config cache-dir /workspaces/${WORKDIR}/.cache
