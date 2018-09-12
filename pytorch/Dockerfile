From ubuntu:latest
MAINTAINER Kato Ryuichi

ENV http_proxy  http://aaa109803:19881231@150.61.8.47:10080/
ENV https_proxy http://aaa109803:19881231@150.61.8.47:10080/

# apt-get設定
RUN apt-get  update \
 && apt-get -y install --no-install-recommends unattended-upgrades \
# 日本語設定
 language-pack-ja \
# 各種エディタ
 nano \
# Python
 python3-setuptools python3-dev python3-pip \
# その他あると便利なもの
 git tree locate wget curl sl \
# sudoは最後にインストールする
 sudo

# bashの補完
RUN apt-get install -y bash-completion \
 && curl https://raw.githubusercontent.com/docker/docker-ce/master/components/cli/contrib/completion/bash/docker -o /etc/bash_completion.d/docker.sh

# aptの後処理
RUN unattended-upgrades \
 && apt-get purge -y unattended-upgrades \
 && apt-get autoremove -y \
 && apt-get clean

#pythonコマンドとpipコマンドのシンボリックリンクを作成
RUN pip3 install -U pip \
# pipアップデート後に発生するエラーを抑制するため
 && hash -r pip3 \
# 必要なライブラリをインストール
 && pip3 install torch torchvision opencv-python matplotlib

#使用言語を日本語に設定
ENV LANG ja_JP.UTF-8

# Display設定
ARG uid=1000
ARG gid=1000
ARG uname="pytorch"
ARG passwd="pytorch"

RUN groupadd -g ${gid} ${uname} \
 && useradd  -g ${uid} -G sudo -m -s /bin/bash ${uname}

RUN echo 'root:'${passwd} | chpasswd \
 && echo ${uname}':'${passwd} | chpasswd \
 && echo 'Defaults visiblepw'              >> /etc/sudoers \
 && echo ${uname}' ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

USER ${uname}
WORKDIR /home/${uname}
ADD .bashrc /home/${uname}

# Gitのサンプル
RUN mkdir /home/${uname}/Git \
 && git clone https://github.com/ka10ryu1/pytorch_test.git /home/${uname}/Git/pytorch_test/

RUN sudo rm -rf \
# /var/lib/apt/lists/* \
# /var/cache/apt/archives/* \
# .cache/pip/* \
 /home/${uname}/Download/*

