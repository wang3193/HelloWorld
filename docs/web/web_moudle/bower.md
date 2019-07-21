<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [bower](#bower)
  - [安装](#%E5%AE%89%E8%A3%85)
  - [执行](#%E6%89%A7%E8%A1%8C)
  - [配置文件](#%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# bower
- web 包管理器
- [bower官网](bower.io)
## 安装
- npm install -g bower
## 执行
- bower
- bower init
- bower install packageName 安装需要的js包, 添加--save 参数可以将数据保存到bower.json中
## 配置文件
- .bowerrc
```
{
  "directory": "app/components/",
  "timeout": 120000,
  "registry": {
    "search": [
      "http://localhost:8000",
      "https://registry.bower.io"
    ]
  }
}
```
  