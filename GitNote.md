```bash
# change directory
mkdir Git & cd Git

# 通过git init命令把这个目录变成Git可以管理的仓库
git init

# 编写一个文件`readme.txt`
touch readme.txt

# vim 编写一些内容到readme.txt并保存
# 版本一
git add readme.txt
git commit -m "wrote a readme file"

# 修改一下readme.txt再进行上面两个操作
# 版本二
git add readme.txt 
git commit -m "append GPL"

# 再修改一下readme.txt再进行上面两个操作
# 版本三
git add readme.txt 
git commit -m "add distributed"

# 查看历史记录
git log
# 简化的记录
git log --pretty=oneline
# ba7cc2fab82ff1a05e834d35e08276bba7a1b4db (HEAD -> master) add distributed
# 994d2836fe293642544c995f19b863c6298cc728 append GPL
# 25f964396affb36ae596f9378310849feb94726a wrote a readme file

# 下面这个也可以查看commit id
git reflog

```



## 版本操作

* 版本一`wrote a readme file`
* 版本二`append GPL`
* 版本三`add distributed`

```
# version 1
Git is a version control system.
Git is a free software.

# version 2
Git is a version control system.
Git is a free software distribute under the GPL.

# version 3
Git is a distributed version control system.
Git is a free software distribute under the GPL.
```



```bash
# 从版本三回到版本二
git reset --hard HEAD^
# HEAD is now at 994d283 append GPL
cat readme.txt
# Git is a version control system.
# Git is a free software distribute under the GPL.

# 再回到version 3？
# 使用commit id的一部分, 版本号没必要写全，前几位就可以了，Git会自动去找
git reset --hard ba7cc2fa
# HEAD is now at ba7cc2f add distributed
```

## 管理修改

```bash
# 修改readme.txt后保存并add: version 4 
git add readme.txt
# 再修改保存: version 5
# 这次修改的不add，直接执行commit
git commit -m "git tracks changes"

# 那么这次commit成功的是version 4
```

提交后，用`git diff HEAD -- readme.txt`命令可以查看工作区和版本库里面最新版本的区别：

```bash
git diff HEAD -- readme.txt 
# diff --git a/readme.txt b/readme.txt
# index c5c295c..532e6f7 100644
# --- a/readme.txt
# +++ b/readme.txt
# @@ -1,4 +1,4 @@
#  Git is a distributed version control system.
#  Git is a free software distribute under the GPL.
#  Git has a mutable index called stage.
# -Git tracks changes.
# +Git tracks changes of files.
```

## 撤销修改

![image-20200619231828280](/Users/ludyling/Library/Application Support/typora-user-images/image-20200619231828280.png)

### 1. 修改了还没add

给文件添加一句后，还没有add和commit，想要撤销:

```bash
git restore readme.txt 
# or
git checkout -- readme.txt
```

文件内容果然复原了。

`git checkout -- file`命令中的`--`很重要，没有`--`，就变成了“切换到另一个分支”的命令，我们在后面的分支管理中会再次遇到`git checkout`命令。

### 2. 修改并add了

`git status`

```bash
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	modified:   readme.txt
```

Git同样告诉我们，用命令`git reset HEAD `可以把暂存区的修改撤销掉（unstage），重新放回工作区：

## 添加远程库

```bash
git init 
git config --local user.name "ludidiya"
git config --local user.email "lingludi1995@163.com"

# 创建一个名叫LearnGit的远程仓库
curl -u ludidiya https://api.github.com/user/repos-d'{"name":"LearnGit"}'
git remote add origin https://github.com/ludidiya/MscProject.git
git remote add origin git@github.com:ludidiya/LearnGit.git
# 添加后，远程库的名字就是origin，这是Git默认的叫法，也可以改成别的，但是origin这个名字一看就知道是远程库。

# 下一步，就可以把本地库的所有内容推送到远程库上
```

