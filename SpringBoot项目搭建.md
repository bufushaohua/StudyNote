## SpringBoot

#### 1.快速搭建一个基于SpringBoot的Web项目

1.使用IntelliJ IDEA创建一个Maven工程

2.添加依赖：

- 首先添加spring-boot-starter-parent作为parent，代码如下：

~~~xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.2.2.RELEASE</version>
</parent>
~~~

- 要开发一个Web项目，还需要引入Web的Starter，代码如下：

~~~xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
</dependencies>
~~~

3.编写启动类

​	创建项目的启动入口类，在maven工程的java目录下创建项目的包，包里创建一个App

类，代码如下：

~~~java
@SpringBootApplication
/**
 * 由三个注解组成
 *     1.@SpringBootConfiguration：标明这是一个配置类，开发者可以在这个类中配置Bean，
 *              有点类似于Spring中的applicationContext.xml文件的角色
 *     2.@EnableAutoConfiguration：表示开启自动化配置。SpringBoot中的自动化配置是非侵入式的
 *              在任何时刻，开发者都可以使用自定义配置代替自动化配置中的某一项配置
 *     3.@ComponentScan完成包扫描
 *
 *     定制Banner：
 *          1.在resources目录下创建一个banner.txt文件，在这个文件中写入的文本将在项目启动时打印出来
 *          2.如何关闭：修改项目启动类的main方法
 *              public static void main(String[] args) {
 *                  SpringApplicationBuilder springApplicationBuilder = new SpringApplicationBuilder(BlogApplication.class);
 *                  springApplicationBuilder.bannerMode(Banner.Mode.OFF).run(args);
 *               }
 */
public class MyBootApplication {
    public static void main(String[] args) {
        SpringApplication.run(MyBootApplication.class,args);
    }
}
~~~

4.创建SpringMVC中的控制器——HelloController，代码如下：

~~~java
@RestController
public class HelloController {
    @GetMapping("/hello")
    public String hello(){
        return "Hello，Spring Boot！";
    }
}
~~~

#### 2.项目启动

**1.使用Maven命令启动，命令如下**：

~~~markdown
mvn springboot:run
~~~

**2.直接运行main方法**

**3.打包启动**

- 要将SpringBoot打包成jar包运行，首先要添加一个Plugin到pom.xml文件中，代码如下：

~~~xml
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <version>2.2.2.RELEASE</version>
        </plugin>
    </plugins>
</build>
~~~

- 然后运行mvn命令进行打包，代码如下：

~~~markdown
mvn package
~~~

> 打包完成后，会在target目录下生成一个jar文件，通过java -jar 命令直接启动这个jar文件

#### 3.Web容器配置

**Tomcat配置**

常规配置：

~~~yaml
server:
	port:8080
	error:
		path: /error
	servlet:
		session:
			timeout: 30m
		context-path: /test
	tomcat:
		uri-encoding: utf-8
		max-threads: 500
		basedir: /home/sang/tmp
~~~

代码解释：

- server.port=8080 配置了Web容器的端口号

- error.path配置了当项目出错时跳转去的页面

- session.timeout配置了session失效时间，30m表示30分钟，如果不写单位，默认单位是秒。

  由于tomcat配置session过期时间以分钟为单位。因此这里单位如果是秒的话，改时间会被转换为不超过所配置描述的最大分钟数，例如这里配置了110，默认单位为秒，则实际session过期时间为1分钟

- context-path表示项目名称，不配置默认为/。如果配置了，就要在访问路径中加上配置的路径

- uri-encoding表示配置tomcat请求编码

- max-threads表示Tomcat最大线程数

- basedir是一个存放Tomcat运行日志和临时文件的目录，若不配置，则默认使用系统的临时目录

Spring Boot项目中的application.properties配置文件一共可以出现在如下4个位置：

- 项目根目录下的config文件夹中
- 项目根目录下
- classpath下的config文件夹中
- classpath下

加载的优先级从1到4依次降低

![image-20200525154459761](C:\Users\CYC\AppData\Roaming\Typora\typora-user-images\image-20200525154459761.png)

若开发中未使用application.properties，而是使用了application.yml作为配置文件，那么配置文件的优先级与上图一致