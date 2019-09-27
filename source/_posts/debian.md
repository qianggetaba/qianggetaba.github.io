---
title: debian
date: 2019-09-27 09:59:09
tags:
categories:
 - debian
---


[下载镜像](https://cdimage.debian.org/debian-cd/current-live/amd64/bt-hybrid/)

[debian-live-10.1.0-amd64-gnome.iso.torrent](https://cdimage.debian.org/debian-cd/current-live/amd64/bt-hybrid/debian-live-10.1.0-amd64-gnome.iso.torrent)

graphical install

``systemctl get-default``

``systemctl set-default multi-user`` 开机启动到终端，不直接进入gui

``systemctl enable graphical.target --force`` 启动进入gui

``systemctl reboot`` reboot

``su -`` 输入root密码，切换为root, 不加"-",切换后usermod找不到，路径不对

``apt install openssh-server`` sshd

``systemctl enable ssh --now`` 启动并设置为开机启动ssh

``ip -4 addr``

``nano /etc/sudoers`` add ``qgtb    ALL=(ALL:ALL) ALL`` sudo权限

``sudo apt list --upgradable`` 列出可升级的包

安装docker
```
sudo apt-get remove docker docker-engine docker.io containerd runc
sudo apt-get install apt-transport-https ca-certificates curl gnupg2 software-properties-common
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
sudo apt-key fingerprint 0EBFCD88

sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/debian \
   $(lsb_release -cs) \
   stable"

sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
sudo docker run hello-world

sudo usermod -aG docker your-user  # user docker without sudo, need relogin

sudo curl -L "https://github.com/docker/compose/releases/download/1.24.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker-compose --version
# sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```

``sudo apt install netcat``  nc