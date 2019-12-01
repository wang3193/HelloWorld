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
- upstream 配置一组被代理的服务器地址
```
upstream mysvr { 
    server 192.168.10.121:3333;
    server 192.168.10.122:3333;
}
server {
    ....
    location  ~*^.+$ {         
        proxy_pass  http://mysvr;  #请求转向mysvr 定义的服务器列表         
    }
}
```
1. 热备
- 当一台服务器发生故障时,才启用第二胎服务器提供服务
```
upstream mysvr { 
    server 192.168.10.121:3333;
    server 192.168.10.122:3333 backup;
}
```
2. 轮询
- 访问顺序为abababab
```
upstream mysvr { 
    server 192.168.10.121:3333;
    server 192.168.10.122:3333;
}
```
3. 加权轮询
- 访问顺序为abbabbabb
```
upstream mysvr { 
    server 127.0.0.1:7878 weight=1;
    server 192.168.10.121:3333 weight=2;
}
```
4. ip_hash
- 让相同的ip访问相同的服务器
```
upstream mysvr { 
    server 127.0.0.1:7878; 
    server 192.168.10.121:3333;
    ip_hash;
}
```
- 状态值
    - down 当前server不参与负载均衡
    - backup 备份机器,非备份机器故障时才会启用
    - max_fails 允许请求失败的次数,默认为1,超过最大次数时,返回proxy_next_upstream模块定义的错误
    - fail_timeout 载经历了max_fails次失败后,暂停服务的时间.
