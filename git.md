<center><b>git学习</b>

git安装后需要登陆用户，--global是针对全部的git目录使用该用户，也可以设置某一个git目录使用不同的用户

 ```java
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
 ```

初始化

 ```java
$ git init
 ```

将文件增加到git仓库

```java
 $ git add 文件名
```

将文件提交到仓库

 ```java
$ git commit -m '提交的备注信息'
 ```

查看文件的变更状态

 ```java
$ git status
 ```

查看文件的变更内容是那些

 ```java
$ git diff 文件名
 ```

git提交记录/简化版提交记录

 ```java
$ git log/git log --pretty=oneline
 ```

git回退版本，首先，Git必须知道当前版本是哪个版本，在Git中，用HEAD表示当前版本，上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100

 ```java
$ git reset --hard HEAD^
 ```

查看git每一次命令的执行记录

 ```java
$ git reflog
 ```

丢弃工作区的修改，恢复到暂存区或者分支的内容

 ```java
$ git checkout -- 文件名
 ```

取消暂存区内容，回到工作区

 ```java
$ git reset HEAD 文件名
 ```

删除git分支中的文件，删除后记得commit提交暂存区

 ```java
$ git rm 文件名
 ```

创建github关联密钥
		第1步：创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：

 ```java
$ ssh-keygen -t rsa -C '仓库当时登陆的邮箱'
 ```

​		第2步：登陆GitHub，打开“Account settings”，“SSH Keys”页面
然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容

与github进行关联，因为本地没有建立git服务器，所以就是用github当作服务器，先创建github用户，然后创建一个远程repostory,然后在本地仓库进行如下代码关联

 ```java
$ git remote add 远程库名 git@github.com:github账号/远程库名.git
 ```

与github连接后就可以上传本地文件到远程仓库了,把本地库的内容推送到远程，用git push命令，实际上是把当前分支master推送到远程。
由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。

 ```java
$git push -u 远程库名 master
 ```

下载克隆github上面的项目

 ```java
$git clone github仓地址 
 ```

---



分支：
  创建分支 

```java
$ git branch 分支名
```

  切换分支 

```java
$ git checkout 分支名
```

  创建并且切换分支 

```java
$ git checkout -b 分支名
```

  指定分支与当前分支合并 

```java
$ git merge 分支名
```

  删除分支 

```java
$ git branch -d 分支名//小写的d是不做强制，如果为合并会删除失败。   大写的D做强制删除，不管三七二十八
```

  新版的git中:switch代替了checkout来切换分支，因为git checkout 分支名称和git checkout -- 文件名两个命令重叠的感觉。
  创建并切换:

```java
$ git switch -c 分支名
```

  切换分支：

```java
$ git switch 分支名
```

  使用新的git switch命令，比git checkout要更容易理解。



假设 masrer的test.txt文件的第一行是"哈哈哈哈"，切换分支dev修改文件test.txt第一行同样增加类似"哈哈哈哈"的字样，然后在切换master进行合并是会提示冲突，并要求我们手动整合在进行提交。

下列代码可以查看当前分支的情况（虽然我也不太理解）

```java
$ git log --graph --pretty=oneline --abbrev-commit
```

---



临时保存当前代码

```java
$ git stash
```

查看当前临时保存列表

```java
$ git stash list 
```

当前工作目录恢复到制定工作列表，pop在恢复后回删除stash列表，apply恢复不删除

```java
//最新的在0
$ git stash pop stash@{下标}
$ git stash apply stash@{下标}
```

删除临时保存的工作列表

```java
$ git stash clear//全部删除
$ git stash drop stash@{下标}
```

复制一个制定提交到当前的分支

```java
$ git cherry-pick id号
```



多人协作

```java
$ git remote //查看远程库的信息
    
$ git remote -v //查看远程库的详细信息  fetch=抓取  push=推送
    
$ git push 远程库名 本地分支名 //推送分支
    
$ git checkout -b dev origin/dev //将远程分支dev创建到本地  按道理来说origin应该是master的，总结来讲，顾名思义，origin就是一个名字，它是在你clone一个托管在Github上代码库时，git为你默认创建的指向这个远程代码库的标签， origin指向的是repository，master只是这个repository中默认创建的第一个branch。当你git push的时候因为origin和master都是默认创建的。
    
$ git branch --set-upstream-to=远程仓库名/远程分支 本地分支//如果git pull提示no tracking information，则说明本地分支和远程分支的链接关系没有创建
    
$ git pull //更新分支内容并合并  如果出现冲突 清手动解决冲突问题
```



git打标签

```java
$ git tag 标签 //给当前分支打上标签 也可以增加上commitID来指定某一个提交
$ git log --pretty=oneline --abbrev-commit //查看标签内容
$ git tag -a 标签 -m 标签解释 commitID // 给某一个提交增加上标签和标签的详细解释
$ git show 标签 //可以查看该标签的详细解释内容

$ git push 远程仓库名 标签 //将标签推送到远程仓库
$ git push 远程仓库名 --tags //将所有的标签推送到远程仓库
$ git tag -d 标签 //删除本地标签
$ git push 远程仓库名 :refs/tags/标签 //删除远程仓库标签

```



因为github是国外的服务器所以速度方面比较慢，国内托管是gitee这个和github差不多一致，同样注册用户上传SSHKEY然后使用即行了

---



如果git中有不想提交的文件但是每次git status总是提示，强迫症的同学可以在git本地仓库的根目录创建一个.gitignore文件并在里面限制那些文件进行验证

如果是在向提交某一个限制文件可以使用-f进行强制提交 $ git add -f 文件名

如果发现某一个文件并没有添加在限制文件中  但是也不提示提交 可以使用 $ git check-ignore -v 文件名  看看限制文件是不是某一个规则把他限制了



```java
$ git config --global alias.st status //该命令是将 status这个单词使用st进行代替  平时输入 git status  现在只需要  git st
```

### 配置文件

配置Git的时候，加上`--global`是针对当前用户起作用的，如果不加，那只针对当前的仓库起作用。

配置文件放哪了？每个仓库的Git配置文件都放在`.git/config`文件中：

别名就在`[alias]`后面，要删除别名，直接把对应的行删掉即可。

而当前用户的Git配置文件放在用户主目录下的一个隐藏文件`.gitconfig`中：





git@github.com:adminyusonghao/gitdemo.git

```
git@127.0.0.1:/home/git/gitrepository/gitpository.git
```

### 搭建本地仓库

```java
//安装git
$ yum install git
//创建一个本地用户
$ adduser git
//切换用户
$ su - git
//创建一个公匙管理文件 ,并将客户端的公匙添加进来 ，每行一个
$ vi /home/git/.ssh/authorized_keys
//创建本地仓库目录
$ git init --bare sample.git
//给权限
$ chown -R git:git sample.git

git@127.0.0.1:/home/git/nodework.git
```

