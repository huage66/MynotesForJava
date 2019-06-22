

# 1.Git的优势	

​	[Git官网地址](http://git-scm.com/)

​	首先Git是一个分布式的代码托管,也就是说每台电脑上都有项目所有的历史版本及其功能,在自己电脑上就可以进行管理.

​	优势:

![1558489202748](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1558489202748.png)

# 2.Git的执行流程

![1558490647087](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1558490647087.png)

# 3.Git和代码托管中心

![1558490819872](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1558490819872.png)

## 3.1本地库和远程库

**团队内部协作**

​	==首先由A创建自己的本地库,然后在自己的本地库push到代码托管中心的远程库中,然后B再把A所创建的远程库clone一份到自己的本地库中,当B想要提交自己所修改的项目时,我们需要先加入A的团队,然后才能push你的代码.==

![1558491342829](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1558491342829.png)



**跨团队协作**

​	==如何进行跨团队协作呢?首先当团队B想要修改团队A的代码,首先需要fork一份团队A的代码,然后就创建了一个自己的远程库,然后clone到自己的本地库里面,进行修改之后push自己的代码到自己的远程库中,最后pull request自己的远程库给团队A,团队A进行审核,通过之后merage操作把自己的远程库和提交的进行合并操作.==

![1558491539633](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1558491539633.png)



# 4.Git的一些基本命令



## 4.1 本地仓库设置以及签名的设置

**==git init==**	初始化git本地仓库

```shell
#git本地仓库的初始化
$ git init
Initialized empty Git repository in D:/Git/GitDemo/wechat/.git/

#创建.git仓库的文件内容
$ ls -la
total 11
drwxr-xr-x 1 mayn 197121   0 5月  22 10:30 ./
drwxr-xr-x 1 mayn 197121   0 5月  22 10:30 ../
-rw-r--r-- 1 mayn 197121 130 5月  22 10:30 config
-rw-r--r-- 1 mayn 197121  73 5月  22 10:30 description
-rw-r--r-- 1 mayn 197121  23 5月  22 10:30 HEAD
drwxr-xr-x 1 mayn 197121   0 5月  22 10:30 hooks/
drwxr-xr-x 1 mayn 197121   0 5月  22 10:30 info/
drwxr-xr-x 1 mayn 197121   0 5月  22 10:30 objects/
drwxr-xr-x 1 mayn 197121   0 5月  22 10:30 refs/


```



**设置签名**

什么叫签名:就是git为了标识提交项目人员所使用标识符

签名分为俩个

 * **项目级别/仓库级别**

   命令为 **git config user.name 命名**

   ​	     **git config user.email  命名**

   ​	    ==email是为了标识的所以email存不存在都无所谓==

   ```shell
   #创建项目级别签名
   $ git config user.name huage_pro
   
   mayn@DESKTOP-N2QG9TL MINGW64 /d/git/GitDemo/wechat (master)
   
   $ git config user.email huage_pro@qq.com
   
   mayn@DESKTOP-N2QG9TL MINGW64 /d/git/GitDemo/wechat (master)
   $ pwd
   /d/git/GitDemo/wechat
   
   mayn@DESKTOP-N2QG9TL MINGW64 /d/git/GitDemo/wechat (master)
   #查看.git/cofig中是否已经记录我们创建的签名
   $ cat .git/config
   [core]
           repositoryformatversion = 0
           filemode = false
           bare = false
           logallrefupdates = true
           symlinks = false
           ignorecase = true
   [user]
           name = huage_pro
           email = huage_pro@qq.com
   
   ```

   

   

 * **系统级别**

   ​	 **git config -- global user.name 命名**

   ​	 **git config -- global user.email  命名**

   

   ```shell
   #当前路径  系统级别的签名存储在当前系统用户的目录中
   $ pwd
   /c/users/mayn
   
   mayn@DESKTOP-N2QG9TL MINGW64 /c/users/mayn
   #查看隐藏文件夹 .gitconfig  我们可以看到我们所创建的系统级别签名
   $ cat .gitconfig
   [filter "lfs"]
           clean = git-lfs clean -- %f
           smudge = git-lfs smudge -- %f
           process = git-lfs filter-process
           required = true
   [user]
           name = huage_glb
           email = huage_glb@qq.com
   
   ```

   

   **俩个签名的优先级问题**:根据就近原则:当项目级和系统级别都有时,我们使用项目级别,但是不能没有签名,必须拥有俩个签名中的一个用来让git来标识用户

## 4.2 使用git命令提交文件给本地仓库



**提交文件给本地仓库**

 * **git status 查看当前git的状态**

   

   ```shell
   $ git status	#查看git本地仓库当前状态
   On branch master	#在master分支上
   	
   No commits yet		#没有提交记录
   
   nothing to commit (create/copy files and use "git add" to track)
   
   ```

   当我们创建在项目中创建一个文件时,再使用git status

   ![1558495832788](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1558495832788.png)

   有一个红色的标识该文件

 * **git add 文件名        把指定项目或者文件放置到暂存区中**

   ```shell
   $ git add goo.txt
   
   ```

   ![1558495979254](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1558495979254.png)

   ​	然后我们在查看git status 我们可以看见goo,txt变成绿色,表示我们把该文件放入暂存区,但是我们还没提交给git本地仓库

 * **git rm --cached  文件名    把暂存区中的文件移除**

   ```shell
   $ git rm --cached goo.txt
   rm 'goo.txt'
   
   
   ```

   ![1558496114734](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1558496114734.png)

   然后我们在查看git status 我们可以看见我们把goo.txt移除暂存区了

 * **git commit 文件名  提交暂存区中项目到git仓库中**    

   ==注意:git commit提交的文件必须先使用git add命令添加到暂存区中才可以提交,否则提交失败==  这是第一次提文件时

   ```shell
   
   $ git commit goo.txt  #提交暂存区中的文件,把该文件提交到本地仓库中
   [master (root-commit) 631087e] My First commit . new file goo.txt
    1 file changed, 5 insertions(+)
    create mode 100644 goo.txt
   
   ```

   

 * 当我们修改已经提交本地仓库中文件的时候,我们使用get status可以看见  

   ![1558500649181](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1558500649181.png)

   

 * 如果我们提交的文件在本地仓库中已经存在过,那么我们就是来修改该文件的,所以我们可以不用提交到暂存区,可以直接使用git commit命令进行提交

   ```shell
   #使用-m参数我们可以不用进入vim编辑器中来书写标记本次提交的注释
   $ git commit -m "My second commmit.edit file" goo.txt
   [master 388e69a] My second commmit.edit file
    1 file changed, 1 insertion(+), 1 deletion(-)
   
   ```

   

   当我们修改已经提交过的文件的时候,我们可以使用 git checkout 文件名 来进行撤销操作

   ```shell
   $ git checkout good.txt
   
   mayn@DESKTOP-N2QG9TL MINGW64 /d/git/GitDemo/wechat (master)
   $ cat good.txt
   
   foaweiwfoawfawe
   wfoawfjaweio
   `
   awjfwjfoawejf
   fjawiofaweoiawe
   fhiawhfif
   
   
   ```

   

   ==使用git命令来提交项目的步骤==

   ![1558502030677](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1558502030677.png)

   ## 4.3 查看本地仓库的历史版本信息

   

   

   

   1. 查看详细的版本信息:  git log

   ```shell
   #查看本地仓库历史版本信息
   $ git log
   commit 519f474368d2a3cb0d3919afa61ced057fd0eaeb (HEAD -> master) #提交的hash值
   Author: huage_pro <huage_pro@qq.com>	#提交的用户
   Date:   Wed May 22 12:56:03 2019 +0800	#时间
   
       My first commit . new good.txt		#对项目的描述信息
   
   commit 388e69ab2c80b029a292d2b2635e623f286f286d
   Author: huage_pro <huage_pro@qq.com>
   Date:   Wed May 22 12:52:08 2019 +0800
   
       My second commmit.edit file
   
   commit 631087e65a4377576cc5fbd7c6ebb0badf1f382a
   Author: huage_pro <huage_pro@qq.com>
   Date:   Wed May 22 11:36:27 2019 +0800
   
       My First commit . new file goo.txt
   ```

   

   2.查看的版本信息简单一些  :git  log  --pretty=oneline

   ```shell
   
   $ git log --pretty=oneline
   519f474368d2a3cb0d3919afa61ced057fd0eaeb (HEAD -> master) My first commit . new good.txt
   388e69ab2c80b029a292d2b2635e623f286f286d My second commmit.edit file
   631087e65a4377576cc5fbd7c6ebb0badf1f382a My First commit . new file goo.txt
   
   ```

   3.git   log    --oneline

   ```shell
   $ git log --oneline
   519f474 (HEAD -> master) My first commit . new good.txt
   388e69a My second commmit.edit file
   631087e My First commit . new file goo.txt
   
   
   ```

   4.git reflog

   ```shell
   $ git reflog
   519f474 (HEAD -> master) HEAD@{0}: commit: My first commit . new good.txt
   388e69a HEAD@{1}: commit: My second commmit.edit file
   631087e HEAD@{2}: commit (initial): My First commit . new file goo.txt
   
   
   #其中HEAD@{0} 表示需要退回到该版本所需要的步数
   ```

   

   ## 4.4 使用git命令来回退本地仓库历史版本

   

   

   1. git   reset  --hard  索引值   ==既可以回退版本也可以前进版本==

      ```shell
      #索引值通过查看历史版本来查看
      $ git reset --hard 631087e
      HEAD is now at 631087e My First commit . new file goo.txt
      
      mayn@DESKTOP-N2QG9TL MINGW64 /d/git/GitDemo/wechat (master)
      $ cat goo.txt
      faweofawe
      wjfoawfjwaei
      jfoaiwfjawe
      jwofawej
      jawfojfaiwoe
      
      ```

   2. git reset --hard  HEAD^   ==一个^符号表示回退一个版本,n个表示回退n个版本,但是不能前进版本只能回退版本==

   3. git reset --hard HEAD~   ==和前面的命令一模一样,都是只能后退不能前进==

   

   

   ​    **git reset  --参数值**

   ​	==--soft参数,就是本地库中的历史版本被移动,但是暂存区和工作区的历史版本信息并未改变==

   ​	==--mixed参数 ,就是版地库和暂存区中的历史版本被移动,但是工作区历史版本信息并未改变==

   ​	![1558506080218](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1558506080218.png)

   ​	

   ## 4.5删除文件之后如何进行找回

   删除已经提交过本地库的文件如何进行找回呢?

   ​	**因为删除之后提交到本地仓库会形成一个历史版本,也就是在当前版本中,该文件已被删除,但是在上一个版本中,该文件并未被删除所以,只要你后退一个版本该文件就被找回了.**

   ​	所以删除文件找回就是回退到上一个版本没有删除的历史版本就可以找回该文件

   

   ​	**当文件删除但是没有提交到本地仓库的时候,我们可以使用刷新当前版本来进行找回文件**

   ```shell
   git reset --hard HEAD   #刷新当前版本,也就暂存区工作区和本地仓库的版本都会被重置
   ```

   ![1558573777824](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1558573777824.png)

   

   ## 4.6 比较文件差异

   

   ![1558574075566](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1558574075566.png)

   ```shell
   #该命令是把工作区的文件和暂存区中的文件进行比较,如果修改之后的文件提交到暂存区,那么就无法比较了
   $ git diff apple.txt
   diff --git a/apple.txt b/apple.txt
   index 8dd8367..5d77557 100644
   --- a/apple.txt
   +++ b/apple.txt
   @@ -1,4 +1,4 @@
    aaaaa
    bbbbb
   -ccccc								#需要删除的行,也就是原先的行
   +ccccc   fweifhwaefiawefiweawe		#新增的行,也就是修改的行
    ddddd
   
   ```

   ```shell
   #改命令是把工作区中的文件和当前历史版本的本地仓库文件进行比较
   $ git diff HEAD apple.txt
   diff --git a/apple.txt b/apple.txt
   index 5d77557..15f5c80 100644
   --- a/apple.txt
   +++ b/apple.txt
   @@ -1,4 +1,4 @@
    aaaaa
    bbbbb
   -ccccc   fweifhwaefiawefiweawe
   -ddddd
   +ccccc   fweifhwaefiawefiweawfajwe
   +ddddd  wfjwoaeijfweofwewewe
   
   ```

   ==还有一种情况,当git diff 不加文件名的话,我们是比较工作区与暂存区中的所有的文件.==

   

   # 5.Git的分支和主干

   

   **每一个分支都是主干的复制,都是独立存在的**

   ![1558575768169](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1558575768169.png)

   

   

   ![1558576333662](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1558576333662.png)

   

   **创建分支 : git   branch  分支名**

   ```shell
   mayn@DESKTOP-N2QG9TL MINGW64 /d/git/GitDemo/wechat (master)
   $ git branch hot_fix	#创建一个分支为 hot_fix
   
   ```

   **查看分支: git  branch  -v**

   ```shell
   $ git branch -v	
     hot_fix 1fe93d6 edit file apple.txt
   * master  1fe93d6 edit file apple.txt
   
   ```

   **切换分支 : git  checkout  分支名**

   ```shell
   $ git checkout hot_fix		#切换分支,从master切换到hot_fix
   Switched to branch 'hot_fix'
   M       apple.txt
   
   ```

   **合并分支:git  merge  分支名**

   第一步:切换分支到被合并的分支上:git  checkout  [被合并的分支名]

   ```shell
   $ git checkout master
   Switched to branch 'master'
   ```

   第二步:进行合并操作, git  merge  [新添加内容的分支名]

   ```shell
   $ git merge hot_fix	
   Updating 1fe93d6..e08aca8
   Fast-forward
    apple.txt | 6 ++++--
    1 file changed, 4 insertions(+), 2 deletions(-)
   
   ```

   

   **合并冲突:**当我们合并的俩个文件都对某一行做了修改,但是修改的内容不同的时候,这时候冲突就产生了,git不知道要覆盖哪一个,所以就会以冲突的形式表现给我们.



​	冲突的表现:

```shell
$ git merge master	#进行合并产生了冲突,原因:俩个分支都对同一行进行了不同的修改产生了冲突
Auto-merging apple.txt
CONFLICT (content): Merge conflict in apple.txt
Automatic merge failed; fix conflicts and then commit the result.

```

如何解决冲突

第一步:查看冲突的文件

![1558578856973](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1558578856973.png)

第二步:删除特殊符号,然后把文件编辑到你满意的程度即可

```shell

$ cat apple.txt
aaaaa
bbbbb
ccccc   fweifhwaefiawefiweawfajwe
ddddd   wfjwoaeijfweofwewewe]
eeeee    edit hot_fix  hot_fix

