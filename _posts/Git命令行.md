# Git命令行

### 个人信息

```g
user.name=Deeeeex
user.email=482788525@qq.com
user.password=245cptbtptp
```

```
查看用户名 ：git config user.name
查看密码： git config user.password
查看邮箱：git config user.email
查看配置信息： $ git config --list  
修改用户名
git config --global user.name "xxxx(新的用户名)"
修改密码
git config --global user.password "xxxx(新的密码)"
修改邮箱
git config --global user.email "xxxx@xxx.com(新的邮箱)"
```



### 时光机

创建版本库

```
$ git init
```

新建一个`README.txt`

```
$ git add readme.txt
$ git commit -m "wrote a readme file"
```

`git status`仓库当前状态

`git diff`看当前改动情况

`git log`查看从最近到最远的提交记录

`$ git log --graph --pretty=oneline --abbrev-commit`简化版的`git log`

`git reset --hard HEAD^`回到上一次同理`HEAD^^`为回退两次，`HEAD~100`为回退一百次

`git reflog`是`git log`的升级（包括了之前因为回退而消失的`commit`记录）

`git reset --hard commit_id`可以退回后反悔，重返未来

`git checkout -- file`可以丢弃工作区的修改，让这个文件回到最近一次`git commit`或`git add`时的状态

`git reset HEAD <file>`可以把暂存区的修改撤销掉（`unstage`），重新放回工作区

从版本库中删除该文件，那就用命令`git rm`删掉，并且`git commit`

入股手动误删了但版本库中没删，则可以直接`$ git checkout -- test.txt`



### 远程仓库

要关联一个远程库，使用命令`git remote add origin git@server-name:path/repo-name.git`；

关联一个远程库时必须给远程库指定一个名字，`origin`是默认习惯命名；

关联后，使用命令`git push -u origin master`第一次推送master分支的所有内容；

此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改；

若要解除关联，先用`git remote -v`查看远程库信息，用`git remote rm <name>`命令可以删除

`$ git clone git@github.com:michaelliao/gitskills.git`从远程仓库`clone`



### 分支管理

创建`dev`分支，然后切换到`dev`分支：

```
$ git checkout -b dev
Switched to a new branch 'dev'
```

`git checkout`命令加上`-b`参数表示创建并切换，相当于以下两条命令：

```
$ git branch dev
$ git checkout dev
Switched to branch 'dev'
```

然后，用`git branch`命令查看当前分支：

```
$ git branch
* dev
  master
```

`git branch`命令会列出所有分支，当前分支前面会标一个`*`号

查看分支：`git branch`

创建分支：`git branch <name>`

切换分支：`git checkout <name>`或者`git switch <name>`

创建+切换分支：`git checkout -b <name>`或者`git switch -c <name>`

合并某分支到当前分支：`git merge <name>`

`--no-ff`方式的`git merge`:`$ git merge --no-ff -m "merge with no-ff" dev`

删除分支：`git branch -d <name>`

##### 分支策略

在实际开发中，我们应该按照几个基本原则进行分支管理：

首先，`master`分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；

那在哪干活呢？干活都在`dev`分支上，也就是说，`dev`分支是不稳定的，到某个时候，比如1.0版本发布时，再把`dev`分支合并到`master`上，在`master`分支发布1.0版本；

你和你的小伙伴们每个人都在`dev`分支上干活，每个人都有自己的分支，时不时地往`dev`分支上合并就可以了

**Bug分支**

修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除

当手头工作没有完成时，先把工作现场`git stash`一下

新建分支`issue-101`修复Bug再合并，最后回到dev上用`git stash list`命令查看过去的`stash`

一是用`git stash apply`恢复，但是恢复后，stash内容并不删除，你需要用`git stash drop`来删除；

另一种方式是用`git stash pop`，恢复的同时把stash内容也删了

你可以多次stash，恢复的时候，先用`git stash list`查看，然后恢复指定的stash，用命令：

```
$ git stash apply stash@{0}
```

在master分支上修复的Bug，想要合并到当前dev分支，可以用`git cherry-pick <commit>`命令，把Bug提交的修改“复制”到当前分支，避免重复劳动

**Feature分支**

开发一个新feature，最好新建一个分支

如果要丢弃一个没有被合并过的分支，可以通过`git branch -D <name>`强行删除



### 多人协作

多人协作的工作模式通常是这样：

1. 首先，可以试图用`git push origin <branch-name>`推送自己的修改
2. 如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并
3. 如果合并有冲突，则解决冲突，并在本地提交
4. 没有冲突或者解决掉冲突后，再用`git push origin <branch-name>`推送就能成功

如果`git pull`提示`no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream-to <branch-name> origin/<branch-name>`



### 标签管理

- 命令`git tag <tagname>`用于新建一个标签，默认为`HEAD`，也可以指定一个commit id
- 命令`git tag -a <tagname> -m "blablabla..."`可以指定标签信息
- 命令`git tag`可以查看所有标签
- 命令`git push origin <tagname>`可以推送一个本地标签
- 命令`git push origin --tags`可以推送全部未推送过的本地标签
- 命令`git tag -d <tagname>`可以删除一个本地标签
- 命令`git push origin :refs/tags/<tagname>`可以删除一个远程标签
