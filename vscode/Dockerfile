From ubuntu:latest
MAINTAINER Kato Ryuichi

# proxy設定
ENV http_proxy  http://[ID]:[pass]@150.61.8.47:10080/
ENV https_proxy http://[ID]:[pass]@150.61.8.47:10080/

RUN set -x \
# apt-get設定
 && apt-get  update \
 && apt-get -y install --no-install-recommends unattended-upgrades \
# 日本語設定
 language-pack-ja \
# 各種エディタ
 nano \
# GUI
 x11-apps x11-xserver-utils \
# Python
 python3-setuptools python3-dev python3-pip \
# VS Code
libnotify4 libnss3 gnupg libgconf-2-4 libsecret-1-0 libxss1 libasound2 libgtk-3-0 libxtst6 \
# bash補完
 bash-completion \
# その他あると便利なもの
 git tree locate wget curl \
# sudoは最後にインストールする
 sudo \
# bash補完の設定ファイルをダウンロード
 && curl https://raw.githubusercontent.com/docker/docker-ce/master/components/cli/contrib/completion/bash/docker -o /etc/bash_completion.d/docker.sh \
 && wget -q https://go.microsoft.com/fwlink/?LinkID=760868 -O /tmp/vscode-amd64.deb \
 && dpkg -i /tmp/vscode-amd64.deb \
 && apt-get install -f \
 && rm -rf /tmp/vscode-amd64.deb \
# aptの後処理
 && unattended-upgrades \
 && apt-get purge -y unattended-upgrades \
 && apt-get autoremove -y \
 && apt-get clean \
 && rm /etc/apt/apt.conf.d/docker-clean \
 && apt-get update \
 && apt-get upgrade -y \
#pythonコマンドとpipコマンドのシンボリックリンクを作成
 && pip3 install -U pip \
# pipアップデート後に発生するエラーを抑制するため
 && hash -r pip3 \
# 必要なライブラリをインストール
# && pip3 install chainer opencv-python matplotlib \
 && pip3 install pylint autopep8

#使用言語を日本語に設定
ENV LANG ja_JP.UTF-8

# Display設定
ARG uid=1000
ARG gid=1000
ARG uname="tester"
ARG passwd="tester"

RUN groupadd -g ${gid} ${uname} \
 && useradd  -g ${uid} -G sudo -m -s /bin/bash ${uname} \
 && echo 'root:'${passwd} | chpasswd \
 && echo ${uname}':'${passwd} | chpasswd \
 && echo 'Defaults visiblepw'              >> /etc/sudoers \
 && echo ${uname}' ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

USER ${uname}
WORKDIR /home/${uname}
ADD .bashrc /home/${uname}
ADD .nanorc /home/${uname}

# Gitのサンプル
RUN mkdir /home/${uname}/Git \
 && git clone https://github.com/ka10ryu1/camera_test.git /home/${uname}/Git/camera_test/

ARG emacs=/home/${uname}/Download/Emacs/
ARG fonts=/home/${uname}/.local/share/fonts

RUN mkdir -p ${fonts} ${emacs} \
 && git clone https://github.com/ka10ryu1/Emacs.git ${emacs} \
 && tar Jxfv ${emacs}/fonts_noemoji.tar.xz -C ${emacs} \
 && cp ${emacs}/fonts_noemoji/Cica* ${fonts} \
 && fc-cache -fv \
 && mkdir -p /home/${uname}/.config/Code/User/ /home/${uname}/.vscode/

ADD ./user_setting/settings.json /home/${uname}/.config/Code/User/
ADD ./workspace_setting/settings.json /home/${uname}/.vscode/
ADD ./extension/extensions.json /home/${uname}/.vscode/

RUN sudo rm -rf \
# /var/lib/apt/lists/* \
# /var/cache/apt/archives/* \
# .cache/pip/* \
 /home/${uname}/Download/*