```

第三步:把修改好的文件添加到暂存区,然后提交到本地库就可以解决冲突了

```shell
$ git add apple.txt

mayn@DESKTOP-N2QG9TL MINGW64 /d/git/GitDemo/wechat (hot_fix|MERGING)
#注意:当进行冲突解决的时候,提交到本地库不要带文件名,这样会报错
$ git commit -m "resolve conflict " apple.txt
fatal: cannot do a partial commit during a merge.

mayn@DESKTOP-N2QG9TL MINGW64 /d/git/GitDemo/wechat (hot_fix|MERGING)
$ git commit -m "resolve conflict "
[hot_fix 0058108] resolve conflict


```

# 6.远程库的创建以及使用

## 6.1远程库的创建

我们使用github,在该网站上面创建我们的远程库

![1558593805558](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1558593805558.png)

**然后我们在本地库拿到远程库的地址给远程库地址起一个别名 : git  remote  add  [别名]  [远程库地址 ] **

```shell
mayn@DESKTOP-N2QG9TL MINGW64 /d/git/GitDemo/githubdemo (master)
#创建远程库的别名	origin
$ git remote add origin https://github.com/huage66/GitDemo.git

mayn@DESKTOP-N2QG9TL MINGW64 /d/git/GitDemo/githubdemo (master)
#查看所有别名
$ git remote -v
origin  https://github.com/huage66/GitDemo.git (fetch)
origin  https://github.com/huage66/GitDemo.git (push)


