### Git常用命令
```
git config --global user.name

git config --global user.email

git config --global credential.helper store

git config --global --list

git init    git clong   git status  git add

git commit -m  

git rm --cached git log git log oneline

git reset --soft/hard/mixed

git ls-files

git reflog

git commit --amend -m

git diff

git rm

git commit -am

git status -s

git push

git remote add git remote -v git remote remove

git pull
```

##### 2024.5.24
配置用户名和邮箱
```
git config --global user.name "qingran"

git config --global user.email 1436784248@qq.com

git config --global credential.helper store

git config --global --list
```
省略(local):本地配置，只对本地仓库有效
--global : 全局配置，所以仓库有效
--system : 系统配置，对所有用户有效
```
所有配置项，都存储在纯文本文件中，所以git config命令其实只是提供便捷的命令行接口，通常，你只需要在新机器上配置一次Git安装，以及，你通常会想要使用--global标记

Git将配置项保存在三个独立的文件中，允许你分别对单个仓库，用户，整个系统设置

/.git/config        特定仓库设置
~/.gitconfig        特点用户设置，也是--global标记的设置项存放的位置
/etc/gitconfig      系统层面的配置
```

<u>当这些文件中配置项发生冲突时，本地仓库设置覆盖用户设置，用户设置覆盖系统设置</u>

git init 在当前目录创建一个Git仓库
git init x 在x指定目录创建一个Git仓库
git clone path 当前在那个目录就克隆到那个目录

**工作区域和文件状态**
![截屏2024-05-24 15.58.28](assets/%E6%88%AA%E5%B1%8F2024-05-24%2015.58.28.png)

![截屏2024-05-24 15.59.30](assets/%E6%88%AA%E5%B1%8F2024-05-24%2015.59.30.png)

```
git inti 创建仓库

git status 查看仓库状态

git add "file or dir" 添加到暂存区(还可以使用通配符*)

git commit 提交 (只会提交已经被跟踪的文件也就是放在暂存区的文件)
```

**提交到暂存区的文件可以从暂存区里删除,注意这里的删除不是真正意义上的删除，是将已经暂存的文件也就是已经被跟踪的文件变成未跟踪的文件**
`use "git rm --cached <file>..." to unstage`

```
git commit -m "提交的信息" 信息会被记录到仓库中
如果这里没有加 -m 会进入命令行交互界面，默认使用vim提交信息
```

git log 查看提交记录(版本记录)
git log --oneline 查看简洁提交记录

```
c6d1f0a (HEAD -> main) 4 commit (当前HEAD指针所指的对象)
2011932 3 commit
b34d347 2 first commit2 first commit
ba625e5 1 first commit (ba625e5 版本号)

```

![截屏2024-05-24 16.36.35](assets/%E6%88%AA%E5%B1%8F2024-05-24%2016.36.35.png)

git reset 默认是 --mixed

**执行 gir reset --soft**
```
qingran@qingrandeMacBook-Pro LearnGit % git log --oneline
c6d1f0a (HEAD -> main) 4 commit
2011932 3 commit
b34d347 2 first commit2 first commit
ba625e5 1 first commit
qingran@qingrandeMacBook-Pro LearnGit % git reset --soft b34d347 
qingran@qingrandeMacBook-Pro LearnGit % git log
commit b34d3476ea9add24d57256ba28143f44c1a78fe8 (HEAD -> main)
Author: qingran <1436784248@qq.com>
Date:   Fri May 24 16:29:38 2024 +0800

    2 first commit2 first commit

commit ba625e522aea36b0f890ad76fc410c8f7e2b74f1
Author: qingran <1436784248@qq.com>
Date:   Fri May 24 16:24:14 2024 +0800

    1 first commit


git reset --soft b34d347 (软回退，本地文件和暂存区都有回退之前基于b34d347这个版本新添加的文件) (HEAD指针指向b34d347)

qingran@qingrandeMacBook-Pro LearnGit % ls
file1.txt	file2.txt	file3.txt	file4.txt
qingran@qingrandeMacBook-Pro LearnGit % git ls-files
file1.txt
file2.txt
file3.txt
file4.txt

可以看到本地文件和暂存区也就是被跟踪的文件都有

git ls-files 查看已经被跟踪的文件

qingran@qingrandeMacBook-Pro LearnGit % git status
On branch main
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	new file:   file3.txt
	new file:   file4.txt

显示 new file 是因为这些文件是第三个版本才添加的，所以相对于第二个版本来说是新文件
```

