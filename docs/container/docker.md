<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Docker](#docker)
  - [定义](#%e5%ae%9a%e4%b9%89)
  - [相关技术](#%e7%9b%b8%e5%85%b3%e6%8a%80%e6%9c%af)
  - [安装](#%e5%ae%89%e8%a3%85)
    - [ubuntu 安装](#ubuntu-%e5%ae%89%e8%a3%85)
    - [mac 安装](#mac-%e5%ae%89%e8%a3%85)
  - [基础命令](#%e5%9f%ba%e7%a1%80%e5%91%bd%e4%bb%a4)
    - [docker](#docker)
  - [启动](#%e5%90%af%e5%8a%a8)
  - [部署静态网站](#%e9%83%a8%e7%bd%b2%e9%9d%99%e6%80%81%e7%bd%91%e7%ab%99)
    - [创建Nginx部署](#%e5%88%9b%e5%bb%banginx%e9%83%a8%e7%bd%b2)
  - [镜像](#%e9%95%9c%e5%83%8f)
  - [构建镜像](#%e6%9e%84%e5%bb%ba%e9%95%9c%e5%83%8f)
    - [docker commit 方式构建镜像](#docker-commit-%e6%96%b9%e5%bc%8f%e6%9e%84%e5%bb%ba%e9%95%9c%e5%83%8f)
    - [docker build 方式构建镜像](#docker-build-%e6%96%b9%e5%bc%8f%e6%9e%84%e5%bb%ba%e9%95%9c%e5%83%8f)
  - [Remote API](#remote-api)
  - [DockerFile](#dockerfile)
  - [DOckerFIle 构建过程](#dockerfile-%e6%9e%84%e5%bb%ba%e8%bf%87%e7%a8%8b)
  - [Docker网络](#docker%e7%bd%91%e7%bb%9c)
  - [Docker容器互联](#docker%e5%ae%b9%e5%99%a8%e4%ba%92%e8%81%94)
  - [Docker容器与外部网络的连接](#docker%e5%ae%b9%e5%99%a8%e4%b8%8e%e5%a4%96%e9%83%a8%e7%bd%91%e7%bb%9c%e7%9a%84%e8%bf%9e%e6%8e%a5)
  - [Docker数据管理](#docker%e6%95%b0%e6%8d%ae%e7%ae%a1%e7%90%86)
  - [docker跨主机连接](#docker%e8%b7%a8%e4%b8%bb%e6%9c%ba%e8%bf%9e%e6%8e%a5)
  - [小技巧](#%e5%b0%8f%e6%8a%80%e5%b7%a7)
    - [删除所有容器](#%e5%88%a0%e9%99%a4%e6%89%80%e6%9c%89%e5%ae%b9%e5%99%a8)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Docker
## 定义
- 一个基于linux内核的轻量级的容器解决技术
## 相关技术
- NameSpaces命名空间 PID NET IPC MNT UTS
- Control groups控制组 资源限制,优先级设定,资源计量,资源控制
- 文件系统隔离
-  进程隔离
-  网络隔离
-  资源隔离和分组
## 安装
### ubuntu 安装
- 内核版本检查 uname -a
- Device Mapper检查 ls -l /usr/class/misc/device-mapper
- apt-get 安装 apt-get install docker.io    source /etc/bash_completion.d/docker.io
- docker 官网安装 
  - apt-get install -y curl
  - curl -sSL https://get.docker.com/ubuntu/|sudo sh
- 添加docker用户
  - sudo groupadd docker
  - sudo gpasswd -a ${User(当前用户)} docker
  - sudo service docker restart
### mac 安装
- brew install docker
## 基础命令
### docker
- run 启动容器
  - --name 给容器命名
  - -i 打开持续输入流
  - -t 打开tty终端
  - -d 启动守护式容器,后台运行容器
  - --restart=always 自动重启容器
  - -p port:imageport 指定主机端口和容器端口绑定
- ps 查看正在运行的容器
  - -a 查看所有容器
  - -l 查看最后一次运行的容器
  - -q 只返回容器id信息
- start 启动停止的容器
- logs 查看容器日志
  - -f 持续查看
- top 查看容器进程
- exec 运行进程任务
- stop 停止容器
- inspect 获取容器信息
- rm 删除容器
- pull 拉取镜像
- search 列出可拉镜像
- images 查看本地下载镜像
- login 登录dockerhub
- commit 基于已有镜像构建新镜像
- build 使用dockerFile构建镜像
  - -t="rep/image:tag" 设置镜像信息
- port 查看容器端口绑定情况
## 启动
- docker run image commend 执行式启动,执行完命令后关闭
  - --privileged=true 是docker中的root拥有真正root的权限 
- docker run -i -t -p:port image /bin/bash 交互式启动
- -i 开启容器中的stdin,持续输入流
- -t 分配一个伪tty终端
- docker ps -a -l 查看docker容器
- docker inspect id/name 依据容器id或名称查看容器
- docker run --name=selfname -i -t image /bin/bash 使用自定义容器名
- docker start -i image 重新启动已经停止的容器, -i表示以交互式启动
- docker rm image 删除已经停止的容器
- 在交互式容器中不用exit退出,而是用ctrl+p, ctrl+q 退出,容器并不会停止运行,而会以守护线程方式运行
- docker attach image 进入已经启动的容器中
- docker run -d image 使用守护线程方式启动容器
- docker logs -tf image 查看容器日志, -t 是加上时间戳 -f 保持日志更新
- docker top image 查看容器进程情况
- docker exec -d -i -t image commend 在容器中启动新的进程
- docker stop image 停止容器,发送信号给容器,等待容器停止运行
- docker kill image 强制停止容器
- man docker-run 查看docker运行命令
- man docker-logs 查看docker日志命令
- man docker-top
- man docker-exec

## 部署静态网站
- run -P -p 设置容器暴露端口 -P是容器暴露所有端口 -p指定映射指定容器端口
### 创建Nginx部署
- 创建映射80端口的交互式容器
- 安装Nginx
- 安装文本编辑器vim
- 创建静态页面
- 修改Nginx配置文件
- 运行Nginx
- 验证网站访问
- docker port imagename 查看容器绑定端口情况
- 使用127.0.0.1:port 来访问
## 镜像
- docker images [options] [repository]
- docker iamges -q $repository 只返回对应仓库的id
- docker inspect $repository 查看镜像信息
- docker rmi $repository 删除仓库镜像
- docker rmi $(docker images repository -q)
- docker hub网站查询镜像
- docker search $image 查找镜像
- docker pull $image 拉取镜像
- docker pull --registry-mirror 从国内网站拉取
- docker push $image 推送镜像到docker hub
- docker commit -a author -m message localimagename remoteimagename 构建镜像
- docker build -t=name dockerfilepath 通过dockerfile 构建对象

## 构建镜像
### docker commit 方式构建镜像
- docker commit -m="commit info" --author="authorname" [imageId] [repName]/[imageName]:[tagname]
### docker build 方式构建镜像
- docker build -t="rep/image:tag" dockerFiledir
  - 会去dir文件夹中查找Dockerfile文件

## Remote API
- 连接方式
  - unix:///var/run/docker.sock
  - tcp://host:port
  - fd://socketfd
- ps -ef|grep docker 查看docker进程
- status docker
- DOCKER HOST: docker客户端参数配置docker远程访问

## DockerFile
- # 注释
- FROM image:version 
  - 必须是已存在的镜像
  - 必须是第一条非注释指令
- MAINTAINER name 
  - 指定镜像作者信息
- RUN 容器构建命令
  - apt-get upgrad shell模式 
  - ["command","param1","param2"] exec模式 
- EXPORT 指定运行镜像容器使用的端口
- CMD 提供容器运行时默认命令,会被启动docker时命令覆盖
  - shell模式
  - CMD["command","param1","param2"]exec模式
  - CMD["param1","param2"] 作为ENTRYPOINT指令的默认参数
- ENTRYPOINT 容器运行时需要执行指令,覆盖启动docker时命令
  - shell
  - exec
  - 一般是用ENTRYPOINT指定命令,CMD指定参数,执行时使用-g 参数覆盖CMD参数
- ADD 
  - src dest 直接输入文件路径
  - ["src","dest"] 适用于文件路径中有空格的情况
- COPY
  - src dest
  - ["src","dest"]
  - ADD包含默认解压tar包和处理url, COPY使用于单纯复制文件
- VOLUME["/data"] 用于向基于镜像创建的容器添加卷
- WORKDIR 指定终端登录默认的目录,使用绝对路径,相对路径会一直传递
- ENV key=value 创建环境变量
- USER 指定镜像以什么用户去运行,不指定默认使用root用户
- ONBUILD 镜像触发器,当一个镜像被其他镜像作为基础镜像时执行,在构建过程中插入指令  
## DOckerFIle 构建过程
- 从基础镜像运行一个容器
- 执行一条指令对容器做出修改
- 执行类似docker commit 操作,提交一个新的镜像层
- 基于刚提交的镜像运行一个新容器
- 执行dockerfile中吓一条指令,知道指令执行完毕
- build命令会删除中间层创建的容器,不会删除中间层常见的镜像
- 可以使用docker run 运行中间层创建的容器,调试中间层
- 构建缓存
- docker build --no-cache 跳过缓存
- docker history 查看构建过程
## Docker网络
- docker0 虚拟网桥
- apt-get bridge-util linux网桥管理程序
- ifconfig docker0 ip netmask mask
- 添加自定义网桥
  - brctl addbr br0 添加一个名为br0的新网桥
  - ifconfig br0 ip netmask mask
  - 将br0添加进docker配置文件中
## Docker容器互联
- 使用--link image alas 使用--link命令+别名进行互联(官方不推荐)
- 使用-h image 进行互联
  - docker network create -d bridge blog
  - docker run --net blog -h blog-mysql --name blog-mysql -d mysql:5.6
  - docker run --net blog -h blog-tomcat --name blog-tomcat -d tomcat:8.0
- 拒绝容器互联
  - --icc=false
- 允许特定容器键互联
  - icc=false iptable=true 守护线程启动项中
## Docker容器与外部网络的连接
- ip_forward
  -  
- iptables
- 允许端口映射访问
- 限制ip访问容器
## Docker数据管理
- 容器的数据卷
  - 独立存在
  - 与宿主机进行交互
  - 可在容器键共享重用
  - 在docker run中使用 -v path:path 指定数据卷在本机的目录和在容器的目录名
  - 添加权限 -v path:path:param 
  - dockerfile中创建使用 VOlUME ["path","path"] 来创建多个数据卷,该数据卷在系统中的位置是docker随机指定的,多次创建不相同
- 数据卷容器
  - 使用一个创建了数据卷的容器来共享数据卷
  - 在docker run --volumes-from imagename 
- 数据卷的备份与恢复
  - 数据备份 docker run --volumes-from image -v $(pwd):/backup ubuntu tar cvf /backup/backup.tar /volumes(要备份的数据卷目录) 将image的数据卷内容打包备份到/backup目录中
## docker跨主机连接
- 使用weave进行跨主机连接
- 安装weave
- 启动weave
  - weave launch
- 连接不同主机
- 通过weave启动容器
  
## 小技巧
### 删除所有容器
- docker rm `docker ps -a -q`
- 