```



## 6.2 本地库与远程库的交互操作

**push推送**

**把我们的本地库中的分支推送到远程库中: git push [ 远程库地址]  [本地库分支名]**

```shell
#把刚起的别名的地址用上
#也就是把本地库中的master分支推送到远程库中
#执行命令之后需要登录你的github账户和密码
$ git push origin master
Counting objects: 3, done.
Delta compression using up to 12 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 252 bytes | 252.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://github.com/huage66/GitDemo.git
 * [new branch]      master -> master

```

**clone复制**

**从远程库中复制项目:  git clone [远程库项目地址]**

```shell

$ git clone https://github.com/huage66/GitDemo.git
Cloning into 'GitDemo'...
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 3 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), done.

```

==使用git clone命令复制过来的项目有三大特点==

 	1. **完整的复制远程库的所有文件.**
 	2. **复制远程库中项目的所有别名**
 	3. **复制过来直接初始化好本地库,不需要再重新创建**

​	

**如何让不是团队成员的人加入团队并推送自己的分支项目:**

登录项目所有者得github,进入该项目,点击项目settings

!1558595531393](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1558595531393.png)

![1558595752042](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1558595752042.png)

==然后加入团队之后再使用  git push  [远程库链接地址]  [分支名]   就可以推送自己的项目到该远程库中了==



**pull命令: git pull  [远程库地址]**   拉取操作,也就是把远程库中的内容拉取到本地库中,和本地库进行合并操作,本地库是被合并方

**pull = fetch+merge 的总和**

==pull等于fetch加上merge命令是因为,当拉取之后进行合并时出现冲突,所以我们先进行拉取查看内容是否有冲突,没有再进行合并,这样可以保证安全==

**git fetch [远程库地址] [远程库的分支名]**   拉取远程库中的内容到本地库,但是不进行合并操作

**git merge [远程库地址/远程库分支名]**  把当前本地库和拉取到的远程库的内容进行合并操作

```shell
$ git fetch origin master
remote: Enumerating objects: 10, done.
remote: Counting objects: 100% (10/10), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 6 (delta 1), reused 6 (delta 1), pack-reused 0
Unpacking objects: 100% (6/6), done.
From https://github.com/huage66/GitDemo
 * branch            master     -> FETCH_HEAD
   2e70eab..4010063  master     -> origin/master