**执行 git reset --hard, git reset --mixed**

```
git reset --hard
直接将本地文件都删除了(小心)

git reset --mixed
本地文件还存在，但是文件变成了未跟踪状态
```

**如果误操作了也没有关系，因为Git所以操作都是可以回溯的，可以使用 git reflog 命名来查看我们的操作的历史记录，然后找到误操作之前的版本号，再一次执行 git reset --hard c6d1f0a，回退到误操作之前的版本**

```
qingran@qingrandeMacBook-Pro LearnGit % git reflog
b34d347 (HEAD -> main) HEAD@{0}: reset: moving to b34d347
c6d1f0a HEAD@{1}: commit: 4 commit
2011932 HEAD@{2}: commit: 3 commit
b34d347 (HEAD -> main) HEAD@{3}: commit: 2 first commit2 first commit
ba625e5 HEAD@{4}: commit (initial): 1 first commit
qingran@qingrandeMacBook-Pro LearnGit % git log --oneline
b34d347 (HEAD -> main) 2 first commit2 first commit
ba625e5 1 first commit
qingran@qingrandeMacBook-Pro LearnGit % git reset --hard c6d1f0a
HEAD is now at c6d1f0a 4 commit
qingran@qingrandeMacBook-Pro LearnGit % git log --oneline
c6d1f0a (HEAD -> main) 4 commit
2011932 3 commit
b34d347 2 first commit2 first commit
ba625e5 1 first commit
qingran@qingrandeMacBook-Pro LearnGit % 
```

**注意，即是当我们使用 git reset --hard b34d347,回退到之前版本，即使当前文件己经从本地删除，但是我们依然可以使用 git reglog 找到之前的版本号，git reset --hard c6d1f0a回退到之前的版本状态，文件也都存在**

##### 2024.5.25
git commit --amend -m "change the most recently submitted"
修改最近一次提交(包括文件的修改，以及提交信息)

```
git log --oneline 不会看到增加一次提交记录，但是在 git reflog 还是可以看到之前的版本号
```

**如果要修改之前提交的内容需要用rebase**

![截屏2024-05-25 14.26.58](assets/%E6%88%AA%E5%B1%8F2024-05-25%2014.26.58.png)

**git diff 默认比较工作区和暂存区的差异**
```
qingran@qingrandeMacBook-Pro LearnGit % git diff
diff --git a/file1.txt b/file1.txt
index cf9767c..f5f7317 100644
--- a/file1.txt
+++ b/file1.txt
@@ -7,3 +7,5 @@
 4 change
 
 5 change
+
+git diff
```

第一行提醒我们发生变更的文件
第二行是哈希算法生成的哈希值 100644 文件的权限

**git diff HEAD 查看工作区和版本库的差异**
```
HEAD 表示HEAD指向的版本
HEAD^ HEAD~ 表示HEAD之前的一个版本
HEAD~2 表示HEAD之前的两个版本
```

**git diff --cached 比较暂存区和版本库的差异**

**git diff HEAD^ HEAD 比较上一个版本和当前版本的差异**

**git diff HEAD^ HEAD file只比较上一个版本和当前版本的file文件差异**

**git diff branch1 branch2查看两个分支的差异**

![截屏2024-05-25 15.59.00](assets/%E6%88%AA%E5%B1%8F2024-05-25%2015.59.00.png)

**删除文件git rm**

