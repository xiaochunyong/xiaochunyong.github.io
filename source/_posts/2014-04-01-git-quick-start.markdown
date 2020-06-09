---
layout:     post
title:      "Git 快速入门"
subtitle:   "Git 快速入门"
date:       2014-04-01
author:     "Ely Xiao"
header-img: "img/common.jpg"
catalog: true
tags:
    - git
---
> Updated by 2019-08-21 15:51
 
## 安装
```
// Ubuntu
$ add-apt-repository ppa:git-core/ppa
$ apt-get update
$ apt-get install git

// macOS
$ brew install git
```

## 概念
### 回收站(stash)
> 我是觉得停像回收站的,其实大家每天都有用到.   如果你已经修改了某几个文件(这几个文件已经被git管理了)  这时候你要执行git pull,  图形客户端,可能会提示你要stash
	git stash 命令会将你所做的修改,暂存区的内容 全部扔到stash 区, 把你的工作区和暂存区还原到上一次push完后的样子,当你pull完成后, git stash pop又可以将你
	之前的修改还     原到工作区和暂存区 

### 工作区(workspace)
> 我们编写代码的地方
	
### 暂存区(index)
> 文件要提交到本地版本库,就必须先添加到暂存区,也可以把暂存区当做一个提交列表
	
### 本地版本库(local repository)


### 远程版本库(remote repository)
>origin : origin 是 remote 的一个短名称,在执行 git remote add origin git@github.com:xxxx/xx.git   
	master : 主分支 与我们自己创建的分支没有什么不同,只是约定俗称的把它当主分支而已.
	HEAD : 永远指向当前分支,其实是  .git/HEAD文件
	.gitignore : 大家都不希望被git管理的文件可以写在这个文件里面
	.git/info/exclude : "我"不希望被git管理的文件可以写在这个文件里面



## Git 常用操作

######  discard 撤销已经修改的文件
```
$ git checkout -- <file>              # 撤消指定的未暂存文件的修改内容
$ git checkout HEAD <file>            # 撤消指定的未提交文件的修改内容
$ git reset --hard HEAD             # 撤消工作目录中所有未提交文件的修改内容
```
 
###### stage
```
$ git add .
$ git add *
$ git add -A
$ git add <file/dir>
```
###### unstage
```
$ git reset HEAD <file>...
```
###### commit
```
$ git commit -m "comment"
```
###### stage & commit
```
$ git commit -a -m "comment"
```
###### amend commit (修正commit)
```
$ git commit --amend -m "new commit message"
```
###### rollback commit
```
$ git reset --soft HEAD^                # 回到最新的提交之前,文件将回到暂存区

$ git push origin master                # 上传代码到远程版本库
```

###### rollback push (还没测试)
今天因为某些尚不明了的问题导致，博客的Git pages始终构建失败，于是想在远程版本库中删除前几次提交。在该网页上找到了解决方案：
```
$ git reset --hard <commit id>    # 本地回退到指定commit id, --hard 抛弃当前工作区的修改, --soft 保留当前工作区的修改 
$ git push --force # 强制提交到远程版本库，覆盖提交记录
```

###### delete untracked files
```
$ git clean -f
```
###### delete untracked files and dirs
```
$ git clean -fd
``` 
###### 连 gitignore 的untrack 文件/目录也一起删掉 （慎用，一般这个是用来删掉编译出来的 .o之类的文件用的）
```
$ git clean -xfd
``` 
###### 在用上述 git clean 前，墙裂建议加上 -n 参数来先看看会删掉哪些文件，防止重要文件被误删
```
$ git clean -nxfd
$ git clean -nf
$ git clean -nfd
```
### 其它操作
```
$ git status
$ git status -s
$ git diff              # 查看变更内容 工作区与暂存区比较
$ git diff HEAD             # 查看变更内容 工作区与本地版本库比较
$ git diff --cached             # 查看变更内容 暂存区与本地版本库比较
$ git mv <old> <new>            # 文件改名
$ git rm <file>               # 删除文件
$ git rm --cached <file>          # 停止跟踪文件但不删除
```


### Tag & Branch
###### 标签
```
$ git tag           # 列出所有本地标签
$ git tag <tagname>           # 基于最新提交创建标签
$ git tag -d <tagname>            # 删除标签
$ git push --tags           # 上传所有标签
 ```