mayn@DESKTOP-N2QG9TL MINGW64 /d/git/GitDemo/githudemo2/GitDemo (master)
$ git merge origin/master
Updating 2e70eab..4010063
Fast-forward
 demo.txt | 1 +
 1 file changed, 1 insertion(+)


```

## 6.3 本地库与远程库之间出现冲突的解决方案

​	首先:本地库与远程库**如何出现冲突:**

​	==在一个团队中,如果有一个成员推送(push)了项目,然后你的推送所修改的内容行和他所修改内容的行号是一致的,这时就会出现冲突,你所推送的版本就不能进行提交==

​	**如何解决该冲突**

​	**第一步:**使用pull命令,把远程库的版本拉取到本地库中

​	**第二步:**查看冲突的文件,删除掉特殊符号,然后修改到满意程序,再次进行推送(push)操作即可



## 6.4 如何进行跨团队操作

​	首先,不在我们团队里面人,我们把该团队里面的项目进行fork一份,然后我们在到本地使用git把该项目clone一份到修改之后在使用push命令提交到自己所fork的那个远程库中.

​	然后在new  pull  request

​	之后我们在团队里面收到他的pull request进行审核之后我们把提交来的项目和原项目进行合并merge操作



## 6.5 使用ssh的远程库地址来进行本地库和远程库的操作

​	==使用ssh地址,我们可以每次进行push或者pull操作的时候,不用每次都验证github账号,但是使用该办法,我们需要注意的是,ssh一台计算机只能使用一个.==

首先切换目录到根目录,然后删除.ssh

```shell
$ cd ~