```
qingran@qingrandeMacBook-Pro ~ % cd Desktop/Git/LearnGit
qingran@qingrandeMacBook-Pro LearnGit % ls
file1.txt	file3.txt	file5.txt
file2.txt	file4.txt	gitle.md
qingran@qingrandeMacBook-Pro LearnGit % rm file1.txt
qingran@qingrandeMacBook-Pro LearnGit % ls
file2.txt	file3.txt	file4.txt	file5.txt	gitle.md
qingran@qingrandeMacBook-Pro LearnGit % git status
On branch main
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	deleted:    file1.txt

no changes added to commit (use "git add" and/or "git commit -a")

//这里只是删除了本地的文件，但是暂存区还存在，这里的删除相当于修改，所以要提交一下到暂存区

qingran@qingrandeMacBook-Pro LearnGit % git ls-files
.gitignore
file1.txt
file2.txt
file3.txt
file4.txt
file5.txt
gitle.md
qingran@qingrandeMacBook-Pro LearnGit % git add file1.txt
qingran@qingrandeMacBook-Pro LearnGit % git ls-files
.gitignore
file2.txt
file3.txt
file4.txt
file5.txt
gitle.md
qingran@qingrandeMacBook-Pro LearnGit % git rm file2.txt
rm 'file2.txt'
qingran@qingrandeMacBook-Pro LearnGit % git status
On branch main
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	deleted:    file1.txt
	deleted:    file2.txt

//可以看到为绿色，所以已经帮修改(删除)后的文件相当于提交到暂存区

qingran@qingrandeMacBook-Pro LearnGit % ls
file3.txt	file4.txt	file5.txt	gitle.md
qingran@qingrandeMacBook-Pro LearnGit % git ls-files
.gitignore
file3.txt
file4.txt
file5.txt
gitle.md
qingran@qingrandeMacBook-Pro LearnGit % git commit -m "delete file1/2.txt"
[main b2b3414] delete file1/2.txt
 2 files changed, 12 deletions(-)
 delete mode 100644 file1.txt
 delete mode 100644 file2.txt
 
 //最后还要提交一下，帮仓库里面的文件给删了
 
 qingran@qingrandeMacBook-Pro MyFirstRepo % git push
 
 //再执行 git push 帮 GitHub 上面的也给删除
```

##### 2024.5.26
![截屏2024-05-26 13.24.39](assets/%E6%88%AA%E5%B1%8F2024-05-26%2013.24.39.png)
![截屏2024-05-26 13.25.07](assets/%E6%88%AA%E5%B1%8F2024-05-26%2013.25.07.png)
```
qingran@qingrandeMacBook-Pro LearnGit % touch hello.log
qingran@qingrandeMacBook-Pro LearnGit % vim hello.log
qingran@qingrandeMacBook-Pro LearnGit % vim .gitignore
qingran@qingrandeMacBook-Pro LearnGit % git status
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   .gitignore

no changes added to commit (use "git add" and/or "git commit -a")
qingran@qingrandeMacBook-Pro LearnGit % git commit -am "ignore log"
[main cc75f0c] ignore log
 1 file changed, 1 insertion(+)
qingran@qingrandeMacBook-Pro LearnGit % ls
file3.txt	file4.txt	file5.txt	gitle.md	hello.log
qingran@qingrandeMacBook-Pro LearnGit % git ls-files
.gitignore
file3.txt
file4.txt
file5.txt
gitle.md
```
**git commit -am 将已经跟踪的文件直接提交到版本库里，相当于执行了两个操作，git add and git commit -m**

<u>注意如果有.log先添加到版本控制中，此时.gitignore中*.log对这个文件是没有作用的</u>

.gitignore也可以配置文件夹(目录),如果文件夹下什么都没有，则不会被放入版本控制中,**而且忽略文件夹要以斜线结尾**`temp/`

