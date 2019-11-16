---
title: debian
date: 2019-09-27 09:59:09
tags:
categories:
 - debian
---


[下载镜像](https://cdimage.debian.org/debian-cd/current-live/amd64/bt-hybrid/)

[debian-live-10.1.0-amd64-gnome.iso.torrent](https://cdimage.debian.org/debian-cd/current-live/amd64/bt-hybrid/debian-live-10.1.0-amd64-gnome.iso.torrent)

[debian-live-10.1.0-amd64-standard.iso.torrent](https://cdimage.debian.org/debian-cd/current-live/amd64/bt-hybrid/debian-live-10.1.0-amd64-standard.iso.torrent)没有桌面，826M

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

```
apt Could not get lock /var/lib/dpkg/lock-frontend - open (11: Resource temporarily unavailable)

ps aux | grep -i apt
systemctl stop apt-daily.service
systemctl kill --kill-who=all apt-daily.service

sudo killall apt apt-get
sudo rm /var/lib/apt/lists/lock
sudo rm /var/cache/apt/archives/lock
sudo rm /var/lib/dpkg/lock*
sudo dpkg --configure -a
sudo apt update
```

``apt-cache show gnome-core | grep ^Depends`` 依赖的包
``apt-cache --installed rdepends firefox-esr`` 依赖于这个包的包

```
sudo apt-get install software-properties-common # for add-apt-repository
sudo add-apt-repository "deb http://ppa.launchpad.net/webupd8team/java/ubuntu xenial main"
sudo apt-get update
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 40976EAF437D05B5
```


```
sudo apt-get install open-vm-tools
sudo systemctl reboot
/usr/bin/vmware-toolbox-cmd -v
```