mayn@DESKTOP-N2QG9TL MINGW64 ~
$ pwd
/c/Users/mayn

mayn@DESKTOP-N2QG9TL MINGW64 ~
$ rm -rvf .ssh

```

然后生成.ssh密钥目录

```shell
$ ssh-keygen -t rsa -C zxhit_com@163.com
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/mayn/.ssh/id_rsa):
Created directory '/c/Users/mayn/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /c/Users/mayn/.ssh/id_rsa.
Your public key has been saved in /c/Users/mayn/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:6fz1kKZhpLbDCLizq5JBhBuQ50EwvX9adikQDMuxsxQ zxhit_com@163.com
The key's randomart image is:
+---[RSA 2048]----+
|*=Eo             |
|+++=o            |
|.=*o .           |
|.ooo.    .       |
|. .o .  S..      |
|. . o =ooo   .   |
| o . * == o =    |
|o o . ..o+ = o   |
|o.o+    ..o   .  |
+----[SHA256]-----+

```

切换到 ~/.ssh目录中,查看id_rsa.pub  , 得到ssh密钥

```shell
$ cd ~/.ssh
mayn@DESKTOP-N2QG9TL MINGW64 ~/.ssh
$ cat id_rsa.pub  #查看生成的ssh密钥
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDRSrmlSYg0BCEqZ/g3Jy3yk/cJbXlVGlsjtKDv0+FOpUwcrxPmAyQLD+PpXDKkKSdRb85be9gZf1TKF+KPi77E6PyWXjVKa7lsJqeyuiYgei83bNWsybI+/aBhufCeFex4Ejr+PAEUTGeb6XOGEJyAsAGxaHoDXvvC3no7KBYkA0GBsd5rCyED3akhSdZRqwRXwalC7eSB5iOYhDfVE4zB6lDcmgPd3sq+wcAff5IJs2gJfNDaYZlO35AZ2qAvve6lA0U6597EFYSZDFZtrTi5589kkE2gNTtqTeRVh5TC+ij2G6xkVtFdvgI1GKSZYsvRif54p4Y4Q5SNXhqij/9b zxhit_com@163.com

