# Linux
## help
- man [command] 查看命令帮助
- info [command] 查看菜单帮助
## 用户/用户组
- UID 用户id
- GID 用户组id
- who 查看用户在线
- etc/passwd 存放密码文件
- etc/shadow 存放密码文件
### 用户操作
- /etc/skel 文件夹下存放创建用户的模板文件
- useradd [username] 添加用户
- useradd -u [UID] [username] 创建指定UID的用户
- useradd -g [group] [username] 创建指定用户组的用户
- useradd -d [homepath] [username] 创建指定home文件夹的用户
- passwd [username] 修改指定用户名的密码,只能root用户使用
- passwd 修改当前用户的密码
- usermod [username] 修改用户信息,参数和useradd相同
- usermod -L [username] 冻结用户
- usermod -U [username] 解锁用户
- userdel [username] 删除用户
- userdel -r [username] 删除用户的同时删除用户文件夹
### 用户组操作
- groupadd [groupname] 添加用户组
- groupdel [groupname] 删除用户组
### 查看用户信息
- users 查看当前系统有哪些用户
- who 查看当前系统有哪些用户,显示信息比users详细
- w 比who的信息更多
- finger [username] 显示用户详细信息
### 切换用户
- su [username] 切换成对应用户
- su- [username] 切换成对应用户,同时应用对应环境的用户环境
- sudo 是当前用户具有root权限,
  - 需要设置/etc/sudoers文件
  - visudo 编辑/etc/sudoers文件命令
## 例行任务管理
- at 特定时间执行一次
- [cron] [command] 周期性执行任务 
- crontab -l 查看所有任务
- crontab -r 删除所有任务
## 文件管理
### 文件
- touch [filename] 创建文件
- rm [filename] 删除文件
- mv [from] [to] 移动文件从from到to,也可用于文件重命名
- cat [filename] 查看文件
- head -n [num] [filename] 查看文件头指定行数
- tail -n [num] -f [filename] 查看文件最后几行,使用-f会动态刷新数据
- dos2unix [filename] 将dos文件格式文件转成unix文件格式
### 文件夹操作
- cd [dir] 进入文件夹
- mkdir [dirname] 创建目录
- mkdir -p [dir1]/[dir2] 创建树型文件夹
- rmdir 删除空文件夹
- rm -rf [dirname] 循环删除文件夹/文件
- cp -r [from] [to] 将from文件夹复制到to文件夹
### 文件权限管理
- ls -al 查看文件信息(带权限)
- lsattr [filename] 查看文件属性
- chattr +a [filename] 给文件添加a属性,即使root用户也无法删除
- chattr +i [filename] 给文件添加i属性,无法写入,改名,删除
- chmod 修改文件权限,如果需要修改文件夹,使用-R来循环修改
- chown [username]:[group] [filename] 修改文件拥有者
- chgrp 修改文件组
- chmod u+s [filename] 给文件添加SUID权限,只能用于二进制文件
- file [filename] 查看文件属性
### 查找文件
- find [path] [filename] 查找path下的filename
  - find . -name filename -type {f/d/l/c/b/s/p} 从当前目录及子目录下查找名称为filename,类型为f:文件,d:文件夹,l:链接,c:字符设备,b块设备,s套接字,p:fifo的文件.
  - find . -maxdepth 3 想下最大限制深度为3
  - find . (-atime|-amin|-mtime|-mmin|-ctime|-cmin) (-7|7|+7) 查找 (最近访问时间天|分钟|修改时间天|分钟|变化时间天|分钟) (7天内|正好7|7以外)的文件
  - find . -size 2(b|c|w|k|M|G) 查找文件大小为2(b|字节|2字节|kb|MB|GB)的文件
  - find . -name filename -delete 删除匹配文件
  - find . -perm 777 依据文件权限进行匹配
  - find . -empty 查找长度为0的文件
  - find . -name file -exec rm -rf {} \; 匹配指定文件文件夹并删除
  - find . -name file |xargs rm -rf  匹配指定文件文件夹并删除
- locate [filename] 从数据库查询文件
  - 如果需要查询新建文件需要updatedb
- which/whereis 查询可执行文件位置
### 文件压缩打包
- gzip/gunzip [filename] 文件压缩/解压缩
- tar -zcvf [filename] [sourcedir] 将文件打成filename的tar包,z表示zip压缩,c创建压缩文件,v显示当前被压缩的文件,f使用文件名
- tar -zxvf [filename] -C [todir] 解压缩tar包, -C表示解压到指定位置
## 文件系统
- fdisk -l 查看磁盘设备
- 创建文件分区
  1. fdisk [磁盘名:sdb/sdc]
  2. 输入n,表示新建分区
  3. 输入e(扩展分区)或者p(主分区)
  4. partition number输入1表示是第一个分区
  5. 输入开始位置1
  6. 输入结束位置
  7. 输入w将创建的分区写入分区表
