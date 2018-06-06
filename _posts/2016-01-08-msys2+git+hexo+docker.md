title: "MSYS2+GIT+HEXO+DOCKER"
date: 2016-01-08 12:20:05
tags:
- docker
- msys2
categories: soft_use
description: 
feature: /images/mainmenu/071.jpg
toc: true
---
#### MSYS2 ####
* pacman -Syu
* pacman -S wget curl make
* 其他见[附I](#jump01)

#### GIT ####
* git push git@gitcafe.com:channe/channe.git HEAD:gitcafe-pages

#### HEXO ####
```
npm install -g hexo-cli
npm install hexo-tag-bootstrap --save
npm install hexo-generator-search --save
npm install hexo-deployer-git --save
```

#### DOCKER ####
##### boot2docker #####
[boot2docker.iso](https://github.com/boot2docker/boot2docker)
docker官方工具包使用的virtualbox,提供docker_machine等工具.
我是只下载了iso文件,这样vmware workstation或者hyper-v也可以用.
默认用户docker,密码tcuser.
注意技巧I:构建自己的boot2docker.iso
例如虽然轻量级的linux也会自动识别磁盘,并在/etc/fstab添加自动挂载,不知为什么就是没有生效,必须要手动mount一次.
这时候就需要修改脚本.参考[GitHub:How to build boot2docker.iso locally](https://github.com/boot2docker/boot2docker/blob/master/doc/BUILD.md)
```
Dockerfile:
FROM boot2docker/boot2docker
# boot2docker welcome modify
RUN echo "" >> $ROOTFS/etc/motd; \
    echo "add solo.sh to /etc/profile by kris" >> $ROOTFS/etc/motd
# http_proxy
#RUN echo "export http_proxy= your proxy" >> $ROOTFS/etc/profile
# start.sh
# COPY ./solo.sh $ROOTFS/root/
# RUN chmod 755 $ROOTFS/root/solo.sh
ADD ./solo.sh $ROOTFS/etc/profile.d/
RUN echo "/root/solo.sh" >> $ROOTFS/etc/profile
RUN /make_iso.sh
CMD ["cat", "boot2docker.iso"]
```
```
sudo mount -o loop boot2docker.iso test/
sudo docker build -t solo_boot2docker_img .
sudo docker run --rm solo_boot2docker_img > boot2docker.iso
docker run -i -t --name u14shell -v /mnt/sdb1:/workspace -p 137:137 -p 138:138 -p 139:139 -p 445:445 --rm u14dev:basepluspython /bin/bash
service smbd start
net use S: \\192.168.88.128\workspace
```

##### daocloud #####
[我的集群/加速器](https://dashboard.daocloud.io/)
<!-- more --><span id="jump01">附I</span>
```
pacman -S   abc                 #从本地数据库中得到abc的信息，下载安装abc包
pacman -Sf abc                  #强制安装包abc 
pacman -Ss abc                 #搜索有关abc信息的包 
pacman -Si abc                  #从数据库中搜索包abc的信息 
pacman -Qi abc                  #列出已安装的包abc的详细信息 
pacman -Syu                      #同步源，并更新系统 
pacman -Sy                        #仅同步源 
pacman -Su                        #更新系统
pacman -R   abc                  #删除abc包 
pacman -Rc abc                  #删除abc包和依赖abc的包 
pacman -Rsc abc                #删除abc包和abc依赖的包 
pacman -Sc                        #清理/var/cache/pacman/pkg目录下的旧包 
pacman -Scc                      #清除所有下载的包和数据库 
pacman -U   abc                 #安装下载的abs包，或新编译的abc包
pacman -Sd abc                 #忽略依赖性问题，安装包abc 
pacman pacman -Su --ignore foo   #升级时不升级包foo 
pacman -Sg abc                  #查询abc这个包组包含的软件包	
```
##### 未整理0x00 #####
docker-machine start default
docker-machine ssh default
docker-machine env default --shell cmd
docker-machine env default --shell powershell
docker-machine env --shell=powershell default | Invoke-Expression
```
deb http://mirrors.163.com/ubuntu/ trusty main restricted
deb-src http://mirrors.163.com/ubuntu/ trusty main restricted
deb http://mirrors.163.com/ubuntu/ trusty-updates main restricted
deb-src http://mirrors.163.com/ubuntu/ trusty-updates main restricted
deb http://mirrors.163.com/ubuntu/ trusty universe
deb-src http://mirrors.163.com/ubuntu/ trusty universe
deb http://mirrors.163.com/ubuntu/ trusty-updates universe
deb-src http://mirrors.163.com/ubuntu/ trusty-updates universe
deb http://mirrors.163.com/ubuntu/ trusty-security main restricted
deb-src http://mirrors.163.com/ubuntu/ trusty-security main restricted
deb http://mirrors.163.com/ubuntu/ trusty-security universe
deb-src http://mirrors.163.com/ubuntu/ trusty-security universe
```
docker run hello-world
docker run -it ubuntu bash
docker run -it ubuntu:14.04 /bin/bash 
docker run docker/whalesay cowsay boo

docker ps -a

ssh docker@192.168.11.128
-v /mnt/sdb1:/workspace

#创建母盘
sudo fdisk /dev/sda
n
e
enter for default start
enter for deafult end
n
l
enter for default start
enter for deafult end
w
mkfs.ext4 /dev/sdb1
sudo mkfs.ext4 -L boot2docker-data /dev/sda5
#创建子盘
#
docker run -d -p 1880:1880 cpswan/node-red-0.5.0t
#使用加速hub
灵雀云 + DaoCloud
#dao pull ubuntu:14.04
```
vi Dockerfile
#FROM ubuntu:14.04
#RUN apt-get update
#RUN apt-get install -y gcc make vim ruby ruby-dev gem
#RUN gem install bundler
docker build --rm -t u14dev:base .
```
docker ps -l
docker commit -m "gcc installed" 957c8935c0a8 u14dev:baseplusgcc
docker attach 957c8935c0a8
docker run -i -t --name u14shell -v /mnt/sda1/data:/home/workspace --rm=true ubuntu:14.04 /bin/bash
docker run -i -t --name u14shell -v /mnt/sdb1:/home/workspace --rm=true ubuntu:14.04 /bin/bash

[workspace]
comment = Shared Folder
path = /workspace
public = yes 
writeable = yes 
valid users = kris
create mask = 0700
directory mask = 0700
force user = nobody
force group = nogroup
available = yes 
browseable = yes 

sudo useradd kris
sudo smbpasswd -a kris			twtypfjc
service smbd start

sudo netstat -tlnp | grep smb

set DOCKER_HOST=tcp://192.168.0.12:2376

##### 未整理0x01 #####
Storage Driver: aufs
Root Dir: /mnt/sda5/var/lib/docker/aufs
Backing Filesystem: extfs
Dirs: 0

docker0   Link encap:Ethernet  HWaddr 02:42:D4:52:89:95
          inet addr:172.17.42.1  Bcast:0.0.0.0  Mask:255.255.0.0
          inet6 addr: fe80::42:d4ff:fe52:8995/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:22373 errors:0 dropped:0 overruns:0 frame:0
          TX packets:22146 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:1455235 (1.3 MiB)  TX bytes:28974711 (27.6 MiB)

eth0      Link encap:Ethernet  HWaddr 00:15:5D:00:02:02
          inet addr:192.168.137.43  Bcast:192.168.137.255  Mask:255.255.255.0
          inet6 addr: fe80::215:5dff:fe00:202/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:139685 errors:0 dropped:0 overruns:0 frame:0
          TX packets:104336 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:200954772 (191.6 MiB)  TX bytes:7485682 (7.1 MiB)

docker run -d -v /mnt/sda1/data --name sharefs busybox true
docker run -v /mnt/sda1/data --name sharefs busybox true
docker run --rm -v /usr/local/bin/docker:/docker -v /var/run/docker.sock:/docker.sock svendowideit/samba sharefs
docker run -v /usr/local/bin/docker:/docker -v /var/run/docker.sock:/docker.sock svendowideit/samba sharefs
﻿\\192.168.59.103\mnt\sda5\data

mount -t vboxsf -o uid=1000,gid=50 code /code
mount /dev/sdb1 /mnt/sdb1 -t ntfs -o default

dd if=/dev/zero of=tmp bs=1024M count=5

退出container保持后台运行	ctrl+p ctrl+q
退出container并关闭			ctrl+c
```
docker ps -l 查询CONTAINER ID
docker commit -m "python installed" 4a979287c2ac u14dev:basepluspython			提交更改(like Git)
docker ps -a
docker rm `docker ps -a -q`				删除所有容器
docker rmi $(docker images -q)			删除所有images
docker rmi $(docker images -a | grep "^<none>" | awk "{print $3}")
```
container based on image
docker run是根据images生成新的container.
所以如果想持久化，需要在更改后提交image
```
sudo docker export * > /mnt/sdb1/ubuntu.tar		导出container
sudo docker save *   > /mnt/sdb1/ubuntu.tar		导出image

cat /home/export.tar | sudo docker import - u14dev:basepluspython
docker load < /home/save.tar

docker run -i -t --name u14shell -v /mnt/sda1/data:/workspace -p 137:137 -p 138:138 -p 139:139 -p 445:445 --rm ubuntu:14.04 /bin/bash
docker run -i -t --name u14shell -v /mnt/sdb1:/workspace -p 137:137 -p 138:138 -p 139:139 -p 445:445 --rm u14dev:basepluspython /bin/bash

--rm=true			run默认会持久化保存container,该选项是在container完成后自动删除
-v					挂载volumes，默认RW，可接:ro指定权限
```