```

然后在github上面设置ssh免密密钥,把查到的密钥填入里面

![1558604720355](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1558604720355.png)

最后就可以使用ssh的远程库链接地址了进行push操作



# 7.使用idea来git操作

## 7.1使用idea进行本地库的创建以及推送(push)

[在idea中使用git](https://blog.csdn.net/qq_37677519/article/details/76168640)

# 8.Git工作流

![1558608881267](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1558608881267.png)

# 9 Gitlab服务器的搭建以及使用

Gitlab的作用:我们自己手动搭建的git远程库服务器

## 9.1Gitlab的搭建



Gitlab服务器搭建的命令

```shell
sudo rpm -ivh /opt/gitlab-ce-10.8.2-ce.0.el7.x86_64.rpm
sudo yum install -y curl policycoreutils-python openssh-server cronie
sudo lokkit -s http -s ssh
sudo yum install postfix
sudo service postfix start
sudo chkconfig postfix on
curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.rpm.sh | sudo bash
sudo EXTERNAL_URL="http://gitlab.example.com" yum -y install gitlab-ce
```

使用脚本执行批处理命令

```shell
vim install.sh
#然后把服务器搭建命令放入该文件中
#修改文件权限,使该文件变成可执行文件
chmod 755 install.sh
#该命令来执行批处理操作
./install.sh
```



安装Gitlab可能出现的问题

当出现什么yumID被锁定

我们可以使用命令来删除yum ID

```shell
rm -f /var/run/yum.pid
```

当安装出现尝试其他镜像的提示的时候,我们可以使用该命令来进行解决

```shell
yum clean all
rpm --rebuilddb
```



## 9.2 Giltlab的配置

使用gitlab-ctl  reconfigure    执行默认的gitlab配置

```shell
gitlab-ctl reconfigure
```

然后启动gitlab服务器  gitlab-ctl start

```shell
[root@localhost ~]# gitlab-ctl start
ok: run: alertmanager: (pid 16091) 62s
ok: run: gitaly: (pid 15871) 65s
ok: run: gitlab-monitor: (pid 15952) 64s
ok: run: gitlab-workhorse: (pid 15906) 65s
ok: run: logrotate: (pid 15255) 113s
ok: run: nginx: (pid 15130) 119s
ok: run: node-exporter: (pid 15940) 64s
ok: run: postgres-exporter: (pid 16114) 62s
ok: run: postgresql: (pid 14545) 160s
ok: run: prometheus: (pid 16055) 63s
ok: run: redis: (pid 14191) 172s
ok: run: redis-exporter: (pid 15965) 63s
ok: run: sidekiq: (pid 14962) 127s
ok: run: unicorn: (pid 14876) 133s

```

## 9.3使用浏览器访问Gitlab服务器

使用linux的ip地址来直接访问Gitlab服务器

访问之前,我们需要关闭linux的防火墙

```shell
[root@localhost ~]# service firewalld stop
Redirecting to /bin/systemctl stop firewalld.service

```

然后才可以访问

![1558614021662](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1558614021662.png)

然后设置账户和密码

账户:root

密码:zxh671251998

