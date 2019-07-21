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
- git rm [filename] 删除文件
- git rm --cached filename 撤销add提交
- git commit 提交本地库
- git mv [oldfilename] [newfilename] git修改文件名
- git stash [stash_name] 将工作区内容暂存进stash中
- git stash apply 将内容放回工作区.stash中的内容不会删除
- git stash pop 将内容放回工作区后删除stash中的内容
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
- git diff 显示暂存区和工作区的差异
- git diff HEAD 显示工作区和当前分支最新commit之间的差异
- git diff [commit1] [commit2] 显示2次提交之间的差异
- git reflog 显示当前分支的最近几次提交
- git show [commit] 显示某次提交的元数据和内容变化
## git 分支
- git branch 列出所有本地分支
- git branch -r 列出所有远程分支
- git branch -a 列出所有本地分支与远程分支
- git branch branch_name 新建分支
- git branch --track [branch] [remote-branch] 新建一个分支,并和远程分支产生追踪关系
- git branch --set-upstream [branch] [remote-branch] 将本地分支与远程分支建立追踪关系
- git branch -v 查看分支
- git checkout branch_name 切换分支
- 合并分支
  - 切换到被合并的分支上
  - git merge merge_branch_name
- git branch -d [branch-name] 删除本地分支
- git branch -dr [remote/branch] 删除远程分支
- git push [remote] --delete [branch-name] 删除远程分支
## git 标签
- git tag 列出所有tag
- git tag [tag] 在当前commit上创建一个tag
- git tag [tag] [commitid] 在指定commit上创建tag
- git tag -d [tag] 删除本地标签
- git push origin :refs/tags/[tagName] 删除远程标签
- git show [tag] 查看tag信息
- git push [remote] [tag] 推送指定tag
- git push [remote] --tags 提交所有tag
- git checkout -b [branch] [tag] 新建一个分支,指向tag
## git 远程仓库
- git remote 查看远程库列表
- git remote -v 查看远程库地址列表
- git remote add alias url 添加远程库
- git push alias branch 推送到远程仓库
- git clone url copy远程库
- git fetch alias branch 将更新内容抓取到本地
- git merge alias/branch 将内容merge到本地
- git pull alias branch 拉取新内容
- git fetch [remote] 下载远程仓库所有变动
- git push [remote] --force 强行推送本地内容到远程仓库,忽略冲突
- git branch --set-upstream-to=origin/dev dev 建立本地和origin远程dev分支的连接
## 撤销
- git checkout [file] 恢复暂存区的指定文件到工作区
- git checkout [commit] [file] 恢复指定commit的指定文件到工作区
- git checkout 恢复暂存区所有文件到工作区
- git reset --hard 重置暂存区与工作区,与上次commit保持一致
- git reset [commit] 重置当前分支的指针为指定commit, 重置暂存区, 但工作区保持不变
- git reset --hard [commit] 重置当前分支的指针为指定commit,并重置暂存区和工作区
- git reset --keep [commit] 重置HEAD指针到指定commit,但暂存区和工作区不变
- git revert [commit] 撤销指定commit,撤销也会作为一次提交进行保存。
- git revert 是用新的commit回滚之前的commit, git reset 是直接删除指定的commit

## 跨团队合作
- fork代码到仓库
- 发起pull request
  - pull request -> new pullrequest ->create pullrequest
- 审核代码
- merge pullrequest 
## Other
- git archive 生成一个可供发布的压缩包
- git rebase -i [remote]/[branch] 合并提交日志
- 将需要合并日志前的命令改为s即为合并日志
- 合并日志后需要使用git push --force [remote] [branch]强行推送!以为日志会改变分支历史