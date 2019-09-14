<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [配置](#%E9%85%8D%E7%BD%AE)
- [快捷键](#%E5%BF%AB%E6%8D%B7%E9%94%AE)
- [插件管理](#%E6%8F%92%E4%BB%B6%E7%AE%A1%E7%90%86)
- [tmux](#tmux)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

[TOC]
# 配置
- vim ~/.vimrc 

# 快捷键
- w word,移动到下一个单词 e end,移动到下一个单词的尾部
- b back,移动到上一个单词
- i 在当前光标位置插入, a 在当前光标后面插入, A 在当前行尾插入
- ctrl+f forward,下一页
- ctrl+b back,上一页
- 12gg 跳到第12行
- 5j  往下5行
- 5k  往上5行
- /word 高亮word,高亮后n查找下一个word,N查找上一个word
- ?word 反向查找word,同/
- crtl+O 跳转到上一次的位置
- crtl+I 跳转到较新的位置
- yy 复制整行 y复制选中 yw复制单个单词 y^复制当前到行头 y$复制当前到行尾 yG复制当前到档尾 "+y 复制进剪切板
- cc 剪切整行
- p 粘贴
- x 删除单个字符
- dd 删除单行
- dw 删除单词
- d$ 删除当前位置到行尾
- de 删除单词.不包括后面的空格
- 0 移动到行首 $移动到行尾
- 2w 向后移动2个单词
- 2e 向后移动2个单词,并移动到词尾
- u undo 撤销
- U 撤销对当前行整行的操作
- ctrl+R 撤销撤销行为
- r 替换当前字符, R 连续替换字符
- cw/ce 删除到词尾,并进入插入模式
- c$ 删除当前到词尾,并进入插入模式
- s/old/new 将第一个old替换成new
- s/old/new/g 将正行old替换为new
- #,#s/old/new/g 将#行到#行的old全部替换为new
- 1,$s/old/new/g 将第一行到最后一行的old替换为new
- %s/old/new/g 将整个文件的old替换new
- %s/old/new/gc 将整个文件的old替换new,替换时进行提示是否替换
- ctrl+G 查看当前光标的状态
- G 跳转到最后一行
- gg 跳转到文件第一行
- 501G 跳到第501行
- % 可以查找配对的括号 )、]、}
- :!SHELL 执行shell命令
- :r FILENAME 向当前文件插入FILENAME文件中的内容
- :r !SHELL 将SHELL命令输出插入到当前文件
- o 在光标下方打开新的一行, O 在光标上方打开新的一行
- set ic ingnore case 忽略大小写 set noic 取消忽略大小写
- set hls is 设置连续高光查询 set nohlsearch 取消高光
- ctrl+w 在窗口中切换
- ctrl+v 块模式选择, I 块模式编辑
- > 在显示模式选择完后进行后缩进,<进行前缩进

# 插件管理
- vim plug
- [vim插件](https://vimawesome.com/)
# tmux
- tmux 启动服务
- tmux new -s mysession　创建名称为mysession的会话
- tmux ls　　显示会话列表
- tmux a -t s1 连接名称为s1的会话
- tmux kill-session -t s1 关闭名称为s1的会话
- tmux kill-server 关闭所有会话
- c 创建新窗口
- & 删除当前窗口
- , 重命名当前窗口
- w 列出所有窗口
- n 下一个窗口
- p 上一个窗口
- % 水平创建pannel
- " 垂直创建pannel
- 方向键 切换pannel
- x 删除pannel
- z 放大/还原pannel


