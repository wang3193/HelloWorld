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

## 启动
- docker run image commend 执行式启动,执行完命令后关闭
- docker run -i -t image /bin/bash 交互式启动
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
- 