```
upstream mysvr { 
    server 127.0.0.1:7878 weight=2 max_fails=2 fail_timeout=2;
    server 192.168.10.121:3333 weight=1 max_fails=2 fail_timeout=1;    
}
```
## 动静分离
- 静态资源分离配置
```
#所有js,css相关的静态资源文件的请求由Nginx处理
location ~.*\.(js|css)$ {
    root    /opt/static-resources; #指定文件路径
    expires     12h; #过期时间为12小时
}
#所有图片等多媒体相关静态资源文件的请求由Nginx处理
location ~.*\.(html|jpg|jpeg|png|bmp|gif|ico|mp3|mid|wma|mp4|swf|flv|rar|zip|txt|doc|ppt|xls|pdf)$ {
    root    /opt/static-resources; #指定文件路径
    expires     7d; #过期时间为7天
}
```
## 配置文件
### 全局块
- 配置文件开始到events块之间的内容,主要设置影响nginx服务器整体运行的配置指令
- worker_process  1;
- include /etc/nginx/conf.d/*.conf     同时检查cond.d文件夹中的配置文件
- 一般有运行nginx服务器的用户组,nginx进程pid存放目录,日志存放目录,配置文件引入,允许生成work process等
### events块
- 主要影响nginx服务器与用户之间的网络连接.
- 如每个进程最大连接数,选取那种事件驱动模型处理连接请求.是否允许同时接受多个网路连接,开启多个网路连接序列化等
### http块
- http全局块   #可以嵌套多个server,配置代理,缓存,日志定义等.
    - server块      #配置虚拟主机相关参数
        - listen 监听   
        - location [PATTERN] location块     #配置请求的路由,以及各种页面处理情况


## nginx基本配置与参数说明
```
########### 每个指令必须有分号结束。#################
#user administrator administrators;  #配置用户或者组，默认为nobody nobody。
#worker_processes 2;  #允许生成的进程数，默认为1
#pid /nginx/pid/nginx.pid;   #指定nginx进程运行文件存放地址
error_log log/error.log debug;  #制定日志路径，级别。这个设置可以放入全局块，http块，server块，级别以此为：debug|info|notice|warn|error|crit|alert|emerg
events {
    accept_mutex on;   #设置网路连接序列化，防止惊群现象发生，默认为on
    multi_accept on;  #设置一个进程是否同时接受多个网络连接，默认为off
    #use epoll;      #事件驱动模型，select|poll|kqueue|epoll|resig|/dev/poll|eventport
    worker_connections  1024;    #最大连接数，默认为512
}
http {
    include       mime.types;   #文件扩展名与文件类型映射表
    default_type  application/octet-stream; #默认文件类型，默认为text/plain
    #access_log off; #取消服务日志    
    log_format myFormat '$remote_addr–$remote_user [$time_local] $request $status $body_bytes_sent $http_referer $http_user_agent $http_x_forwarded_for'; #自定义格式
    access_log log/access.log myFormat;  #combined为日志格式的默认值
    sendfile on;   #允许sendfile方式传输文件，默认为off，可以在http块，server块，location块。
    sendfile_max_chunk 100k;  #每个进程每次调用传输数量不能大于设定的值，默认为0，即不设上限。
    keepalive_timeout 65;  #连接超时时间，默认为75s，可以在http，server，location块。

    upstream mysvr {   
      server 127.0.0.1:7878;
      server 192.168.10.121:3333 backup;  #热备
    }
    error_page 404 https://www.baidu.com; #错误页
    server {
        keepalive_requests 120; #单连接请求上限次数。
        listen       4545;   #监听端口
        server_name  127.0.0.1;   #监听地址       
        location  ~*^.+$ {       #请求的url过滤，正则匹配，~为区分大小写，~*为不区分大小写。
           #root path;  #根目录
           #index vv.txt;  #设置默认页
           proxy_pass  http://mysvr;  #请求转向mysvr 定义的服务器列表
           deny 127.0.0.1;  #拒绝的ip
           allow 172.18.5.54; #允许的ip           
        } 
    }
}
```
1. $remote_addr 与 $http_x_forwarded_for 用以记录客户端的ip地址；
2. $remote_user ：用来记录客户端用户名称；
3. $time_local ： 用来记录访问时间与时区；
4. $request ： 用来记录请求的url与http协议；
5. $status ： 用来记录请求状态；成功是200；
6. $body_bytes_s ent ：记录发送给客户端文件主体内容大小；
7. $http_referer ：用来记录从那个页面链接访问过来的；
8. $http_user_agent ：记录客户端浏览器的相关信息；
9. 惊群现象：一个网路连接到来，多个睡眠的进程被同事叫醒，但只有一个进程能获得链接，这样会影响系统性能。
```
#运行用户
user nobody;
#启动进程,通常设置成和cpu的数量相等
worker_processes  1;

#全局错误日志及PID文件
#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;

#工作模式及连接数上限
events {
    #epoll是多路复用IO(I/O Multiplexing)中的一种方式,
    #仅用于linux2.6以上内核,可以大大提高nginx的性能
    use   epoll; 

    #单个后台worker process进程的最大并发链接数    
    worker_connections  1024;

    # 并发总数是 worker_processes 和 worker_connections 的乘积
    # 即 max_clients = worker_processes * worker_connections
    # 在设置了反向代理的情况下，max_clients = worker_processes * worker_connections / 4  为什么
    # 为什么上面反向代理要除以4，应该说是一个经验值
    # 根据以上条件，正常情况下的Nginx Server可以应付的最大连接数为：4 * 8000 = 32000
    # worker_connections 值的设置跟物理内存大小有关
    # 因为并发受IO约束，max_clients的值须小于系统可以打开的最大文件数
    # 而系统可以打开的最大文件数和内存大小成正比，一般1GB内存的机器上可以打开的文件数大约是10万左右
    # 我们来看看360M内存的VPS可以打开的文件句柄数是多少：
    # $ cat /proc/sys/fs/file-max
    # 输出 34336
    # 32000 < 34336，即并发连接总数小于系统可以打开的文件句柄总数，这样就在操作系统可以承受的范围之内
    # 所以，worker_connections 的值需根据 worker_processes 进程数目和系统可以打开的最大文件总数进行适当地进行设置
    # 使得并发总数小于操作系统可以打开的最大文件数目
    # 其实质也就是根据主机的物理CPU和内存进行配置
    # 当然，理论上的并发总数可能会和实际有所偏差，因为主机还有其他的工作进程需要消耗系统资源。
    # ulimit -SHn 65535

}


http {
    #设定mime类型,类型由mime.type文件定义
    include    mime.types;
    default_type  application/octet-stream;
    #设定日志格式
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  logs/access.log  main;

    #sendfile 指令指定 nginx 是否调用 sendfile 函数（zero copy 方式）来输出文件，
    #对于普通应用，必须设为 on,
    #如果用来进行下载等应用磁盘IO重负载应用，可设置为 off，
    #以平衡磁盘与网络I/O处理速度，降低系统的uptime.
    sendfile     on;
    #tcp_nopush     on;

    #连接超时时间
    #keepalive_timeout  0;
    keepalive_timeout  65;
    tcp_nodelay     on;

    #开启gzip压缩
    gzip  on;
    gzip_disable "MSIE [1-6].";

    #设定请求缓冲
    client_header_buffer_size    128k;
    large_client_header_buffers  4 128k;


    #设定虚拟主机配置
    server {
        #侦听80端口
        listen    80;
        #定义使用 www.nginx.cn访问
        server_name  www.nginx.cn;

        #定义服务器的默认网站根目录位置
        root html;

        #设定本虚拟主机的访问日志
        access_log  logs/nginx.access.log  main;

        #默认请求
        location / {
            
            #定义首页索引文件的名称
            index index.php index.html index.htm;   

        }

        # 定义错误提示页面
        error_page   500 502 503 504 /50x.html;
        location = /50x.html {
        }

        #静态文件，nginx自己处理
        location ~ ^/(images|javascript|js|css|flash|media|static)/ {
            
            #过期30天，静态文件不怎么更新，过期可以设大一点，
            #如果频繁更新，则可以设置得小一点。
            expires 30d;
        }

        #PHP 脚本请求全部转发到 FastCGI处理. 使用FastCGI默认配置.
        location ~ .php$ {
            fastcgi_pass 127.0.0.1:9000;
            fastcgi_index index.php;
            fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
            include fastcgi_params;
        }

        #禁止访问 .htxxx 文件
            location ~ /.ht {
            deny all;
        }

    }
}
```

### 虚拟服务器配置
- 配置http服务(80端口)
```
# Virtual Host configuration for arlingbc.com
#
# You can move that to a different file under sites-available/ and symlink that
# to sites-enabled/ to enable it.
#

# 丢弃缺乏 Host 头的请求
server {
       listen 80;
       return 444;
}

server {
       listen 80;
       listen [::]:80;
       server_name example.com www.example.com;

       # 定义服务器的默认网站根目录位置
       root /var/www/example/;
       
       # Add index.php to the list if you are using PHP
       index index.html index.htm index.nginx-debian.html;

       # access log file 访问日志
       access_log logs/nginx.access.log main;
       
       # 禁止访问隐藏文件
       # Deny all attempts to access hidden files such as .htaccess, .htpasswd, .DS_Store (Mac).
       location ~ /\. {
                deny all;
                access_log off;
                log_not_found off;
       }
    
       # 默认请求
       location / {
                # 首先尝试将请求作为文件提供，然后作为目录，然后回退到显示 404。
                # try_files 指令将会按照给定它的参数列出顺序进行尝试，第一个被匹配的将会被使用。
                # try_files $uri $uri/ =404;
      
                try_files $uri $uri/ /index.php?path_info=$uri&$args =404;
                access_log off;
                expires max;
       }
    
       # 静态文件，nginx 自己处理
       location ~ ^/(images|javascript|js|css|flash|media|static)/ {
            
           #过期 30 天，静态文件不怎么更新，过期可以设大一点，
           #如果频繁更新，则可以设置得小一点。
           expires 30d;
       }
    
       # .php 请求
       location ~ \.php$ {
                try_files $uri =404;
                include /etc/nginx/fastcgi_params;
                fastcgi_pass unix:/var/run/php5-fpm.sock;
                fastcgi_index index.php;
                fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
                fastcgi_intercept_errors on;
       }
    
      # PHP 脚本请求全部转发到 FastCGI 处理. 使用 FastCGI 默认配置.
      # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
      #
      #location ~ \.php$ {
      #       include snippets/fastcgi-php.conf;
      #
      #       # With php7.0-cgi alone:
      #       fastcgi_pass 127.0.0.1:9000;
      #       # With php7.0-fpm:
      #       fastcgi_pass unix:/run/php/php7.0-fpm.sock;
      #}
      
      # 拒绝访问. htaccess 文件，如果 Apache 的文档根与 nginx 的一致
      # deny access to .htaccess files, if Apache's document root
      # concurs with nginx's one
      #
      #location ~ /\.ht {
      #       deny all;
      #}
}
```
- 配置https服务(443端口)
```
##
# 80 port
##

# 默认服务器，丢弃缺乏 Host 头的请求
server {
       listen 80;
       return 444;
}

server {
        listen 80;
        listen [::]:80;
        sever_name example.com www.example.com;

        rewrite ^(.*)$ https://$host$1 permanent;  ## 端口转发，301 重定向
}

##
# 443 port
##
server {
    
    ##
    # 阿里云参考配置
    ##
    
    listen 443;
    listen [::]:443;
    server_name example.com www.example.com;
    
    root /var/www/example/;    # 为虚拟服务器指明文档的根目录
    index index.html index.htm; # 给定URL文件
    
    ##
    # 部署 HTTP 严格传输安全（HSTS）
    ##
    add_header Strict-Transport-Security "max-age=31536000; includeSubDomains; preload;"
    
    # Note: You should disable gzip for SSL traffic.
    # See: https://bugs.debian.org/773332
    gzip off;
    
    ##
    # SSL configuration
    ##
    
    ssl on;
    ssl_certificate   cert/certfile.pem;    # 证书
    ssl_certificate_key  cert/certfile.key; # 私钥
    ssl_session_timeout 5m; # 设置超时时间
    # 密码套件配置
    # 密码套件名称构成：密钥交换-身份验证-加密算法（算法-强度-模式）-MAC或PRF
    ssl_ciphers ECDHE-RSA-AES128-GCM- SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4; 
    ssl_protocols TLSv1.2; # 设置 SSL/TSL 协议版本号
    ssl_prefer_server_ciphers on; # 控制密码套件优先级，让服务器选择要使用的算法套件
    ssl_buffer_size 1400; # 减少TLS缓冲区大小，可以显著减少首字节时间（《HTTPS权威指南》P416）
    
    ##
    # location configuration
    ##
    
    # ...
}
```

