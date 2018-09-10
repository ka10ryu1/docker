From ubuntu:latest
MAINTAINER Kato Ryuichi

# apt-get設定
RUN apt-get  update \
 && apt-get -y install --no-install-recommends unattended-upgrades \
# 日本語設定
 language-pack-ja \
# 各種エディタ
 emacs nano \
# GUI
 x11-apps x11-xserver-utils \
# Python
 python3-setuptools python3-dev python3-pip \
 python-setuptools python-dev python-pip \
# emacs-mozc用
 libgail-common libgail18 libgtk2.0-0 libgtk2.0-bin libgtk2.0-common libprotobuf10 \
# その他あると便利なもの
 git tree locate wget curl \
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
 && pip install -U pip \
# pipアップデート後に発生するエラーを抑制するため
 && hash -r pip3 \
 && hash -r pip \
# 必要なライブラリをインストール
 && pip3 install chainer opencv-python matplotlib \
 && pip install autopep8 pyflakes virtualenv jedi epc

#使用言語を日本語に設定
ENV LANG ja_JP.UTF-8

# Display設定
ARG uid=1000
ARG gid=1000
ARG uname="tester"
ARG passwd="tester"

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
 && git clone https://github.com/ka10ryu1/camera_test.git /home/${uname}/Git/camera_test/

# Emacsの設定
ARG emacs=/home/${uname}/Download/Emacs
RUN mkdir -p /home/${uname}/.emacs.d /home/${uname}/Download /home/${uname}/.local/share/fonts \
 && git clone https://github.com/ka10ryu1/Emacs.git ${emacs} \
 && git clone https://github.com/zk-phi/git-complete.git/ /home/${uname}/Download/git-complete/ \
 && cp -r ${emacs}/init.el ${emacs}/custom/ ${emacs}/snippets/ /home/${uname}/.emacs.d/ \
 && cp /home/${uname}/Download/git-complete/git-complete.el /home/${uname}/.emacs.d/custom/ \
 && tar Jxfv ${emacs}/fonts_noemoji.tar.xz -C ${emacs} \
# && xz -dc ${emacs}/fonts_noemoji.tar.xz | tar xfv - -C ${emacs} \
 && cp ${emacs}/fonts_noemoji/Cica* /home/${uname}/.local/share/fonts/ \
 && fc-cache -fv

ADD ibus-mozc-ubuntu18.04/emacs-* ibus-mozc-ubuntu18.04/mozc-server* /home/${uname}/Download/
RUN sudo -E dpkg -i /home/${uname}/Download/*.deb

RUN sudo rm -rf \
# /var/lib/apt/lists/* \
# /var/cache/apt/archives/* \
# .cache/pip/* \
 /home/${uname}/Download/*