`git status -s`查看状态的简写(s为short的缩写)
```
qingran@qingrandeMacBook-Pro LearnGit % git status -s
 M file3.txt
```
##### 2024.5.30
**创建远程仓库然后克隆到本地**
![截屏2024-05-30 10.33.32](assets/%E6%88%AA%E5%B1%8F2024-05-30%2010.33.32.png)
创建好GitHub仓库之后
`https://github.com/qrgwh123/MyFirstRepo.git`**推送(push)的时候需要验证用户名和密码**

`git@github.com:qrgwh123/MyFirstRepo.git`**SSH协议推送push的时候，不需要验证用户名和密码，但是需要配置GitHub上面的SSH公钥(推荐)**

![截屏2024-05-30 11.06.46](assets/%E6%88%AA%E5%B1%8F2024-05-30%2011.06.46.png)
左上角显示的是这个仓库的分支(main,Branch),和Tags

**配置SSH公钥**
```
qingran@qingrandeMacBook-Pro ~ % cd Desktop/Git
qingran@qingrandeMacBook-Pro Git % cd 
qingran@qingrandeMacBook-Pro ~ % cd .ssh
cd: no such file or directory: .ssh
qingran@qingrandeMacBook-Pro ~ % mkdir .ssh
qingran@qingrandeMacBook-Pro ~ % cd .ssh
qingran@qingrandeMacBook-Pro .ssh % ssh-keygen -t rsa -b 4096

//-t 指定协议为rsa -b 指定生成的大小为4096
//第一次创建SSH密钥，全部回车即可

Generating public/private rsa key pair.
Enter file in which to save the key (/Users/qingran/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /Users/qingran/.ssh/id_rsa
Your public key has been saved in /Users/qingran/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:S1Zpb14SW/WHbyUa6gC3HhQ+SMcSSL4NRRe71d9hZko qingran@qingrandeMacBook-Pro.local
The key's randomart image is:
+---[RSA 4096]----+
|   ..o=o=.      .|
|   ..o.=.o o   o.|
|    o o.* = +E+=+|
|     + + B o.B=++|
|    . . S . *.o +|
|       + = o o . |
|        o . .    |
|                 |
|                 |
+----[SHA256]-----+
qingran@qingrandeMacBook-Pro .ssh % ls -l
total 16
-rw-------  1 qingran  staff  3401  5 30 10:51 id_rsa
-rw-r--r--  1 qingran  staff   760  5 30 10:51 id_rsa.pub
qingran@qingrandeMacBook-Pro .ssh % ls -ltr
total 16
-rw-------  1 qingran  staff  3401  5 30 10:51 id_rsa
-rw-r--r--  1 qingran  staff   760  5 30 10:51 id_rsa.pub

//id_rsa 私钥文件 id_rsa.pub 公钥文件，copy公钥到GitHub的SSH配置项里面

qingran@qingrandeMacBook-Pro .ssh % vi id_rsa.pub 
qingran@qingrandeMacBook-Pro .ssh % cd Desktop/Git
cd: no such file or directory: Desktop/Git
qingran@qingrandeMacBook-Pro .ssh % cd ..
qingran@qingrandeMacBook-Pro ~ % cd Desktop/Git
qingran@qingrandeMacBook-Pro Git % git clone git@github.com:qrgwh123/MyFirstRepo.git
Cloning into 'MyFirstRepo'...
The authenticity of host 'github.com (20.205.243.166)' can't be established.
ED25519 key fingerprint is SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? 
Host key verification failed.
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
qingran@qingrandeMacBook-Pro Git % ls
GitOperate.md	LearnGit	assets
qingran@qingrandeMacBook-Pro Git % git clone git@github.com:qrgwh123/MyFirstRepo.git
Cloning into 'MyFirstRepo'...
The authenticity of host 'github.com (20.205.243.166)' can't be established.
ED25519 key fingerprint is SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes

//记得选yes

Warning: Permanently added 'github.com' (ED25519) to the list of known hosts.
remote: Enumerating objects: 4, done.
remote: Counting objects: 100% (4/4), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 4 (delta 0), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (4/4), 12.74 KiB | 86.00 KiB/s, done.
qingran@qingrandeMacBook-Pro Git % ls
GitOperate.md	LearnGit	MyFirstRepo	assets
qingran@qingrandeMacBook-Pro Git % cd MyFirstRepo
qingran@qingrandeMacBook-Pro MyFirstRepo % ls
LICENSE		README.md
qingran@qingrandeMacBook-Pro MyFirstRepo % cat README.md
# MyFirstRepo
Learn Git
qingran@qingrandeMacBook-Pro MyFirstRepo % echo hello > hello.txt
qingran@qingrandeMacBook-Pro MyFirstRepo % git status
On branch main
Your branch is up to date with 'origin/main'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	hello.txt

nothing added to commit but untracked files present (use "git add" to track)
qingran@qingrandeMacBook-Pro MyFirstRepo % git add .
qingran@qingrandeMacBook-Pro MyFirstRepo % git status
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	new file:   hello.txt

qingran@qingrandeMacBook-Pro MyFirstRepo % git commit -m "2024.5.30,first commit"
[main d6c37ca] 2024.5.30,first commit
 1 file changed, 1 insertion(+)
 create mode 100644 hello.txt
qingran@qingrandeMacBook-Pro MyFirstRepo % git ls-files
LICENSE
README.md
hello.txt
qingran@qingrandeMacBook-Pro MyFirstRepo % git push

//push 到远程仓库，在GitHub上才可以看到哈

Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 8 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 313 bytes | 313.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To github.com:qrgwh123/MyFirstRepo.git
   5f561ab..d6c37ca  main -> main
qingran@qingrandeMacBook-Pro MyFirstRepo % 
```

