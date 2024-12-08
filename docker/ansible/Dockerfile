FROM ubuntu:22.04

ENV ANSIBLE_HOST_KEY_CHECKING False
ENV DEBIAN_FRONTEND=noninteractive

# 必要パッケージのインストール
RUN apt-get update && apt-get install -y \
    ansible ssh sudo python3-pip vim \
    && rm -rf /var/lib/apt/lists/*

# ansibleユーザ作成
RUN useradd -m ansible && echo "ansible ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

# SSHクライアントの設定
USER ansible
WORKDIR /var/data

# ローカルで作成した秘密鍵・公開鍵をコンテナにコピー（本番では注意）
USER root
COPY id_rsa /home/ansible/.ssh/id_rsa
COPY id_rsa.pub /home/ansible/.ssh/id_rsa.pub
RUN chown ansible:ansible /home/ansible/.ssh/id_rsa* && \
    chmod 600 /home/ansible/.ssh/id_rsa

USER ansible
