<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Nginx](#nginx)
  - [install](#install)
  - [command](#command)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Nginx
- 代理服务器
## install
- brew install nginx
- /usr/local/etc/nginx/nginx.conf
- /usr/local/etc/nginx/logs
## command
- nginx --start nginx
- nginx -s stop --fast shutdown
- nginx -s reload --reload the configuration file
- nginx -s quit --graceful shutdown
- nginx -s reopen --reopening the log files
- 