**提交自己的文件到远程仓库**
![截屏2024-05-30 11.27.02](assets/%E6%88%AA%E5%B1%8F2024-05-30%2011.27.02.png)
```
qingran@qingrandeMacBook-Pro ~ % cd Desktop/Git
qingran@qingrandeMacBook-Pro Git % ls
GitOperate.md	LearnGit	MyFirstRepo	assets
qingran@qingrandeMacBook-Pro Git % cd MyFirstRepo
qingran@qingrandeMacBook-Pro MyFirstRepo % ls
GitOperate.md	LICENSE		README.md	hello.txt
qingran@qingrandeMacBook-Pro MyFirstRepo % git status
On branch main
Your branch is up to date with 'origin/main'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	GitOperate.md

nothing added to commit but untracked files present (use "git add" to track)
qingran@qingrandeMacBook-Pro MyFirstRepo % git add .
qingran@qingrandeMacBook-Pro MyFirstRepo % git ls-files
GitOperate.md
LICENSE
README.md
hello.txt
qingran@qingrandeMacBook-Pro MyFirstRepo % git status
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	new file:   GitOperate.md

qingran@qingrandeMacBook-Pro MyFirstRepo % git commit -m "2024.5.30 first commit GitOperate"   
[main 81768de] 2024.5.30 first commit GitOperate
 1 file changed, 467 insertions(+)
 create mode 100644 GitOperate.md
qingran@qingrandeMacBook-Pro MyFirstRepo % git push
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 8 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 5.35 KiB | 5.35 MiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To github.com:qrgwh123/MyFirstRepo.git
   d6c37ca..81768de  main -> main
qingran@qingrandeMacBook-Pro MyFirstRepo % 
```
**/User/qingran/Desktop/Git/MyFirstRepo/GitOperate.md已经被push到GitHub上，而且也被Git管理**

