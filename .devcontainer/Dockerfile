FROM ubuntu:latest

ARG DEBIAN_FRONTEND=noninteractive
ENV TZ=Asia/Tokyo

RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y build-essential libbz2-dev libdb-dev \
      libreadline-dev libffi-dev libgdbm-dev liblzma-dev \
      libncursesw5-dev libsqlite3-dev libssl-dev \
      zlib1g-dev uuid-dev tk-dev \
      sudo git vim wget sudo locales apt-utils curl lsb-release iputils-ping iproute2 && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* && \
    apt-get update

RUN wget https://www.python.org/ftp/python/3.11.3/Python-3.11.3.tgz && \
    tar -xf Python-3.11.3.tgz && \
    cd Python-3.11.3 && \
    ./configure --enable-optimizations && \
    make -j $(nproc) | tee build.log && \
    sudo make altinstall

RUN /usr/local/bin/pip3.11 install autopep8 black yapf bandit flake8 mypy pycodestyle pydocstyle pylint

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
RUN apt-get install bash-completion
USER vscode
RUN curl -sSL https://raw.githubusercontent.com/python-poetry/poetry/master/install-poetry.py | python3.11 -
RUN echo "export PATH=/home/vscode/.local/bin:$PATH" >> /home/vscode/.bashrc
RUN /home/vscode/.local/bin/poetry completions bash | sudo tee /etc/bash_completion.d/poerty.bash-completion > /dev/null
