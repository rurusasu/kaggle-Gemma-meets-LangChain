ARG BASE_IMAGE=ubuntu:22.04
FROM ${BASE_IMAGE}

ARG USER_NAME=user
ARG PROJECT_NAME=gemma
ARG PYTHON_VERSION=3.11
ARG APPLICATION_DIRECTORY=/home/${USER_NAME}/${PROJECT_NAME}

# kaggle APIを利用するためのアカウントデータの仮想環境内での保存先
ARG KAGGLE_CONFIG_DIR="/root/.kaggle/kaggle.json"

# Poetry のインストール
#RUN curl -sSL https://install.python-poetry.org | python3 -
# Poetry を環境変数に追加
#ENV PATH /root/.local/bin:$PATH

# Poetry が作成する仮想環境のディレクトリをプロジェクト直下に変更
# Ref: [Poetry documentation (ver. 1.1.6 日本語訳)]
# (https://cocoatomo.github.io/poetry-ja/configuration/)
#RUN poetry config virtualenvs.in-project true \
#  && poetry new ${PROJECT_NAME}
#  && rm /home/${PROJECT_NAME}/pyproject.toml

# 環境変数の設定
ENV DEBIAN_FRONTEND="noninteractive" \
    TZ="Asia/Tokyo" \
    PYTHONPATH=${APPLICATION_DIRECTORY}

# Package Install
RUN apt-get update && \
    apt-get install --no-install-recommends -y \
        sudo \
        wget \
        curl \
        gnupg \
        unzip

# Python の設定
RUN apt-get update && \
    apt-get install --no-install-recommends -y \
        git \
        make \
        ssh \
        openssh-client \
        software-properties-common && \
    add-apt-repository ppa:deadsnakes/ppa && \
    apt-get update -y && \
    apt-get install --no-install-recommends -y \
        python${PYTHON_VERSION} \
        python-is-python3 && \
    update-alternatives --install /usr/bin/python3 python3 /usr/bin/python${PYTHON_VERSION} 1 && \
    update-alternatives --set python3 /usr/bin/python${PYTHON_VERSION} && \

# pip install
# https://www.bioerrorlog.work/entry/install-pip-pip3-ubuntu
    curl "https://bootstrap.pypa.io/get-pip.py" -o "get-pip.py" && \
    python3 get-pip.py && \
    rm -rf get-pip.py && \

# Poetry install
# Poetry が作成する仮想環境のディレクトリをプロジェクト直下に変更
# Ref: [Poetry documentation (ver. 1.1.6 日本語訳)]
# (https://cocoatomo.github.io/poetry-ja/configuration/)
    python3 -m pip install --upgrade pip setuptools requests && \
    python3 -m pip install poetry && \
    poetry config virtualenvs.in-project true

# 不要なパッケージの削除
RUN apt-get autoremove -y && \
    apt-get -y clean && \
    rm -rf /var/lib/apt/lists/*

WORKDIR ${APPLICATION_DIRECTORY}