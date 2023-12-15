## 简看

URL：[Git教程 - 廖雪峰的官方网站 (liaoxuefeng.com)](https://www.liaoxuefeng.com/wiki/896043488029600)

### 对比

#### SVN-集中式控制系统

版本集中存放在中央服务器，上传和下载需要访问中央服务器

必须连网才可以工作



#### Git-分布式控制系统

每个人电脑都有一个完整版本库供相互访问。

会有一个中央服务器方便大家访问

### 使用Git

#### 安装

Git->Git Bash

#### 初次配置

```git
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```

#### 初始化某个文件作为仓库

```
git init				//	产生一个.git文件
```

#### 提交

```
git add change.txt		//	将修改的文件添加到stage
git commit -m "message"	//	提交stage到仓库
```

#### 查询

```
git status				//	查询仓库当前状态
git diff				//	查看修改内容
get diff HEAD -- a.txt	//	查看文件和最新版本的区别

git log					//	查看历史提交记录
git log --pretty=oneline//	简化记录
//commit id(版本号)是一个SHA1计算出来的key 而不是依次递增数
//防止每个人版本号产生冲突
```

#### 版本

```
HEAD					//	表示当前版本
HEAD^					//	上个版本
HEAD^^					//	上上版本
HEAD~100				//	前一百个版本

git reset --hard HEAD^	//	回退至上个版本
git reset --hard 1094a	//	回退到1094a版本

//回退后，当前版本在git log查询不到，必须知道ID才可以回退
git reflog 				//	记录每一次命令
```

#### 撤销修改

```
//	让文件回到最近一次commit/add 的状态
git checkout -- a.txt	
```

#### 删除

```
rm a.txt				//	文件管理器删除没用文件
git rm a.txt			//	把删除指令提交到缓存区
git commit -m "remove"	//	从库里删除
git checkout -- a.txt	//	撤销删除
```

#### 远程仓库-GitHUb

```
//	用户主目录下创建SSH KEY存于.ssh目录内
//	id_rsa 私钥	id_rsa.pub 公钥
ssh-keygen -t rsa -C "youremail@example.com"

//	本地库提交到github
git remote add origin git@github.com:xxx
//	从远程库克隆到本地
git clone git@github.com:xxx

git remote -v			//	查看远程库信息
git remote rm origin	//	删除origin
```

#### 分支

```
master					//	主支
HEAD 					//	指向当前分支

git branch				//	查看当前分支

git branch dev			//	创建dev分支
get branch -d dev		//	删除dev分支
get branch -D dev		//	强行删除dev分支

get switch dev			//	切换dev分支(推荐)
git checkout dev		//	切换dev分支

git switch -c dev		//	创建+切换dev分支(推荐)
git checktou -b dev		//	创建+切换dev分支

//	若合并中产生冲突 需要手动解决冲突
git merge dev			//	合并dev到当前分支

git log					//	也可以查看分支情况
git log --graph --pretty=oneline 	

//	不使用[Fast forward]合并分支并且产生一个新的commit
git merge --on-ff -m "message" dev

git stash				//	储存现场
git stash list			//	查看储存列表
git stash apply			//	恢复现场
git stash drop			//	删除列表项
git stash pop			//	还原现场并删除列表项

git cherry-pick 4c805e2	//	复制特定的提交到当前分支

```



实际开发中：
【master】：稳定的分支，用来发布新版本

【dev】:工作分支，版本发布的时候再合并到【master】

不使用[Fast forward]合并会产生一个记录，便于回溯



Bug分支：

当master遇到一个突然的bug时：

```
get stash				//	储存工作现场

git switch master		//	切换到master分支
git switch -c issue-101	//	创建并切换bug分支
//修复bug并且提交
git switch master		//	切换到master分支合并
git merge --on-ff -m "mes" issue-101
git switch dev			//	切换到dev分支

git stash list			//	查看储存列表
git stash pop			//	还原现场并删除列表项

git cherry-pick 4c805e2	//	把dev上也应用这个bug的修复
```

#### 多人协作

```
git remote 				//	查看远程库
git remote -v			//	查看详细

git push origin master	//	推送到远程库中master分支

//	把本地dev分支和远程库关联,方便后续操作
git branch --set-upstream-to=origin/dev dev

git rebase				//	把提交线改为线性
```

#### 标签

```
git tag 				//	查看tag
git show v0.9			//	查看0.9标签的信息

git tag v1.0			//	给当前分支最近的commit打tag
git tag v1.0 f52c633	//	给特定commit打tag
git tag -a v1.0 -m "mes" f52c633	

git tag -d v1.0			//	删除本地tag
git tag origin ：refs/tags/v1.0

git push origin --tags	//	推送本地标签到远程库
```

