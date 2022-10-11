---

---

# 20221008

## 一、JavaWeb 概念

### 1、定义

所有使用浏览器访问的 Java 程序的总称。

> 基于成对出现的请求与响应。

### 2、分类

静态资源：html、css、js、mp4、jpg 等。

动态资源：Servlet 程序、jsp 页面。

### 3、常用的 WEB 服务器

Tomcat。

### 4、Tomcat 的使用

#### (1) 安装

解压如下压缩包即可。

![image-20221010211638118](Z:\postgraduate_learning\JavaWeb_Notes\img\Tomcat压缩包.png)

#### (2) 目录介绍

- bin：专门存放可执行程序。
- conf：专门存放配置文件。
- lib：专门存放依赖的 jar 文件。
- logs：专门存放日志文件。
- temp：专门存放运行时的临时数据。
- webapps：专门存放部署的 web 工程。
- work：Tomcat 工作时目录。

#### (3) 服务的启动

##### a. 使用 .bat 文件

双击 `bin/startup.bat` 启动 Tomcat 服务器。

##### b. 使用命令行

命令行切换到 `bin` 目录下，输入 `catalina run`。

这种方式可以更详细地显示错误信息。

#### (4) 服务的终止

双击 `bin/shutdown.bat` 停止 Tomcat 服务器。

#### (5) 修改 Tomcat 默认端口号

在 `conf/server.xml` 中进行修改。

```xml
    <Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
```

#### (6) 将 Web 工程部署到 Tomcat 服务器的方法

##### a. 直接拷贝

将项目文件直接拷贝到 `webapps` 文件夹中 。

##### b. 配置文件方式

在 `conf/Catalina/localhost` 文件夹中新建 `.xml` 配置文件。

`.xml` 文件案例：

```xml
<!--
	Context 为工程上下文
	path：工程访问路径，即 localhost:8080path
	docBase：工程实际存放的地址

-->
<Context path="/book" docBase="Z:\book" />
```

> `.xml` 文件编码方式必须为 `UTF-8`。

#### (7) 使用浏览器直接打开 HTML 文件和使用 Tomcat 访问的区别

##### a. 直接打开

使用 `file://` 协议，告诉浏览器解析磁盘中的内容。

##### b. 使用 Tomcat 访问

使用 `http://` 协议，请求访问并解析服务器中的内容。

![image-20221010213656537](Z:\postgraduate_learning\JavaWeb_Notes\img\HTTP方式解析页面.png)

#### (8) 默认访问页面

输入 `http://ip:port` 将默认访问 `http://ip:port/root/index.html`。

##### (9) IDEA 整合 Tomcat

在 `Settings/Application Servers` 下进行服务器的配置。

![image-20221010214006457](Z:\postgraduate_learning\JavaWeb_Notes\img\IDEA整合Tomcat.png)

# 20221009

## 一、IDEA JavaWeb

### 1、使用 IDEA 创建动态 Web 工程

第一步：新建模块

![image-20221010214317987](Z:\postgraduate_learning\JavaWeb_Notes\img\新建模块.png)

第二步：配置模块

左侧生成器选择 `Java Enterprise`，Template 选择 `Web Application`，选取对应的服务器。

### 2、目录结构

>生成项目目录的小技巧：
>
>​		在项目文件夹内，使用快捷键 `Shift + 右击` ，启动 Windows PowerShell 命令窗口。
>
><img src="Z:\postgraduate_learning\JavaWeb_Notes\img\启动PowerShell.png" alt="image-20221010215152519" style="zoom: 50%;" />
>
>在命令行窗口输入 `tree /f >project.txt` 项目目录便存入了 project.txt 文本文件中。

├─java
│  └─com
│      └─zzy
│          └─servlet
│                  HelloServlet.java
│                  HelloServlet2.java
│                  HelloServlet3.java
│                  
├─resources
└─webapp
    │  a.html
    │  b.html
    │  index.jsp
    │  
    └─WEB-INF
            web.xml

`webapp` 专门存放 web 工程资源文件。

`WEB-INF` 是受服务器保护的目录，浏览器无法直接访问此目录的内容。

`web.xml` 是整个 web 工程的配置文件。

将20221009/20221010整合于此。

