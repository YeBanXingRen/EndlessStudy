### git学习手册

#### 	1.git config

```git
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```

​		因为Git是分布式版本控制系统，所以，每个机器都必须自报家门：你的名字和Email地址。

​		注意`git config`命令的`--global`参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址。



	#### 	2.创建版本库

```git
$ git init
Initialized empty Git repository in /Users/michael/learngit/.git/
```

​		使用git init 在当前路径下部署版本库。

#### 	3.add & commit

```git
$ git add file1.txt
$ git commit -m <message>
```

#### 	4.git status  & git diff

```git
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")

*************************************************

$ git diff readme.txt 
diff --git a/readme.txt b/readme.txt
index 46d49bf..9247db6 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1,2 +1,2 @@
-Git is a version control system.
+Git is a distributed version control system.
 Git is free software.
```

​		`git status`命令可以让我们时刻掌握仓库当前的状态，上面的命令输出告诉我们，`readme.txt`被修改过了，但还没有准备提交的修改。

***

​		`git diff`顾名思义就是查看difference.



#### 	5.git log & git reflog

```git
$ git log
commit 1094adb7b9b3807259d8cb349e7df1d4d6477073 (HEAD -> master)
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:06:15 2018 +0800

    append GPL

commit e475afc93c209a690c39c13a46716e8fa000c366
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:03:36 2018 +0800

    add distributed

commit eaadf4e385e865d25c48e7ca9c8395c3f7dfaef0
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 20:59:18 2018 +0800

    wrote a readme file
    
***************************************************
$ git reflog
e475afc HEAD@{1}: reset: moving to HEAD^
1094adb (HEAD -> master) HEAD@{2}: commit: append GPL
e475afc HEAD@{3}: commit: add distributed
eaadf4e HEAD@{4}: commit (initial): wrote a readme file
```

​		git log 可以查看版本控制的历史记录。

​		git reflog 可以查看每一次的命令。

#### 	6.git reset

```git
$ git reset --hard HEAD^
HEAD is now at e475afc add distributed

***************************************************

$ git reset --hard 1094a
HEAD is now at 83b0afe append GPL
```

​		`HEAD^` 	回退一个版本；

​		`HEAD^^ `	回退两个版本。

​		`git reset --hard commit id ` 	回退到指定版本

#### 	7.git checkout

```git
git checkout -- readme.txt
```

`git checkout`其实是用版本库里的版本替换工作区的版本。而在这里命令`git checkout -- readme.txt`意思就是，把`readme.txt`文件在工作区的修改全部撤销,让这个文件回到最近一次`git commit`或`git add`时的状态。

#### 8.git rm

```git
$ git rm test.txt
rm 'test.txt'

$ git commit -m "remove test.txt"
[master d46f35e] remove test.txt
 1 file changed, 1 deletion(-)
 delete mode 100644 test.txt
```

当在工作区删除文件后想在版本库中删除文件，那就用命令`git rm`删掉，__并且`git commit`__

### git连接github

#### 	本地创建ssh密钥并于github建立联系

​		第1步：创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有`id_rsa`和`id_rsa.pub`这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：

```git
$ ssh-keygen -t rsa -C "youremail@example.com"
```

​		你需要把邮件地址换成你自己的邮件地址，然后一路回车。如果一切顺利的话，可以在用户主目录里找到`.ssh`目录，里面有`id_rsa`和`id_rsa.pub`两个文件，这两个就是SSH Key的秘钥对，`id_rsa`是私钥，不能泄露出去，`id_rsa.pub`是公钥，可以放心地告诉任何人。

​		第2步：登录github 进入“Add SSH Key”，填上任意Title，在Key文本框里粘贴`id_rsa.pub`文件的内容，然后点击“Add Key”。

#### 	连接github

* 在github中创建一个本地同名数据仓库

* 把本地仓库与github仓库关联

```git
$ git remote add origin git@github.com:michaelliao/learngit.git
```

​		将`michaelliao`替换成你自己的GitHub账户名。

* 将本地库推送到github上

```git
$ git push -u origin master
```

​		把本地库的内容推送到远程，用`git push`命令，实际上是把当前分支`master`推送到远程。

* 使用github

  ​	从现在起，只要本地作了提交，就可以通过命令：

```
$ git push origin master
```

​		把本地`master`分支的最新修改推送至GitHub。

#### 	删除远程库

​		删除之前先使用`git remote -v`确认本地与远程库的连接状况。

```git
$ git remote -v
origin  git@github.com:YeBanXingRen/learngit.git (fetch)
origin  git@github.com:YeBanXingRen/learngit.git (push)
```

​		然后，根据名字删除，比如删除`origin`：

```
$ git remote rm origin
```

​		此处的“删除”其实是解除了本地和远程的绑定关系，并不是物理上删除了远程库。远程库本身并没有任何改动。要真正删除远程库，需要登录到GitHub，在后台页面找到删除按钮再删除。