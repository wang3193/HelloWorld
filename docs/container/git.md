# GIt
## 本地库初始化
- git init
- 设置签名 (项目)
  - git config user.name username
  - git config user.email email
- 设置签名 (全局)
  - git config --global user.name username
  - git config --global user.eamil email
- 签名只用于区分提交代码身份,和远程仓库账号密码无关
## git 基本操作
- git status 查看git状态
- git add filename 添加文件进暂存区
- git rm --cached filename 撤销add提交
- git commit 提交本地库
## git 版本操作
- git log
  - 多屏展示 空格:下一页 b:上一页 q:退出
  - git log --pretty=oneline
  - git log --oneline
- git reflog
- 基于索引值操作
  - git reset --hard id
- 使用^符号
  - git reset --hard HEAD^^ 一个^后退一个版本
- 使用~符号
  - git reset --hard HEAD~5 后退5个版本
- git diff 比较文件不同
## git 分支
- git branch branch_name 新建分支
- git branch -v 查看分支
- git checkout branch_name 切换分支
- 合并分支
  - 切换到被合并的分支上
  - git merge merge_branch_name
## git 远程仓库
- git remote add alias url 添加远程库
- git push alias branch 推送到远程仓库
- git clone url copy远程库
- git fetch alias branch 将更新内容抓取到本地
- git merge alias/branch 将内容merge到本地
- git pull alias branch 拉取新内容
## 跨团队合作
- fork代码到仓库
- 发起pull request
  - pull request -> new pullrequest ->create pullrequest
- 审核代码
- merge pullrequest 