##### 2024.6.2
**将本地仓库关联到远程仓库**
![截屏2024-06-02 13.11.58](assets/%E6%88%AA%E5%B1%8F2024-06-02%2013.11.58.png)
**1**先创建一个远程仓库
`创建的时候，什么都不要添加，只写一个仓库名字即可，否则后面在执行git push -u origin main会有error`
![截屏2024-06-02 14.00.40](assets/%E6%88%AA%E5%B1%8F2024-06-02%2014.00.40.png)
```
在已经创建被Git管理的目录下依次次执行
git remote add origin git@github.com:qrgwh123/Test.git
git branch -M main
git push -u origin main
将可以了
```
**我这里下面不一样是因为已经被覆盖了，在创建了一个Test做演示**
**2**回到命令行进入已经创建好的Git仓库
```
Last login: Sun Jun  2 13:25:09 on ttys000
qingran@qingrandeMacBook-Pro ~ % cd Desktop/Git/LearnGit
qingran@qingrandeMacBook-Pro LearnGit % ls
file3.txt	file4.txt	file5.txt	gitle.md	hello.log
qingran@qingrandeMacBook-Pro LearnGit % git remote add origin git@github.com:qrgwh123/SecondRepoPro.git
error: remote origin already exists.
qingran@qingrandeMacBook-Pro LearnGit % git remote -v

//git remote -v 查看当前仓库所对应的远程仓库的别名的地址

origin	git@github.com:qrgwh123/SecondRepo.git (fetch)
origin	git@github.com:qrgwh123/SecondRepo.git (push)
qingran@qingrandeMacBook-Pro LearnGit % git remote remove origin

//git remote remove origin 移除别名为origin的远程仓库

qingran@qingrandeMacBook-Pro LearnGit % git remote -v
qingran@qingrandeMacBook-Pro LearnGit % git remote add origin git@github.com:qrgwh123/SecondRepoPro.git
qingran@qingrandeMacBook-Pro LearnGit % git branch -M main
qingran@qingrandeMacBook-Pro LearnGit % git push -u origin main

//git branch -M main 指定分支名称
//git push -u origin main 帮我们的本地的mian分支和远程的origin仓库的main分支关联起来
  全称 git push -upstream origin main:mian
//我不需要输入密码，如果需要需要输入密码，输入GitHub的密码即可

Enumerating objects: 37, done.
Counting objects: 100% (37/37), done.
Delta compression using up to 8 threads
Compressing objects: 100% (26/26), done.
Writing objects: 100% (37/37), 3.23 KiB | 827.00 KiB/s, done.
Total 37 (delta 9), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (9/9), done.
To github.com:qrgwh123/SecondRepoPro.git
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.
qingran@qingrandeMacBook-Pro LearnGit % 
```
**关联成功**
![截屏2024-06-02 13.55.52](assets/%E6%88%AA%E5%B1%8F2024-06-02%2013.55.52.png)
```
Last login: Sun Jun  2 13:48:00 on ttys000
qingran@qingrandeMacBook-Pro ~ % cd Desktop/Git/LearnGit
qingran@qingrandeMacBook-Pro LearnGit % ls
file3.txt	file4.txt	file5.txt	gitle.md	hello.log
qingran@qingrandeMacBook-Pro LearnGit % git pull origin main

//执行完 git pull 后Git会自动帮我们执行一次合并操作，如果远程仓库中的修改内容和本地仓库中的修改内容没有冲突的话，合并成功，否则合并失败，这个时候需要手动来解决冲突，还可以使用fetch(只会获取远程仓库修改，但不会主动合并到本地仓库中，需要手动合并)

remote: Enumerating objects: 4, done.
remote: Counting objects: 100% (4/4), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), 932 bytes | 310.00 KiB/s, done.
From github.com:qrgwh123/SecondRepoPro
 * branch            main       -> FETCH_HEAD
   cc75f0c..7f83fce  main       -> origin/main
Updating cc75f0c..7f83fce
Fast-forward
 README.md | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 README.md
qingran@qingrandeMacBook-Pro LearnGit % ls
README.md	file4.txt	gitle.md
file3.txt	file5.txt	hello.log
qingran@qingrandeMacBook-Pro LearnGit % cat README.ma
cat: README.ma: No such file or directory
qingran@qingrandeMacBook-Pro LearnGit % cat README.md
添加一个README
qingran@qingrandeMacBook-Pro LearnGit % 
```