- 磁盘挂载
  - mount [sdb] [dir] 将磁盘sdb挂载到dir文件夹下
  - 自动挂载 /etc/fstab文件添加 [sdb] [dir] ext3 defaults 0 0 
    - 将sdb挂载到dir,文件类型是ext3,使用默认挂载参数defaults,dump备份是否存档默认为0,系统启动是否对该设备fsck,1保留给根分区,2检查完根分区后检查,0不检查
  - fsck 检查磁盘状态,需要载未挂载状态下检查,即要先unmount
## 逻辑卷
- pvcreate 创建物理卷
  1. 使用fdisk创建文件分区
  2. 修改分区id为8e
    1. fdisk [sdb]
    2. 输入t 修改分区代码
    3. 输入要修改的分区[1-4]
    4. 输入L可以查看分区代码
    5. 输入分区代码8e
    6. 输入w写入分区表
- pvcreate [sdb1] 将分区sdb1创建为逻辑卷
- pvscan 查看系统中的PV
- pvdisplay 查看pv详细信息
- vgcreate [vgname] [sdb1] [sdb2] 使用sdb1,sdb2创建vgname的卷组
- vgscan 查看卷组
- vgextend [vgname] [sdb3] 将sdb3扩展进vgname
- lvcreate -L [size?100M] [lvName]  [vgName] 从vgname中划分出size大小的空间创建lvName的逻辑卷
- lvdisplay 查看逻辑卷的使用情况
- mkfs.ext3 [lvname] 将逻辑卷创建文件系统
- mount [lvname] [dir] 将逻辑卷挂载
## 软/硬链接
- ln [target] [link] 为target创建link的硬链接,硬链接:通过索引节点来进行链接,如果一个文件有多个硬链接,删除单个硬链接不会使文件删除,只有所有的硬链接都被删除,该文件才会被删除
- ln -s [target] [link] 为target创建软连接,类似快捷方式,如源文件被删除,软连接会断链
## 字符处理
- | 管道符,将|左侧的输出作为右侧的输入
- grep [-icnv] [字符] [filename] 基于行的搜索文本,i:忽略大小写,c统计包含匹配的行数,n输出行号,v:反向匹配
- sort [-ntkr] 为文本内容排序,n:按数字排序,t指定分隔符,k指定排序列,和t组合使用,r反向排序
- uniq [-ic] 删除连续重复行,常和sort连用,i:忽略大小写,c统计重复行数
- cut [-cd] 截取指定字符,-d:指定分隔符,-c:指定分割后的列或者指定的字符列数,连续列使用-,如1-3,1到3列字符,非连续的使用,分隔,例如1,5,第一列和第五列
- tr 文本转换 如:tr '[a-z]' '[A-Z]' 将小写替换成大写,tr -d ':' 删除所有:
- paste -d[分隔符] [fileA] [fileB] 将a和b按照行合并,-d指定分隔符
- split [-lb] [bigfile] [smallfile] 将大文件按照l行数,b大小分割为小文件
## 网络管理
- ifconfig 显示系统中处于活动状态的网络接口
- ifup [网卡] 手动启动网卡
- ifdown [网卡] 手动关闭网卡
- /etc/sysconfig/network-scripts/[网卡] 配置文件所在地址
- service network restart 重启网络服务
- /etc/hosts hosts文件所在地址
## 进程
- ps 查看进程
- top 实时观察进程状态,使用O进入排序选择页
- kill -[1,9,15] [pid] 1重启,9强制关闭,15正常结束pid进程
- killall [psname] 使用进程名关闭进程
- lsof [dir] 查看文件夹被占用进程
- lsof -i [port] 查看端口被占用进程
## 编译,安装程序
- 配置环境变量 echo "export PATH=$PATH:/path" >> /etc/rc.local
- 编译安装软件
  1. 运行configure生成makefile文件
  2. 运行make生成模块主程序
  3. 运行make install 将必要文件复制到安装目录
## shell
- 自动编译.bashrc文件
 - 在~/.bash_profile中添加加载.bashrc语句
```
 # 加载.bashrc文件
if test -f .bashrc ; then
source .bashrc 
fi
```
### login shell/nologin shell
- 需要使用用户密码登录的shell为login shell, login shell 读取/etc/profile、$HOME/.bash_profile,$HOME/.bash_login，$HOME/.profile文件
- 不需要用户密码的shell为nologin shell, nologin shell 读取$HOME/.bashrc，而.bashrc又会执行/etc/bashrc文件
- mac的shell 是一个login shell,所以会读取.bash_profile文件而不是.bashrc文件
