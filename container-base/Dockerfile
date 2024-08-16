# FROM ubuntu:22.04
FROM nvcr.io/nvidia/cuda:12.1.1-cudnn8-devel-ubuntu22.04

ARG DEBIAN_FRONTEND=noninteractive
# Time zone
ENV TZ=Asia/Tokyo
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# refer:https://zenn.dev/efficientyk/articles/0fde4dcd4a9520
RUN apt-get update \
    && apt-get install -y \
    git \
    git-lfs \
    curl \
    wget \
    build-essential \
    zlib1g-dev \
    libncurses5-dev \
    libgdbm-dev \
    libbz2-dev \
    libnss3-dev \
    libsqlite3-dev \
    libssl-dev \
    liblzma-dev \
    libreadline-dev \
    libffi-dev \
    libgl1-mesa-dev \
    locales \
    fish \
    vim \
    nano \
    iputils-ping \
    net-tools \
    software-properties-common \
    fonts-powerline \
    less \
    tzdata \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/*

# https://smile-jsp.hateblo.jp/entry/2020/02/11/010753
RUN localedef -f UTF-8 -i en_US en_US.UTF-8
ENV LC_CTYPE="en_US.UTF-8"

# ********************************************************
# * Create the user                                      *
# ********************************************************
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    #
    # [Optional] Add sudo support. Omit if you don't need to install software after connecting.
    && apt-get update \
    && apt-get install -y sudo \
    && apt clean \
    && rm -rf /var/lib/apt/lists/* \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME
RUN chsh -s /bin/bash $USERNAME

USER $USERNAME

# ********************************************************
# python environment                                     *
# ********************************************************
ENV HOME /home/$USERNAME
ENV PYENV_ROOT $HOME/.pyenv
ENV PATH $PYENV_ROOT/bin:$PATH

RUN git clone https://github.com/pyenv/pyenv.git ~/.pyenv && \
    echo 'eval "$(pyenv init --path)"' >> ~/.bashrc && \
    echo 'eval "$(pyenv init -)"' >> ~/.bashrc

RUN pyenv install 3.11.6
# RUN pyenv install 3.10.10
RUN pyenv global 3.11.6


# ********************************************************
# 必要なライブラリのインストール                          *
# ********************************************************

# install node.js npm
USER root
RUN apt-get update \
 && apt-get install -y \
    nodejs npm \
 && npm install n -g \
 && n stable \
 && apt-get purge -y nodejs npm \
 && apt-get autoremove -y \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/*
USER $USERNAME

# flash-attentionをソースコードビルド
# RUN git clone https://github.com/Dao-AILab/flash-attention.git \
#  && cd flash-attention \
#  && MAX_JOBS=4 python setup.py install --user \
#  && cd csrc/layer_norm \
#  && pip install . --user

# RUN LLAMA_CUBLAS=1 CUDA_PATH=/usr/local/cuda FORCE_CMAKE=1 CMAKE_ARGS='-DLLAMA_CUBLAS=on' \
#     pip3 install llama-cpp-python --force-reinstall --upgrade --no-cache-dir -vv

# RUN pip install tokenizers>=0.13.2 prompt_toolkit numpy torch
# RUN pip install rwkv transformers accelerate sentencepiece colorama
# RUN pip install llama-cpp-python==0.2.68 anthropic==0.25.7
# RUN pip install openai==1.29.0
# RUN pip install google-generativeai==0.5.4
# RUN pip install einops bitsandbytes scipy
# RUN pip install numpy pandas matplotlib japanize-matplotlib seaborn
# RUN pip install scikit-learn lightgbm tqdm ipykernel ipywidgets
# RUN pip install openpyxl
# RUN pip install gradio==4.31.1
# RUN pip install qrcode
WORKDIR /workspaces
