### git use

#####  初始化参数
> git config --global user.name "你的名字"
> git config --global user.email "你的邮箱地址"


##### SSH key生成
>  ssh-keygen -t rsa -C "你的邮箱地址"

##### 检出分之
> git branch -r  查看所有分支列表
> git clone uri  分之名称(为空时默认master)   检出某个分支所有代码

##### 更新代码
> git pull
> git status 
> git log

##### 切换分支
> git checkout 分支名称

##### 查看当前分支所在位置
> git branch

##### 提交代码
> git status
> git add xxx.java  or git add  .  或者使用  git add -a
>  git commit -m "本次修改的备注，如修改了用户头像不展示问题"  
>  git push -u origin master

#### 撤销
>  git reset --hard HEAD  回到上次提交的状态
>  git revert HEAD^  回到上上次提交的状态
>  git reset  file_name 单个文件回到上次提交的状态
>  git checkout file_name 恢复远程的状态

##### 创建分支
> git mastert
> git checkout -b test  从master分支上创建了一个test分支
> git push origin test 将test分支推送到远程服务器

##### 分支合并
> git checkout master 
> git merge test   将test分支合并到master
> git push -u origin master  合并之后推送

##### log
> git log 查看提交历史记录
> git log --oneline  或者 git log --pretty=oneline 以精简模式显示
> git log --graph 以图形模式显示
> git log --stat 显示文件更改列表
> git log --author= 'name' 显示某个作者的日志
> git log -p filepath 查看某个文件的详细修改
> git log -L start,end:filepath 查看某个文件某几行范围内的修改记录
>  git log --stat commitId  或者 git show --stat commitId 查看某一次提交的文件修改列表  
