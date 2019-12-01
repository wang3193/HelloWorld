<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Nginx](#nginx)
  - [install](#install)
  - [command](#command)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Nginx
- 代理服务器
- http中间件
- 轻量级
- CPU亲和 把CPU核心和Nginx工作进程绑定,减少切换cpu的cache miss
- 
## install
### mac
- brew install nginx
- /usr/local/etc/nginx/nginx.conf
- /usr/local/etc/nginx/logs
### linux
## command
- nginx --start nginx
- nginx -s stop --fast shutdown
- nginx -s reload --reload the configuration file
- nginx -s quit --graceful shutdown
- nginx -s reopen --reopening the log files
- nginx -c [nginxfile] 启动nginx

## 反向代理
- 正向代理,客户端配置代理服务器
- 反向代理,服务器配置代理服务器

## 负载均衡
## 动静分离

## 配置文件
### 全局块
- 配置文件开始到events块之间的内容,主要设置影响nginx服务器整体运行的配置指令
- worker_process  1;
### events块
- 主要影响nginx服务器与用户之间的网络连接
### http块
- http全局块
- server块
    - listen 监听
    - location 