###### 分支
```
# 显示所有本地分支
$ git branch
# 显示所有远程分支
$ git branch -r         
# 显示所有分支
$ git branch -a         
# 基于最新提交创建新分支
$ git branch <new-branch>        
# 基于当前分支创建新分支, 并切换到新分支
$ git checkout -b <new-branch> 
# 基于指定提交创建新分支,并切换到新分支
$ git checkout <commit_id> -b <newbranch>  
# 从远程分支创建一个新的本地分支(重命名分支, 同事用中文分支, 恶心的时候可以用)
$ git checkout -b improve-0611 origin/优化0611
# 切换到指定分支或标签
$ git checkout <branch/tag>           

# 删除本地分支
$ git branch -d <branch>  
# 强制删除本地分支
$ git branch -D <branch> 
# 删除远程分支	
$ git branch -r -d origin/branch-name
$ git push origin :branch-name
# 实例(删除uat分支):
$ git branch -D uat
$ git branch -r -d origin/uat
$ git push origin :uat
```
	
 
 
###### 分支的相关操作
```
$ git cherry-pick <commit_id>         # 挑选某个commit进行合并
$ git merge <branch>          #合并指定分支到当前分支
$ git rebase <branch>         #衍合指定分支到当前分支
```



### Log
```
$ git log           # 查看提交历史
$ git log -n            # 查看最近n次提交历史
$ git log -p            # 查看提交历史,以patch形式显示
$ git log filename          # 查看指定文件的提交历史
$ git log --graph
$ git log --oneline                                                     # 等价于 git log --pretty=oneline --abbrev-commit
$ git log --pretty=oneline
$ git log --pretty=format:"%h - %an, %ar : %s"
$ git blame <file>  #以列表方式查看指定文件的提交历史(通过这个,可以看到每一行是谁改的)
```
 
### 常用组合命令
```
$ git log <commit_id> -p --stat               #以补丁形式 & 统计形式  查看指定提交
$ git log <file> -n -p --stat                     #以补丁形式 & 统计形式  查看指定文件 最近n次提交
 
$ git log  -n -p --stat                             #以补丁形式 & 统计形式  查看最近n次提交

```



### Stash
```
$ git stash                                                                             # 保存进度 默认相当于 git stash save
$ git stash [save [--patch] [-k|--[no-]keep-index] [-q|--quiet] [<message>]]
$ git stash list                                                                        # 列出进度
$ git stash pop [--index] [<stash>]                                                       # 恢复进度,如果指定了--index,将尝试恢复暂存区,并将恢复的进度从存储的工作进度列表中删除
$ git stash apply [--index] [<stash>]                                                 # 恢复进度,如果指定了--index,将尝试恢复暂存区,不删除
$ git stash drop [<stash>]
$ git stash clear
$ git stash branch <branchname> <stash>
 
 
例:

$ git stash save "stash for pull"
$ git stash pop stash@{0}
```


### Git 初始化
###### 路线一
```
$ mkdir app
$ cd app
$ git init
$ git remote add origin git@github.com:xxx/app.git
 ```
###### 路线二
```
$ git clone git@github.com:xxx/app.git
 ```
###### git remote 相关操作
```
$ git remote
$ git remote -v
$ git remote show origin
$ git remote rename origin <newname>
$ git remote rm origin
```



### 附录：dff 详解
```
$ diff -u hello world > diff.txt
--beginoutput
 hello   2014-04-11 10:43:27.642739542 +0800     #表示原始文件名 & 时间戳
+++ world   2014-04-11 10:45:55.038735015 +0800     #+++表示目标文件名 & 时间戳
@@ -1,4 +1,4 @@                         #差异小节-差异定位语句(前后用两个@进行标识) -1,4表示原始文件中的第一行开始的4行
-应该杜绝文章中的错别字.                            #以-号开始的行是只出现在原始文件中的行
+应该杜绝文章中的错别子.                            #以+号开始的行是只出现在目标文件中的行
 
   
 但是无论使用                                       #以空格开始的行是在原始文件与目标文件都出现的行
@@ -7,6 +7,7 @@
 是人就有可能犯错,软件更是如此.
-犯了错,就要扣工资!
-
 改正的成本可能会很高.
+
+但是"只要眼球足够多,所有BUg都好捉",
+这就是开源的哲学之一.
```

Question 1: 执行 git diff 提示 old mode & new mode

