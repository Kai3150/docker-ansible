FROM ubuntu:22.04

RUN apt-get update && apt-get install -y sudo openssh-server \
    && rm -rf /var/lib/apt/lists/*

# SSHセットアップ
RUN mkdir /var/run/sshd

# ansibleユーザ作成
RUN useradd -m ansible && echo "ansible:ansible" | chpasswd \
    && usermod -aG sudo ansible \
    && echo "ansible ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

# 公開鍵設定
COPY authorized_keys /home/ansible/.ssh/authorized_keys
RUN chown ansible:ansible /home/ansible/.ssh -R && chmod 700 /home/ansible/.ssh && chmod 600 /home/ansible/.ssh/authorized_keys

# SSHサーバ設定：パスワード認証有効化
RUN sed -i 's/#PasswordAuthentication yes/PasswordAuthentication yes/' /etc/ssh/sshd_config
RUN sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config

EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"]
