# Nginx

## 一、Nginx介绍

### 1.1 引言

> 为什么要学习Nginx  
>
> 问题1：客户端到底要将请求发送到哪台服务器。
>
> 问题2： 如果所有客户端的请求都发送给了服务器1。
>
> 问题3： 客户端发送的请求可能是申请动态资源的，也有申请静态资源。

~~~java
服务器搭建后
~~~



![image-20201205125953470](C:\Users\CYC\AppData\Roaming\Typora\typora-user-images\image-20201205125953470.png)

### 1.2 Nginx介绍

> Nginx的特点：
>
> 1. 稳定性极强，7*24小时不间断运行
>2. Nginx提供了非常丰富的配置实例
> 3. 占用内存小，并发能力强
>



## 二、Nginx安装

### 2.1 安装Nginx

~~~yml
version： '3.1'
services:
	nginx:
		restart: always
		image: daocloud.io/library/nginx:latest
		container_name: nginx
		ports:
			- 80:80
~~~

### 2.2 Nginx配置文件

> 关于Nginx的核心配置文件
>
> ​	进入方式：
>
> ​		1、docker ps  【查看Nginx容器名称】
>
> ​		2、docker exec -it [容器id]  bash  【进入容器内部】
>
> ​		3、cd /etc/nginx
>
> ​		4、cat nginx.conf  【查看Nginx的配置文件】

~~~json
# worker_processes的数值越大，Nginx的并发能力就越强
# error_log：代表Nginx错误日志的存放位置

user  nginx;
worker_processes  1;

error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;

# 以上统称为全局块

# event块
# worker_connections它的数值越大，Nginx并发能力越强

events {
    worker_connections  1024;
}


# http块
# include代表引入一个外部的文件  --> /mine.types中放着大量的媒体类型,default_type表示默认类型
# include /etc/nginx/conf.d/*.conf   --> 引入了conf.d目录下的以.conf为结尾的配置文件


http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout  65;

    #gzip  on;
    
	include /etc/nginx/conf.d/*.conf；
	【include /etc/nginx/conf.d/default.conf文件内容如下】
	server {
        listen       80;
        listen  [::]:80;
        server_name  localhost;

        location / {
            root   /usr/share/nginx/html;
            index  index.html index.htm;
        }

        # location块
        # root：将接收到的请求根据/usr/share/nginx/html去查找静态资源
        # index：默认去上述的路径中找到index.html或者index.htm
	}
	
	# server块
	# listen： 代表Nginx监听的端口号
	# localhost：代表Nginx接收请求的ip

}

~~~

2.3 修改docker-compose文件

~~~yml
version： '3.1'
services:
	nginx:
		restart: always
		image: daocloud.io/library/nginx:latest
		container_name: nginx
		ports:
	      - 80:80
		volumes:
		  - /opt/docker_nginx/conf.d:/etc/nginx/conf.d
~~~



~~~json
# /opt/docker_nginx/conf.d/default.conf
server{
  listen 80;
  server_name localhost;

  location / {
    root /usr/share/nginx/html;
    index index.html index.htm;
  }
}

~~~



## 三、Nginx的反向代理

### 3.1 正向代理和反向代理的介绍

> 正向代理：
>
> 1. 正向代理服务时由客户端设立的
>
> 	2. 客户端了解代理服务器和目标服务器都是谁
>  	3. 帮助实现突破访问权限，提高访问的速度，对目标服务器隐藏客户端的ip地址

>反向代理：
>
>1. 反向代理服务器是配置在服务端的
>2. 客户端是不知道访问的到底是哪一台服务器
>3. 达到负载均衡，并且可以隐藏服务器真正的ip地址

### 3.2 基于Nginx实现反向代理

> 准备一个目标服务器



四、Nginx负载均衡



五、Nginx动静分离



六、Nginx集群