```
old mode 100755  
new mode 100644
``` 

Solution:
```
git config core.filemode false
```
详情:[http://stackoverflow.com/questions/1257592/how-do-i-remove-files-saying-old-mode-100755-new-mode-100644-from-unstaged-cha](http://stackoverflow.com/questions/1257592/how-do-i-remove-files-saying-old-mode-100755-new-mode-100644-from-unstaged-cha)

### [Git vs SVN](http://www.vaikan.com/5-fundamental-differences-between-git-svn/)

我是一開始就用Mercurial, Git這類的系統。（現在已經百分百用Git了。）
 用過Git之後才接觸SVN，發現了一些非常大的差別。在這裡提出我個人一些主要理由為何棄SVN而用Git。

 1。速度：
 克隆一份全新的目錄，以同樣擁有五個（才五個）分支來說，SVN是同時複製5個版本的文件，也就是說重複五次同樣的動作。而Git只是獲取文件的每個版本的元素，然後只載入主要的分支（master）。在我的經驗，克隆一個擁有將近一萬個提交（commit），五個分支，每個分支有大約1500個文件的SVN，耗了將近一個小時！而Git只用了區區的1分鐘！

 2。版本庫（repository）：
 據我所知，SVN只能有一個指定中央版本庫。當這個中央版本庫有問題時，所有工作成員都一起癱瘓直到版本庫維修完畢或者新的版本庫設立完成。

 而Git可以有無限個版本庫。或者，更正確的說法，每一個Git都是一個版本庫，區別是它們是否擁有活躍目錄（Git Working Tree）。如果主要版本庫（例如：置於GitHub的版本庫）發生了什麼事，工作成員仍然可以在自己的本地版本庫（local repository）提交，等待主要版本庫恢復即可。工作成員也可以提交到其他的版本庫！

 3。分支（Branch）
 在SVN，分支是一個完整的目錄。且這個目錄擁有完整的實際文件。如果工作成員想要開啟新的分支，那將會影響“全世界”！每個人都會擁有和你一樣的分支。如果你的分支是用來進行破壞工作（安檢測試），那將會像傳染病一樣。

 而Git，每個工作成員可以任意在自己的本地版本庫開啟無限個分支。舉例：當我想嘗試破壞自己的程序（安檢測試），並且想保留這些被修改的文件供日後使用，我可以開一個分支，做我喜歡的事。完全不需擔心妨礙其他工作成員。只要我不合并及提交到主要版本庫，沒有一個工作成員會被影響。等到我不需要這個分支時，我只要把它從我的本地版本庫刪除即可。無痛無癢。
 Git的分支名是可以使用不同名字的。例如：我的本地分支名為testing，而在主要版本庫的名字其實是master。
 最值得一提，我可以在Git的任意一個提交點（commit point）開啟分支！（其中一個方法是使用gitk –all 可觀察整個提交記錄，然後在任意點開啟分支。）

 4。提交（Commit）
 在SVN，當你提交你的完成品時，它將直接記錄到中央版本庫。當你發現你的完成品存在嚴重問題時，你已經無法阻止事情的發生了。如果網路中斷，你根本沒辦法提交！

 而Git的提交完全屬於本地版本庫的活動。而你只需“推”（git push）到主要版本庫即可。Git的“推”其實是在執行“同步”（Sync）。

 5。重新設立起點（Rebase）
 我沒在SVN嘗試過，不知道有沒有這樣的功能。

 在Git，如果你想把別人的最新提交設立為現在這個分支的起點，只要執行git rebase branch_name 即可。這個和合并（merge）不同點是，merge會依據修改的時間視為最新，而Rebase會要求你去解決雙方都有修改過的地方的矛盾（conflict）。

 A - B - E
 \- C - D
 A - B - E
 \ - C - D

 6。系統檔案
 SVN會在每一個目錄置放一個.svn。如果想移除這些.svn是很累的。
 而Git會在目錄起點擁有一個.git目錄，以及.gitignore。

 對我而言，管理一個Git 的版本庫是很容易的事。


### Git 学习资料
[Pro Git 在线版](https://git-scm.com/book/zh/v2)
[Pro Git Github](https://github.com/progit/progit/tree/master/zh)

[http://ndpsoftware.com/git-cheatsheet.html](http://ndpsoftware.com/git-cheatsheet.html)

