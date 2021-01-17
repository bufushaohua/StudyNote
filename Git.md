## Git

### Git工作区域

- 工作区：添加、编辑、修改文件等动作
- 暂存区：暂存已经修改的文件最后统一提交到Git仓库
- Git Repository ：最终确定的文件保存到仓库，成为一个新的版本，并且对他人可见

![image-20200517094416044](C:\Users\CYC\AppData\Roaming\Typora\typora-user-images\image-20200517094416044.png)

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

  