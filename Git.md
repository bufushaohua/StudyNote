# Git

## Git命令整理

![img](D:\Files\Note\Typora\PNG\Git.assets\20150526165114055.jfif)

### **1) 远程仓库相关命令**

检出仓库：$ git clone git://github.com/jquery/jquery.git

查看远程仓库：$ git remote -v

添加远程仓库：$ git remote add [name] [url]

删除远程仓库：$ git remote rm [name]

修改远程仓库：$ git remote set-url --push [name] [newUrl]

拉取远程仓库：$ git pull [remoteName] [localBranchName]

推送远程仓库：$ git push [remoteName] [localBranchName]

 

*如果想把本地的某个分支test提交到远程仓库，并作为远程仓库的master分支，或者作为另外一个名叫test的分支，如下：

$git push origin test:master     // 提交本地test分支作为远程的master分支

$git push origin test:test        // 提交本地test分支作为远程的test分支

 

### **2）分支(branch)操作相关命令**

查看本地分支：$ git branch

查看远程分支：$ git branch -r

创建本地分支：$ git branch [name] ----注意新分支创建后不会自动切换为当前分支

切换分支：$ git checkout [name]

创建新分支并立即切换到新分支：$ git checkout -b [name]

删除分支：$ git branch -d [name] ---- -d选项只能删除已经参与了合并的分支，对于未有合并的分支是无法删除的。如果想强制删除一个分支，可以使用-D选项

合并分支：$ git merge [name] ----将名称为[name]的分支与当前分支合并

创建远程分支(本地分支push到远程)：$ git push origin [name]

删除远程分支：$ git push origin :heads/[name] 或 $ gitpush origin :[name] 

 

*创建空的分支：(执行命令之前记得先提交你当前分支的修改，否则会被强制删干净没得后悔)

$git symbolic-ref HEAD refs/heads/[name]

$rm .git/index

$git clean -fdx

 

### **3）版本(tag)操作相关命令**

查看版本：$ git tag

创建版本：$ git tag [name]

删除版本：$ git tag -d [name]

查看远程版本：$ git tag -r

创建远程版本(本地版本push到远程)：$ git push origin [name]

删除远程版本：$ git push origin :refs/tags/[name]

合并远程仓库的tag到本地：$ git pull origin --tags

上传本地tag到远程仓库：$ git push origin --tags

创建带注释的tag：$ git tag -a [name] -m 'yourMessage'

 

### **4) 子模块(submodule)相关操作命令**

添加子模块：$ git submodule add [url] [path]

  如：$git submodule add git://github.com/soberh/ui-libs.git src/main/webapp/ui-libs

初始化子模块：$ git submodule init  ----只在首次检出仓库时运行一次就行

更新子模块：$ git submodule update ----每次更新或切换分支后都需要运行一下

删除子模块：（分4步走哦）

 1) $ git rm --cached [path]

 2) 编辑“.gitmodules”文件，将子模块的相关配置节点删除掉

 3) 编辑“ .git/config”文件，将子模块的相关配置节点删除掉

 4) 手动删除子模块残留的目录

 

### **5）忽略一些文件、文件夹不提交**

在仓库根目录下创建名称为“.gitignore”的文件，写入不需要的文件夹名或文件，每个元素占一行即可，如

target

bin

*.db

## Git工作区域

- Workspace：工作区
- Index / Stage：暂存区
- Repository：仓库区（或本地仓库）
- Remote：远程仓库

![img](D:\Files\Note\Typora\PNG\Git.assets\1453965-20180907151948945-2047488841.png)

- 工作区：添加、编辑、修改文件等动作
- 暂存区：暂存已经修改的文件最后统一提交到Git仓库
- Git Repository ：最终确定的文件保存到仓库，成为一个新的版本，并且对他人可见

![image-20200517094416044](D:\Files\Note\Typora\PNG\Git.assets\image-20200517094416044.png)

#### 向仓库添加文件流程

- git status

- git add hello.java

- git commit  -m "提交描述"

  #### Git初始化及仓库创建和操作

  ##### 基本信息设置

  ~~~bash
  1.设置用户名
  git config --global user.name 'Github上的用户名'
  
  2.设置用户名邮箱
  git config --global user.email '邮箱地址'
  
  3.查看设置
  git config --list
  ~~~

  脚下留心：该设置在GitHub仓库主页显示谁提交了文件

  > 初始化一个新的Git仓库
  >
  > 1.创建文件夹
  >
  > mkdir a.java
  >
  > 2.在文件内初始化git（创建git仓库）
  >
  > pwd 只显示当前位置
  >
  > git init :在当前文件夹下生成隐藏文件.git​

  ##### 添加文件

  - 创建文件： touch test1.php
  - 修改文件：vim a.java    
  - 保存修改文件 ：  先esc切换模式，在：wq + 回车就ok了 
  - 添加到暂存区： git add test1.php
  - 提交到仓库：git commit -m "提交描述" 

  ##### 删除文件

  - 删除文件： rm test.php

  - 从Git中删除文件：git rm test.php
  - 提交操作：git commit -m "提交描述"


## 常见问题

### 1、使用git提交到github,每次都要输入用户名和密码

==原因==：在clone 项目的时候，使用了 https方式，而不是ssh方式。默认clone 方式是：https

**解决方法：**

1. 查看clone地址：git remote -v

![image-20210117210534757](D:\Files\Note\Typora\PNG\Git.assets\image-20210117210534757.png)

地址以https开头 说明是https方式，现在换成ssh方式。

2. 移除https的方式，换成 ssh方式

​        git remote rm origin 

3. 添加新的git方式：ssh方式，ssh方式地址的话，在github上，切换到ssh方式，然后复制地址。

4. 查看push方式是否修改成功：

   git remote -v

   看到如下，说明成功，地址是以git开头

![image-20210117210904214](D:\Files\Note\Typora\PNG\Git.assets\image-20210117210904214.png)

5. 重新push（提交一下）

   git push origin master![image-20210117211101830](D:\Files\Note\Typora\PNG\Git.assets\image-20210117211101830.png)