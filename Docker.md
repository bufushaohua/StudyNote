> ​	本文档是根据B站千锋Java学习营的《2020Docker最新超详细版教程通俗易懂》所整理的
>
> ​																																				                                             —— CYC

# Docker

> ### 什么是Docker？？
>
> ​		Docker 是一个开源的应用容器引擎，让开发者可以打包他们的应用以及依赖包到一个可移植的容器中,然后发布到任何流行的[Linux](https://baike.baidu.com/item/Linux)机器或Windows 机器上,也可以实现虚拟化,容器是完全使用沙箱机制,相互之间不会有任何接口。
>
> 一个完整的Docker有以下几个部分组成：
>
> 1. DockerClient客户端
> 2. Docker Daemon守护进程
> 3. Docker Image镜像
> 4. DockerContainer容器

## 一、Docker基本操作

### 1.1安装Docker

~~~sh
# 1.需要Liunx版本为CentOS7.0以上
~~~

~~~sh
# 2.卸载旧版本（非必须项）
yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
~~~

![image-20200906110736323](C:\Users\CYC\AppData\Roaming\Typora\typora-user-images\image-20200906110736323.png)

~~~sh
# 3. 下载关于Docker的依赖环境
yum install -y yum-utils \ device-mapper-persistent-data \ lvm2 
~~~

![image-20200906110826521](C:\Users\CYC\AppData\Roaming\Typora\typora-user-images\image-20200906110826521.png)

~~~sh
# 4.设置镜像仓库(阿里云的镜像仓库)
yum-config-manager  --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
~~~

![image-20200906110903567](C:\Users\CYC\AppData\Roaming\Typora\typora-user-images\image-20200906110903567.png)

~~~sh
# 5.安装Docker
yum makecache fast  #  这个命令是将软件包信息提前在本地缓存一份，用来提高搜索安装软件的速度
yum -y install docker-ce
~~~

~~~sh
# 6.启动，并设置为开机自动启动，测试
# 启动Docker服务
systemctl start docker
# 设置开机自动启动
systemctl enable docker
# 测试
docker run hello-world
~~~

## 二、docker入门

### 2.2 Docker的中央仓库

> 1. Docker官方的中央残酷：这个仓库是镜像最全的，但是下载速度较慢
>
>    https://hub.docker.com/
>
> 2. 国内的镜像网站：网易蜂巢、daocloud
>
>    http://c.163.com/hub#/home
>
>    http://hub.daocloud.io/
>
> 3. 在公司内部会采用私服的方式拉取镜像（添加配置）

~~~json
# 需要在 /etc/docker/daemon.json
{
	"registry-mirrors":["https://registry.docker-cn.com"],
	"insecure-registries":["ip:port"]
}
# 重启两个服务
systemctl daemon-reload
systemctl restart docker
~~~

### 2.3 镜像的操作

#### 1. 拉取镜像到本地

~~~sh
# 镜像一般分为两种，一种基础镜像，另一种是用户镜像,例如cyc595394100/centos，这种就是带有用户名称为前缀的用户镜像

# 1. 拉取镜像到本地
docker pull 镜像名称[:tag]
# 举个例子
docker pull daocloud.io/library/tomcat:8.5.15-jre8	
~~~

#### 2.查看全部本地镜像

~~~sh
# 2. 查看全部本地镜像
docker images
~~~

#### 3. 删除本地镜像

~~~sh
# 3. 删除本地镜像
docker rmi 镜像的标识


[root@cyc ~]# docker rmi b8
Untagged: daocloud.io/library/tomcat:8.5.15-jre8
Untagged: daocloud.io/library/tomcat@sha256:0400f5d82666cf5aef26a4beac7640f08f9c468e5ef32afb5f6aff46ea6f8561
Deleted: sha256:b8dfe9ade31661d865d11388245a3382365b197786ad6c0077f3567ac03bb570
Deleted: sha256:a31c4bd773fa4d1c1c56eb44d200d3be4bb4a0d31084fb7402db0b7e7e4d740f
Deleted: sha256:646254632b3dc6a3eca043c7bf952d252eec37508f42e115795b25b4bc420e24
Deleted: sha256:078c54c5eadc786663009fe4b1b30c793ef31be09a1218372c94f33893d68be0
Deleted: sha256:236865f1b823601f41a1ee16614aff5934c420f935a2f81815a61cd0a8d03fe0
Deleted: sha256:4877af8b2dbb41cf6ddbe506b3498a86197a0f24b24896d89af38db23777cf09
Deleted: sha256:932865e35aec27c1337325abf51889e82e34d1bad62fec4a010ce3d606ae0ea3
Deleted: sha256:78eaa1bf5ccdab00cce3ef3b34e3fb565d15329723e5b7f93bf60265826c55f8
Deleted: sha256:78367d8bfbdd186003a629384189a2203fd63e22a6f4bc5418a63a9f2581666a
Deleted: sha256:efbd63cd7dbd42723c240102d4228236da91690ba3b297df889ea218d23edebc
Deleted: sha256:fa186606764c107c5db39cc31d7faa724f743576e898c17b9c6147e450d9c370
Deleted: sha256:c8c3ca96eb5fb848ef062913ece0284563dac491320aab7c3e46cebba11acb39
Deleted: sha256:e4a93bc5ab2fe334eb93d265acce6aa0f190b71e9a08b75e6fe0fc4c8ed5cc7c
Deleted: sha256:0f7e33898490bdfec7fabce3b451f60361c7b5bc83c53ef1ab27a8b41969ff4c
Deleted: sha256:0d960f1d4fba2435d731fee5d1e68e25058bc1b22921bb95dbab98149548ce9e

~~~

#### 4.镜像的导入导出（不规范）

~~~sh
#4.镜像的导入导出（不规范）
# 将本地的镜像导出（镜像ID不需要写全，只需要能唯一标识）
docker save -o 导出的路径 镜像Id

[root@cyc ~]# docker save -o ./tomcat.image b8
[root@cyc ~]# ls
anaconda-ks.cfg  initial-setup-ks.cfg  tomcat.image

# 加载本地的镜像文件
docker load -i 镜像文件

[root@cyc ~]# docker load -i tomcat.image 
0d960f1d4fba: Loading layer [==================================================>]  129.3MB/129.3MB
71ce2dc7f761: Loading layer [==================================================>]  45.45MB/45.45MB
d1de89c613d4: Loading layer [==================================================>]  1.226MB/1.226MB
32ceb20ee08e: Loading layer [==================================================>]  3.584kB/3.584kB
d69122875478: Loading layer [==================================================>]  3.584kB/3.584kB
1ae7b18f5354: Loading layer [==================================================>]  1.536kB/1.536kB
0583af55a445: Loading layer [==================================================>]  141.6MB/141.6MB
80bee1a65ead: Loading layer [==================================================>]  425.5kB/425.5kB
156545607418: Loading layer [==================================================>]   2.56kB/2.56kB
a56217ad4142: Loading layer [==================================================>]   5.12kB/5.12kB
362ed0033c5c: Loading layer [==================================================>]   7.57MB/7.57MB
990331df9550: Loading layer [==================================================>]  134.1kB/134.1kB
1334a29f0bed: Loading layer [==================================================>]  16.84MB/16.84MB
10ec38b51da1: Loading layer [==================================================>]  2.048kB/2.048kB
Loaded image ID: sha256:b8dfe9ade31661d865d11388245a3382365b197786ad6c0077f3567ac03bb570

# 修改镜像名称
docker tag 镜像id 新镜像名称：版本

[root@cyc ~]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
<none>              <none>              b8dfe9ade316        3 years ago         334MB
[root@cyc ~]# docker tag b8 tomcat:8.5.15
[root@cyc ~]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
tomcat              8.5.15              b8dfe9ade316        3 years ago         334MB

~~~

### 2.4 容器的操作

#### 1. 运行容器

~~~sh
# 1. 运行容器
# 1.1 启动容器
docker start 容器id

#1.2 新建并启动容器
# 简单操作
docker run 镜像的标识|镜像的名称[:tag]
# 常用的参数
docker run -d -p 宿主机端口:容器端口 --name 容器名称 镜像的标识|镜像名称[:tag]
# -d:代表后台运行容器
# -p 宿主机端口:容器端口：为了映射当前Linux的端口和容器的端口
# --name 容器名称：指定容器的名称

[root@cyc ~]# docker run -d -p 8080:8080 --name tomcat b8
b4a3b0450245cffe16fc98c80a93e4898a50095b798eedf680a603fcbe28ee6d

在浏览器中输入：http://192.168.65.128:8080/  即可出现tomcat启动页
~~~

#### 2.查看正在运行的容器

~~~sh
# 2.查看正在运行的容器
docker ps
# 查看容器
docker ps [-qa]
# -a:查看全部的容器，包括没有运行
# -q：只查看容器得到标识

[root@cyc ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                    NAMES
b4a3b0450245        b8                  "catalina.sh run"   2 minutes ago       Up 2 minutes        0.0.0.0:8080->8080/tcp   tomcat
[root@cyc ~]# docker ps -q
b4a3b0450245
[root@cyc ~]# docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                    NAMES
b4a3b0450245        b8                  "catalina.sh run"   7 minutes ago       Up 7 minutes        0.0.0.0:8080->8080/tcp   tomcat
[root@cyc ~]# docker ps -qa
b4a3b0450245
~~~

#### 3. 查看容器的日志

~~~sh
# 3. 查看容器的日志
docker logs -f 容器id
# -f：可以滚动查看日志的最后几行
 
[root@cyc ~]# docker logs -f b8
Error: No such container: b8   # container是上面的容器id，只需要标识唯一就ok
[root@cyc ~]# docker logs -f b4

~~~

#### 4. 进入容器内部

~~~sh
# 4. 进入容器内部

# 4.1 attach命令
docker attach [--detach-keys[=[]]] [--no-stdin] [--sig-proxy[=true]] 容器id
# --detach-keys[=[]]：指定UI出attach模式的快捷键序列，默认是CTRL-p CTRL-q
# --no-stdin=true|false：是否关闭标准输入，默认是保持打开
# --sig-proxy[=true]：是否代理收到的系统信号给应用进程，默认为true

docker exec -it 容器id bash|/bin/bash   # 注意：一般不进入容器内部操作

# 退出容器内部
exit
~~~

#### 5.删除容器

~~~sh
# 5.删除容器（删除容器前，需要先停止容器 ）
docker stop 容器id      # 停止指定的容器
docker stop $(docker ps -qa )      #停止全部容器
docker rm 容器id         #删除指定容器
docker rm $(docker ps -qa )         #删除全部容器
~~~

#### 6.启动容器

~~~sh
# 6.启动容器
docker start 容器id
~~~

#### 7.停止容器

~~~sh
# 7.1 暂停容器
docker pause 容器id

# 7.2 终止容器  【处于终止状态的容器可以使用start 命令来重新启动】
docker stop 容器id
~~~

#### 8.容器的导入导出

~~~sh
# 导出容器
docker export -o 文件名 容器id

# 导入容器
docker import 文件名 - 容器id
~~~

#### 9.查看容器

~~~sh
# 9.1 查看容器详细信息（会json格式返回包括容器id、创建时间、路径、状态镜像配置等在内的各项信息）
docker container inspect 容器id

# 9.2 查看容器内进程  【打印容器内的进程信息，包括PID、用户、时间、命令】
docker stop 容器id

# 9.3 查看统计信息
docker stats
~~~

#### 10.其他容器命令

~~~sh
# 10.1 复制文件
# 将本地的路径data复制到test容器的/tmp路径下
docker cp data  test:/tmp/

# 10.2 查看变更
# 查看test容器内的数据修改
docker container diff test

# 10.3 查看端口映射
# 查看test端口映射情况
docker container port test
~~~

#### 11.查看容器的挂在目录

~~~sh
docker inspect container_name | grep Mounts -A 20
docker inspect container_id | grep Mounts -A 20
~~~



### 2.4搭建本地私有仓库

#### 1.下载registry镜像创建本地的私有仓库服务

~~~sh
# 自动下载并启动一个registry容器, 创建本地的私有仓库服务，监听端口为5000
docker run -d -p 5000:5000 registry:2

# 默认情况下，仓库会被创建在容器的/var/bin/registry目录下，可以用 -v 将镜像文件存放在本地的指定路径
docker run -d -p 5000:5000 -v /opt/data/registry:/var/lib/registry registry:2
~~~



## 三、Docker应用

### 3.1 准备SSM工程

~~~sh
# mysql数据库的连接用户名和密码改变了，修改db.properties
~~~



### 3.2 准备MySQL容器

~~~sh
# 运行MySQL容器
docker run -d -p 3306:3306 --name mysql -e MYSQL_ROOT_PASSWORD=root daocloud.io/library/mysql:5.7.4


[root@cyc ~]# docker run -d -p 3306:3306 --name mysql -e MYSQL_ROOT_PASSWORD=root daocloud.io/library/mysql:5.7.4
Unable to find image 'daocloud.io/library/mysql:5.7.4' locally
5.7.4: Pulling from library/mysql
a3ed95caeb02: Pull complete
a9816d489674: Pull complete
b1f7ef5cc072: Pull complete
c7110e7a9f56: Pull complete
d78d8ec4f800: Pull complete
52e42de90064: Pull complete
c8a5c40648db: Pull complete
c535209921b3: Pull complete
Digest: sha256:0f3b5658a3fbdbb0283d004b09d326fe827b4139886253a9d51d74e51b9d6808
Status: Downloaded newer image for daocloud.io/library/mysql:5.7.4
944f9dbcbb46e0c25e56060d98522c3c4b496f6a59469870d0e740d3e4cd76dc

~~~



### 3.3 准备Tomcat

~~~sh
# 运行Tomcat容器，前面已经搞定，只需要将SSM项目的war包部署到Tomcat容器内部即可
# 可以通过命令将宿主机的内容复制到容器内部 
docker cp 文件名称 容器id:容器内部路径
# 举个例子
docker cp maven_web.war 8e:/usr/local/tomcat/webapps

[root@cyc ~]# docker images
REPOSITORY                  TAG                 IMAGE ID            CREATED             SIZE
tomcat                      8.5.15              b8dfe9ade316        3 years ago         334MB
daocloud.io/library/mysql   5.7.4               aa5364eb3d85        5 years ago         252MB
[root@cyc ~]# docker run -d -p 8080:8080 --name tomcat b8
8e8fcee5dc159687e07f748f1506b7f4e81388b4ec73e821daebbbb959766a5c
[root@cyc ~]# docker ps
CONTAINER ID        IMAGE                             COMMAND                  CREATED             STATUS              PORTS                    NAMES
8e8fcee5dc15        b8                                "catalina.sh run"        6 seconds ago       Up 5 seconds        0.0.0.0:8080->8080/tcp   tomcat
944f9dbcbb46        daocloud.io/library/mysql:5.7.4   "/entrypoint.sh mysq…"   9 minutes ago       Up 9 minutes        0.0.0.0:3306->3306/tcp   mysql
[root@cyc ~]# docker cp maven_web.war 8e:/usr/local/tomcat/webapps
[root@cyc ~]#
~~~



### 3.4 数据卷

> 为了部署SSM的工程，需要使用到CP的命令将宿主机内的maven_web.war文件复制到容器内部
>
> 数据卷：将宿主机的一个目录映射到容器的一个目录中
>
> 可以在宿主机中操作目录中的内容，那么容器内部映射的文件，也会跟着一起改变

~~~sh
# 1.创建数据卷
docker volume create 数据卷名称
# 创建数据卷之后，默认会存放在一个目录下 /var/lib/docker/volumes/数据卷名称/_data

[root@cyc ~]# docker volume create tomcat
tomcat
[root@cyc ~]# cd /var/l
lib/   local/ lock/  log/
[root@cyc ~]# cd /var/lib/docker/volumes/
[root@cyc volumes]# ls
cfceb459fcdcdf93e020d9a8c8b0a1b3268807c4ead844a9eadd0f369acf37b6  metadata.db  tomcat
[root@cyc volumes]#
[root@cyc volumes]# cd tomcat/
[root@cyc tomcat]# ls
_data
[root@cyc tomcat]# cd _data/
[root@cyc _data]# ls
[root@cyc _data]#
~~~

~~~sh
# 2.查看数据卷的详细信息
docker volume inspect 数据卷名称
~~~

~~~sh
# 3.查看全部数据卷
docker volume ls
~~~

~~~sh
# 4.删除数据卷
docker volume rm 数据卷名称
~~~

~~~sh
# 5.应用数据卷
# 当你映射数据卷时，如果数据卷不存在，Docker会帮你自动创建，会将容器内部自带的文件，存储在默认的存放路径中
docker run -v 数据卷名称:容器内部的路径 镜像ID
# 直接指定一个路径作为数据卷的存放位置，这个路径下是空的  
docker run  -v 路径:容器内部的路径 镜像ID
~~~



## 四、Docker自定义镜像

> 中央仓库上的镜像，也是Docker用户自己传过去的

~~~sh
# 1. 创建一个Dockerfile文件，并且指定自定义镜像信息
# Dockerfile文件中常用的内容
from: 指定当前自定义镜像依赖的环境
copy: 将相对路径下的内容复制到自定义的镜像中
workdir: 声明镜像的默认工作目录
cmd: 需要执行的命令（在workdir下执行的，cmd可以写多的，只以最后一个为准）
# 举个例子
from daocloud.io/library/tomcat:8.5.15-jre8
copy maven_web.war /usr/local/tomcat/webapps
~~~

~~~sh
# 2.将准备好的Dockerfile和相应的文件拖拽到Linux操作系统中，通过Docker的命令制作镜像
docker build -t 镜像名称:[tag] .   # "."作用自动找Dockerfile文件

# 举个例子
[root@cyc ~]# cd ~
[root@cyc ~]# mkdir ssm-tomcat             【创建ssm-tomcat目录】
[root@cyc ~]# ls
anaconda-ks.cfg  initial-setup-ks.cfg  maven_web.war  ssm-tomcat  tomcat.image
[root@cyc ~]# cd ssm-tomcat/                             
[root@cyc ssm-tomcat]# ls
[root@cyc ssm-tomcat]# pwd                 【将Dockerfile和maven_web.war两个文件上传到当前目录下】
/root/ssm-tomcat
[root@cyc ssm-tomcat]# ls               
Dockerfile  maven_web.war
[root@cyc ssm-tomcat]# docker build -t maven_web:1.0.0 . 【制作镜像】
Sending build context to Docker daemon  7.168kB             
Error response from daemon: Dockerfile parse error line 3: WORKDIR requires exactly one argument
[root@cyc ssm-tomcat]# vi Dockerfile      【上步出错是因为Dockerfile文件中WorkDir和cmd没去掉，这里用不上他们俩】
[root@cyc ssm-tomcat]# docker build -t maven_web:1.0.0 .
Sending build context to Docker daemon  7.168kB
Step 1/2 : from daocloud.io/library/tomcat:8.5.15-jre8
8.5.15-jre8: Pulling from library/tomcat
Digest: sha256:0400f5d82666cf5aef26a4beac7640f08f9c468e5ef32afb5f6aff46ea6f8561
Status: Downloaded newer image for daocloud.io/library/tomcat:8.5.15-jre8
 ---> b8dfe9ade316
Step 2/2 : copy maven_web.war /usr/local/tomcat/webapps
 ---> ef1e51f3d487
Successfully built ef1e51f3d487
Successfully tagged maven_web:1.0.0
[root@cyc ssm-tomcat]# docker images              【制作成功】
REPOSITORY                   TAG                 IMAGE ID            CREATED             SIZE
maven_web                    1.0.0               ef1e51f3d487        24 seconds ago      334MB
daocloud.io/library/tomcat   8.5.15-jre8         b8dfe9ade316        3 years ago         334MB
tomcat                       8.5.15              b8dfe9ade316        3 years ago         334MB
daocloud.io/library/mysql    5.7.4               aa5364eb3d85        5 years ago         252MB
[root@cyc ssm-tomcat]# docker ps
CONTAINER ID        IMAGE                             COMMAND                  CREATED             STATUS              PORTS                    NAMES
c10d6d7c73cf        b8                                "catalina.sh run"        2 days ago          Up 2 days           0.0.0.0:8080->8080/tcp   ssm_tomcat
944f9dbcbb46        daocloud.io/library/mysql:5.7.4   "/entrypoint.sh mysq…"   3 days ago          Up 3 days           0.0.0.0:3306->3306/tcp   mysql
[root@cyc ssm-tomcat]# docker run -d -p 8081:8080 --name ssm_tomcat ef    【自制镜像名称不能和已有镜像同名】
docker: Error response from daemon: Conflict. The container name "/ssm_tomcat" is already in use by container "c10d6d7c73cfde4ea08983eb214319ef926f43f42be8939afaa5f71237723d35". You have to remove (or rename) that container to be able to reuse that name.
See 'docker run --help'.
[root@cyc ssm-tomcat]# docker run -d -p 8081:8080 --name custom_ssm_tomcat ef   【ef为IMAGE ID】
c8b4c5987fd7f81314f1a40fa82b776bca2fb9910177702cb39d62fe1f6e7a89
[root@cyc ssm-tomcat]# docker ps
CONTAINER ID        IMAGE                             COMMAND                  CREATED             STATUS              PORTS                    NAMES
c8b4c5987fd7        ef                                "catalina.sh run"        2 minutes ago       Up 2 minutes        0.0.0.0:8081->8080/tcp   custom_ssm_tomcat
c10d6d7c73cf        b8                                "catalina.sh run"        2 days ago          Up 2 days           0.0.0.0:8080->8080/tcp   ssm_tomcat
944f9dbcbb46        daocloud.io/library/mysql:5.7.4   "/entrypoint.sh mysq…"   3 days ago          Up 3 days           0.0.0.0:3306->3306/tcp   mysql
[root@cyc ssm-tomcat]#

~~~



## 五、Docker—Compose

> 之前运行一个镜像，需要添加大量的参数
>
> 可以通过Docker—Compose编写这些参数
>
> Docker—Compose可以帮助我们批量管理容器
>
> 只需要通过一个docker-compose.yml去维护即可

### 5.1 下载Docker—Compose

~~~sh
# 1.去Github官网搜索docker-compose，下载1.24.1版本的Docker-Compose
链接地址：https://github.com/docker/compose/releases/download/1.24.1/docker-compose-Linux-x86_64

# 2.将下载好的文件，拖拽到Linux操作系统中

# 3.需要将DockerCompose文件的名称修改一下，基于DockerCompose文件一个可执行的权限
[root@cyc ~]# ls
anaconda-ks.cfg              initial-setup-ks.cfg  ssm-tomcat
docker-compose-Linux-x86_64  maven_web.war         tomcat.image
[root@cyc ~]# mv docker-compose-Linux-x86_64  /usr/local     【将安装文件移动到/usr/local/目录下】
[root@cyc ~]# cd /usr/local/
[root@cyc local]# ls
bin  docker-compose-Linux-x86_64  etc  games  include  lib  lib64  libexec  sbin  share  src
[root@cyc local]# mv docker-compose-Linux-x86_64  docker-compose      【修改文件名为docker-image】
[root@cyc local]# ls
bin  docker-compose  etc  games  include  lib  lib64  libexec  sbin  share  src
[root@cyc local]# ll
总用量 15792
drwxr-xr-x. 2 root root        6 4月  11 2018 bin
-rw-r--r--. 1 root root 16168192 9月  13 11:10 docker-compose
drwxr-xr-x. 2 root root        6 4月  11 2018 etc
drwxr-xr-x. 2 root root        6 4月  11 2018 games
drwxr-xr-x. 2 root root        6 4月  11 2018 include
drwxr-xr-x. 2 root root        6 4月  11 2018 lib
drwxr-xr-x. 2 root root        6 4月  11 2018 lib64
drwxr-xr-x. 2 root root        6 4月  11 2018 libexec
drwxr-xr-x. 2 root root        6 4月  11 2018 sbin
drwxr-xr-x. 5 root root       49 4月   5 17:11 share
drwxr-xr-x. 2 root root        6 4月  11 2018 src
[root@cyc local]# chmod 777 docker-compose     				【修改为可执行权限】

# 4.方便后期操作，配置一个环境变量
# 将docker-compose文件移动到了/usr/local/bin，修改了/etc/profile文件，给/usr/local/bin配置到了PATH中
 mv docker-compose  /usr/local/bin/ 
 vi /etc/profile 
 	# docker_compose_path = `/usr/local/bin`
    PATH=/usr/local/bin:$PATH
    export PATH USER LOGNAME MAIL HOSTNAME HISTSIZE HISTCONTROL

 source /etc/profile

#5. 测试一下
  在任意目录下输入docker-compose

# 举个例子
[root@cyc local]# mv docker-compose  /usr/local/bin/    【将docker-compose文件移动到了/usr/local/bin】
[root@cyc local]# cd bin/
[root@cyc bin]# ls
docker-compose
[root@cyc bin]# vi /etc/profile               【修改了/etc/profile文件，给/usr/local/bin配置到了PATH中】    
[root@cyc bin]# source /etc/profile           【使路径生效】
[root@cyc bin]# cd ~
[root@cyc ~]# docker-compose				  【测试环境变量是否安装好，出现以下提示说明已装好】
Define and run multi-container applications with Docker.

Usage:
  docker-compose [-f <arg>...] [options] [COMMAND] [ARGS...]
  docker-compose -h|--help

Options:
  -f, --file FILE             Specify an alternate compose file
                              (default: docker-compose.yml)
  -p, --project-name NAME     Specify an alternate project name
                              (default: directory name)

~~~

### 5.2 Docker-Compose管理MySQL和Tomcat容器

> yml文件以key:value方式来指定配置信息
>
> 多个配置信息以换行+缩进的方式来区分
>
> 在docker-compose.yml文件中，不要使用制表符即TAB键，因为docker-compose不识别

~~~yml
version: '3.1'   # 因为docker-compose版本时1.24.1
services:
  mysql:                 # 服务的名称
    restart: always      # 代表只要docker启动，那么这个容器就跟着一起启动
    image: daocloud.io/library/mysql:5.7.4     # 指定镜像路径
    container_name: mysql     # 指定容器名称
    ports:
      3306:3306               # 指定端口号的映射
    environment:
      MYSQL_ROOT_PASSWORD: root       # 指定MYSQL的ROOT用户登录密码
      TimeZone: Asia/Shanghai         # 指定时区  （或者直接TZ）
    volumes:
      - /opt/docker_mysql_tomcat/mysql_data:/var/lib/mysql    # 映射数据卷
  tomcat:
    restart: always
    image: daocloud.io/library/tomcat:8.5.15-jre8
    container_name: tomcat
    ports:
      8080:8080
    environment:
      TimeZone: Asia/Shanghai         # 指定时区  （或者直接TZ）
    volumes:
      - /opt/docker_mysql_tomcat/tomcat_webapps:/usr/local/tomcat/webapps
      - /opt/docker_mysql_tomcat/tomcat_logs:/usr/local/tomcat/logs
  
~~~

~~~sh
# 举例
[root@cyc ~]# cd /opt/
[root@cyc opt]# ls
containerd  maven_web_tomcat  rh
[root@cyc opt]# mkdir docker_mysql_tomcat
[root@cyc opt]# ls
containerd  docker_mysql_tomcat  maven_web_tomcat  rh
[root@cyc opt]# cd docker_mysql_tomcat/
[root@cyc docker_mysql_tomcat]# vi docker-compose.yml

~~~



### 5.3 使用docker-compose命令管理容器

> 在使用docker-compose的命令时，默认会在当前目录下找docker-compose.yml文件

~~~sh
# 1.基于docker-compose.yml启动管理的容器
  docker-compose up -d

# 2.关闭并删除容器
  docker-compose down

# 3.开启|关闭|重启已经存在的由docker-compose维护的容器
  docker-compose start|stop|restart
  
# 4.查看由docker-compose管理的容器
  docker-compose ps

# 5.查看日志
# 一般不这样查看日志  直接去/opt/docker_mysql_tomcat/tomcat_logs  这里可以查看每天的日志
  docker-compose logs -f  
  
  
 # 实例
 [root@cyc docker_mysql_tomcat]# docker ps
CONTAINER ID        IMAGE                                    COMMAND                  CREATED             STATUS              PORTS                    NAMES
a7052ac8c32b        daocloud.io/library/mysql:5.7.4          "/entrypoint.sh mysq…"   6 seconds ago       Up 4 seconds        0.0.0.0:3306->3306/tcp   mysql
f228c33c0238        daocloud.io/library/tomcat:8.5.15-jre8   "catalina.sh run"        4 hours ago         Up 4 hours          0.0.0.0:8080->8080/tcp   tomcat
[root@cyc docker_mysql_tomcat]# docker-compose down
Stopping mysql  ... done
Stopping tomcat ... done
Removing mysql  ... done
Removing tomcat ... done
Removing network docker_mysql_tomcat_default
[root@cyc docker_mysql_tomcat]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
[root@cyc docker_mysql_tomcat]# docker-compose up -d
Creating network "docker_mysql_tomcat_default" with the default driver
Creating mysql  ... done
Creating tomcat ... done
[root@cyc docker_mysql_tomcat]# docker-compose  stop
Stopping mysql  ... done
Stopping tomcat ... done
[root@cyc docker_mysql_tomcat]# docker-compose  start
Starting mysql  ... done
Starting tomcat ... done
[root@cyc docker_mysql_tomcat]# docker-compose  restart
Restarting mysql  ... done
Restarting tomcat ... done
[root@cyc docker_mysql_tomcat]# docker-compose ps
 Name               Command               State           Ports
------------------------------------------------------------------------
mysql    /entrypoint.sh mysqld --da ...   Up      0.0.0.0:3306->3306/tcp
tomcat   catalina.sh run                  Up      0.0.0.0:8080->8080/tcp

~~~

### 5.4 docker-compose配置Dockerfile使用

> 使用docker-compose.yml文件以及Dockerfile文件在生成自定义镜像的同时启动当前镜像，并且由docker-compose去管理容器

~~~yml
# docker-compose.yml文件
version: '3.1'
services:
  maven_web:
    restart: always
    build:                # 构建自定义镜像
      context: ../        # 指定dockerfile文件的所在路径
      dockerfile: Dockerfile    # 指定Dockerfile文件名称
    image: maven_web:1.0.1
    container_name: maven_web
    ports:
      - 8081:8080
    environment:
      TimeZone: Asia/Shanghai
~~~

~~~markdown
# Dockerfile文件
from daocloud.io/library/tomcat:8.5.15-jre8
copy maven_web.war /usr/local/tomcat/webapps
~~~

~~~sh
# 可以直接启动基于docker-compose.yml以及Dockerfile文件构建的自定义镜像
  docker-compose up -d
  
# 如果自定义镜像不存在，会帮助我们构建出自定义镜像，如果自定义镜像已经存在，会直接运行这个自定义镜像

# 重新构建的话
# 重新构建自定义镜像
  docker-compose build

# 运行前，重新构建
docker-compose up -d --build
~~~



## 六、Docker CI

### 6.1 引言

> 项目部署
>
> 1. 将项目通过Maven进行编译打包
> 2. 将文件上传到指定的服务器中
> 3. 将war包放到tomcat的目录中
> 4. 通过Dockerfile将Tomcat和War包转成一个镜像，由DockerCompose去运行容器
>
> 项目更新了
>
> ​	将上述流程再次的从头到尾的执行一次

### 6.2 CI介绍

> CI （continuous  integration ）持续集成
>
> ​	持续集成：编写代码时，完成了一个功能后，立即提交代码到Git仓库中，将项目重新的构建并且测试
>
> - 快速发现错误
> - 防止代码偏离主分支 （写一点 测一点）

### 6.3 实现持续集成

#### 6.3.1 搭建GitLab服务器

> 1. 创建一个全新的虚拟机，并且至少指定4G的运行内存

> 2. 安装Docker以及docker-compose

> 3. docker-compose.yml文件去安装GitLab 

docker-compose.xml文件

~~~yml
version: '3.1'
services:
  gitlab: 
   image: 'twang2218/gitlab-ce-zh:11.1.4'
   container_name: "gitlab"
   restart: always
   privileged: true
   hostname: 'gitlab'
   environment: 
     TZ: 'Asia/Shanghai'
     GITLAB_OMNIBUS_CONFIG: 
       external_url 'http://192.168.199.110'
       gitlab_rails['time_zone'] = 'Asia/Shanghai'
       gitlab_rails['smtp_enable'] = true
   	   gitlab_rails['gitlab_shell_ssh_port'] = 22
   	 ports:
   	  - '80:80'
   	  - '443:443'
   	  - '22:22'    【注意这里22端口为远程连接虚拟机端口，会导致端口冲突，需要重新设置ssh端口号】
   	 volumes:
   	  - /opt/docker_gitlab/config:/etc/gitlab
   	  - /opt/docker_gitlab/data:/var/opt/gitlab
   	  - /opt/docker_gitlab/logs:/var/log/gitlab
~~~

端口号没改的话 搭建gitlab服务器就会报错

~~~sh
Creating gitlab ... error

ERROR: for gitlab  Cannot start service gitlab: driver failed programming external connectivity on endpoint gitlab (c76b439f4ee44f6df30861f49790fc2097d1d8e712c11f9ba8e716bf53649926): Error starting userland proxy: listen tcp 0.0.0.0:22: bind: address already in use   【地址已占用】

~~~

还没有搞好：docker-compose.xml文件有做变动，gitlab端口要修改



> **重新设置SSH端口号**

~~~sh
vi /etc/ssh/sshd_config    # 查看并修改SSH配置文件
~~~

~~~sh
#       $OpenBSD: sshd_config,v 1.100 2016/08/15 12:32:04 naddy Exp $

# This is the sshd server system-wide configuration file.  See
# sshd_config(5) for more information.

# This sshd was compiled with PATH=/usr/local/bin:/usr/bin

# The strategy used for options in the default sshd_config shipped with
# OpenSSH is to specify options with their default value where
# possible, but leave them commented.  Uncommented options override the
# default value.

# If you want to change the port on a SELinux system, you have to tell
# SELinux about this change.
# semanage port -a -t ssh_port_t -p tcp #PORTNUMBER
#
#Port 22           【修改之处：默认是#Port 22  修改为Port 60022】
#AddressFamily any
#ListenAddress 0.0.0.0
#ListenAddress ::

HostKey /etc/ssh/ssh_host_rsa_key
#HostKey /etc/ssh/ssh_host_dsa_key
HostKey /etc/ssh/ssh_host_ecdsa_key
HostKey /etc/ssh/ssh_host_ed25519_key

# Ciphers and keying
#RekeyLimit default none

# Logging
#SyslogFacility AUTH
SyslogFacility AUTHPRIV
#LogLevel INFO

# Authentication:

#LoginGraceTime 2m
#PermitRootLogin yes
#StrictModes yes
#MaxAuthTries 6
#MaxSessions 10

#PubkeyAuthentication yes

# The default is to check both .ssh/authorized_keys and .ssh/authorized_keys2
# but this is overridden so installations will only check .ssh/authorized_keys
AuthorizedKeysFile      .ssh/authorized_keys

#AuthorizedPrincipalsFile none

#AuthorizedKeysCommand none
#AuthorizedKeysCommandUser nobody

# For this to work you will also need host keys in /etc/ssh/ssh_known_hosts
#HostbasedAuthentication no
# Change to yes if you don't trust ~/.ssh/known_hosts for
# HostbasedAuthentication
#IgnoreUserKnownHosts no
"sshd_config" 139L, 3907C

#       $OpenBSD: sshd_config,v 1.100 2016/08/15 12:32:04 naddy Exp $

# This is the sshd server system-wide configuration file.  See
# sshd_config(5) for more information.

# This sshd was compiled with PATH=/usr/local/bin:/usr/bin

# The strategy used for options in the default sshd_config shipped with
# OpenSSH is to specify options with their default value where
# possible, but leave them commented.  Uncommented options override the
# default value.

# If you want to change the port on a SELinux system, you have to tell
# SELinux about this change.
# semanage port -a -t ssh_port_t -p tcp #PORTNUMBER
#
#Port 22          【修改之处：默认是#Port 22  修改为Port 60022】
#AddressFamily any
#ListenAddress 0.0.0.0
#ListenAddress ::

HostKey /etc/ssh/ssh_host_rsa_key
#HostKey /etc/ssh/ssh_host_dsa_key
HostKey /etc/ssh/ssh_host_ecdsa_key
HostKey /etc/ssh/ssh_host_ed25519_key

# Ciphers and keying
#RekeyLimit default none

# Logging
#SyslogFacility AUTH
SyslogFacility AUTHPRIV
#LogLevel INFO

# Authentication:

#LoginGraceTime 2m
#PermitRootLogin yes
#StrictModes yes
#MaxAuthTries 6
#MaxSessions 10

#PubkeyAuthentication yes

# The default is to check both .ssh/authorized_keys and .ssh/authorized_keys2
# but this is overridden so installations will only check .ssh/authorized_keys
AuthorizedKeysFile      .ssh/authorized_keys

#AuthorizedPrincipalsFile none

#AuthorizedKeysCommand none
#AuthorizedKeysCommandUser nobody

~~~

~~~sh
# 修改好后保存并重新启动sshd
systemctl restart sshd
~~~







### 6.4  CD介绍

> CD（持续交付、持续部署）
>
> 持续交付：将代码交付给专业的测试团队去测试
>
> 持续部署：将测试通过的代码，发布到生产环境

![image-20201130194313049](C:\Users\CYC\AppData\Roaming\Typora\typora-user-images\image-20201130194313049.png)



### 6.5 实现持续交付持续部署

#### 6.5.1 安装Jenkins

> 官网：https://www.jenkins.io/

~~~yml
# docker-compose.yml 文件
version: "3.1"
services:
  jenkins:
  	image: jenkins/jenkins
  	restart: always
  	container_name: jenkins
  	ports:
  	  - 8888:8080
  	  - 50000:50000
    volumes:
      - ./data:/var/jenkins_home
~~~

> 第一次运行时，会因为data目录没有权限，导致启动失败

~~~sh
chmod 777 data
~~~

> 访问http://192.168.199.109:8888

~~~
访问速度较慢 需要耐心等待~
~~~

> 输入密码（密码需要查看docker运行日志并找到）

> 手动指定插件安装，指定下面两个插件，等待安装即可~（可以修改为清华的镜像，具体需要去实践）
>
> publish ssh...
>
> git param...

#### 6.5.2 配置目标服务器及GitLab免密码登录

> GitLab --> Jenkins --> 目标服务器

> 1. Jenkins去连接目标服务器

~~~markdown
1. 左侧的系统设置

2. 选中系统设置

3. 搜索publish over SSH

4. 点击新增
~~~

#### 6.5.3 配置GitLab免密码登录

> 1. 登录Jenkins容器内部

~~~sh
docker exec -it jenkins bash
~~~

> 2. 输入生成SSH秘钥命令

~~~sh
ssh-keygen -t rsa -c "邮箱（随便都行）"
~~~

> 3. 将秘钥复制到GitLab的SSH中

#### 6.5.4 配置JDK和Maven

> 1. 复制本地的JDK和Maven的压缩包到data目录下

> 2. 手动解压

> 3. 在监控界面中配置JDK和Maven

#### 6.5.5 手动拉取GitLab项目

> 使用SSH无密码连接时，第一次连接需要手动确定