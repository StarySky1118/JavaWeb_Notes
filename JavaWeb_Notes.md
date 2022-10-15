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

![](img/Tomcat压缩包.png)

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

![](img/HTTP方式解析页面.png)

#### (8) 默认访问页面

输入 `http://ip:port` 将默认访问 `http://ip:port/root/index.html`。

##### (9) IDEA 整合 Tomcat

在 `Settings/Application Servers` 下进行服务器的配置。

![](img/IDEA整合Tomcat.png)

# 20221009

## 一、IDEA JavaWeb

### 1、使用 IDEA 创建动态 Web 工程

第一步：新建模块

![](img/新建模块.png)

第二步：配置模块

左侧生成器选择 `Java Enterprise`，Template 选择 `Web Application`，选取对应的服务器。

### 2、目录结构

>生成项目目录的小技巧：
>
>​		在项目文件夹内，使用快捷键 `Shift + 右击` ，启动 Windows PowerShell 命令窗口。
>
>![](img/启动PowerShell.png)
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

### 3、为 JavaWeb 工程添加依赖

#### (1) 选择“项目结构”



![image-20221011155911189](img/image-20221011155911189.png)

#### (2) 添加依赖

![image-20221011160043470](img/image-20221011160043470.png)

在 Artifacts 中检查部署选项。

### 4、将工程部署到 Tomcat 

#### (1) 编辑配置

![image-20221011160300856](img/image-20221011160300856.png)

#### (2) 部署配置

![image-20221011160423323](img/image-20221011160423323.png)

选择将什么部署到 Tomcat 中。

#### (3) 默认 url 设置

![image-20221011160818140](img/image-20221011160818140.png)

需要保证此 url 与上图 Application Content 保持一致。

> Application Content 就是工程路径。

#### (4) 实例的重启

![image-20221011161331387](img/image-20221011161331387.png)

### 5、相关配置更改

- 设置资源热部署

![image-20221011161440449](img/image-20221011161440449.png)

# 20221010

## 一、Servlet

### 1、什么是 Servlet

- Java EE 规范之一。规范即为接口。
- 是 JavaWeb 三大组件之一。另外两个为 Filter 过滤器和 Lisenter 监听器。
- 是服务器上的一个 Java 程序，可以接收客户端请求，并向客户端发送数据。

### 2、Servlet 手动实现

#### 实现方式一：实现 Servlet 接口

在 `src/main/java` 下创建一个类，实现 Servlet 接口。

```java
public class HelloServlet implements Servlet {
    @Override
    public void init(ServletConfig servletConfig) throws ServletException {

    }

    @Override
    public ServletConfig getServletConfig() {
        return null;
    }

    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        System.out.println("Hello Servlet");
    }

    @Override
    public String getServletInfo() {
        return null;
    }

    @Override
    public void destroy() {

    }
}
```

其中，`service()` 方法专门处理 Servlet 请求。

实现 Servlet 类后，还要配置使用怎样的 url 才能够使用这个类。

在 `src/main/webapp/WEB-INF/web.xml` 中进行配置。

```xml
    <servlet>
        <!--类的别名-->
        <servlet-name>HelloServlet</servlet-name>
        <!--全类名-->
        <servlet-class>com.servlet.HelloServlet</servlet-class>
    </servlet>
    
    <servlet-mapping>
        <!--类的别名-->
        <servlet-name>HelloServlet</servlet-name>
        <!--url配置-->
        <!--http://ip:port/工程路径/hello-->
        <url-pattern>/hello</url-pattern>
    </servlet-mapping>
```

> url 寻找资源的过程：
>
> ![image-20221011163442020](img/image-20221011163442020.png)

### 3、Servlet 生命周期

Servlet 生命周期分为四步：

- 调用构造方法，创建实例

- `init()` 方法，进行初始化

  前两个过程只执行一次

- `service()` 处理请求
- `destroy()` 销毁

### 4、Servlet 请求的分发处理

> Http 请求分为 get 请求和 post 请求。

因此，如果想要对 Http 请求进行分发处理，首先要获取请求的类型。

```java
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        System.out.println("Hello Servlet");
    }
```

`ServletRequest` 接口中无法获取请求的类型，实现 `ServletRequest` 接口的 `HttpServletRequest` 接口中有获取请求类型的方法 `getMethod()`。

```java
String getMethod();
```

客户端发来的 Http 请求一定是 `HttpServletRequest` 接口的实现类，将 `servletRequest` 向下转型为 `HttpServletRequest` 接口类型，一定不会导致类型转换问题。

```java
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        // 类型转换
        HttpServletRequest httpServletRequest = (HttpServletRequest) servletRequest;

        // 请求分发
        if("GET".equals(httpServletRequest.getMethod())) {
            System.out.println("收到了get请求");
        } else {
            System.out.println("收到了 post 请求");
        }
    }
```

### 5、通过继承 `HttpServlet` 类实现 Servlet

`HttpServlet` 间接实现了 `Servlet`，并且更加明确程序是处理 HTTP 请求的。

#### (1) 继承 `HttpServlet` 

为了实现请求的分发处理，重写处理两种请求的方法。

```java
public class HelloServlet02 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("收到了get方法");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("收到了post方法");
    }
}
```

抽象类 `HttpServlet` 的 `doGet` 和 `doPost` 方法具有默认实现。

```java
// 具有默认实现的 doGet()    
protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String protocol = req.getProtocol();
        String msg = lStrings.getString("http.method_get_not_supported");
        if (protocol.endsWith("1.1")) {
            resp.sendError(405, msg);
        } else {
            resp.sendError(400, msg);
        }

    }
```

```java
// 具有默认实现的 doPost()    
protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String protocol = req.getProtocol();
        String msg = lStrings.getString("http.method_post_not_supported");
        if (protocol.endsWith("1.1")) {
            resp.sendError(405, msg);
        } else {
            resp.sendError(400, msg);
        }

    }
```

#### (2) 在 `web.xml` 中进行配置

### 6、使用 IDEA 工具生成 Servlet 程序

#### (1) 生成继承 `HttpServlet` 的类

并且，类中会重写 `doGet` 和 `doPost` 方法。

```java
public class HelloServlet03 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        
    }

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }
}
```

#### (2) `web.xml` 中自动填充

会自动填入 `<servlet>` 标签，`<servlet-mapping>` 需要自己填写。

# 20221011

## 一、Servlet 继承谱系

### 1、继承关系与相关方法

- `servlet` 接口

  抽象方法 `service()`。

  ```java
  void service(ServletRequest var1, ServletResponse var2) throws ServletException, IOException;
  ```

  

  - `GenericServlet` 抽象类

    `service()` 方法仍然是抽象的。

    ```java
    public abstract void service(ServletRequest var1, ServletResponse var2) throws ServletException, IOException;
    ```

    

    - `HttpServlet` 抽象类

      对 `service()` 方法有了默认实现。将对请求进行分发，例如方法为 GET 时，会调用`doGet()`。

      而 `doGet()` 方法在类中也有基本的默认实现，具体的功能可以继承该类重写实现。

      ```java
      protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          	// 获取请求的方法
              String method = req.getMethod();
              long lastModified;
              if (method.equals("GET")) {
                  lastModified = this.getLastModified(req);
                  if (lastModified == -1L) {
                      this.doGet(req, resp);
                  } else {
                      long ifModifiedSince = req.getDateHeader("If-Modified-Since");
                      if (ifModifiedSince < lastModified) {
                          this.maybeSetLastModified(resp, lastModified);
                          this.doGet(req, resp);
                      } else {
                          resp.setStatus(304);
                      }
                  }
              } else if (method.equals("HEAD")) {
                  lastModified = this.getLastModified(req);
                  this.maybeSetLastModified(resp, lastModified);
                  this.doHead(req, resp);
              } else if (method.equals("POST")) {
                  this.doPost(req, resp);
              } else if (method.equals("PUT")) {
                  this.doPut(req, resp);
              } else if (method.equals("DELETE")) {
                  this.doDelete(req, resp);
              } else if (method.equals("OPTIONS")) {
                  this.doOptions(req, resp);
              } else if (method.equals("TRACE")) {
                  this.doTrace(req, resp);
              } else {
                  String errMsg = lStrings.getString("http.method_not_implemented");
                  Object[] errArgs = new Object[]{method};
                  errMsg = MessageFormat.format(errMsg, errArgs);
                  resp.sendError(501, errMsg);
              }
      
          }
      ```

      `doGet()` 默认实现方法：

      ```java
          protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
              String protocol = req.getProtocol(); // 获取协议
              // 在 lStrings 中获取要输出的信息
              String msg = lStrings.getString("http.method_get_not_supported");
              if (protocol.endsWith("1.1")) {
                  resp.sendError(405, msg);
              } else {
                  resp.sendError(400, msg);
              }
      
          }
      ```

### 	2、Servlet 生命周期

Servlet 生命周期对应 Servlet 中的三个方法：`init()`、`service()` 和 `destroy()`。

第一次请求时，Tomcat 会实例化并初始化。

这会导致第一次请求时，等待时间过长。

#### (1) 在特定时间进行初始化

为了加快第一次请求的访问速度，可以将 Servlet 类实例化的时机改为 Tomcat 启动时。

在 `web.xml` 中添加 `<load-on-startup>` 标签来更早地初始化 Servlet 实例。

```xml
    <servlet>
        <servlet-name>HelloServlet02</servlet-name>
        <servlet-class>com.servlet.HelloServlet02</servlet-class>
        <load-on-startup>1</load-on-startup>
    </servlet>
```

标签 `<load-on-startup>` 最小值为 0。

这样，我还没有请求，Servlet 实例就创建好了。

```
Connected to server
[2022-10-11 08:07:55,122] Artifact web03:war exploded: Artifact is being deployed, please wait...
11-Oct-2022 20:07:55.301 警告 [RMI TCP Connection(2)-127.0.0.1] org.apache.tomcat.util.descriptor.web.WebXml.setVersion Unknown version string [4.0]. Default version will be used.
init方法...
```

> 想提高系统的启动速度，可以在请求时再实例化 Servlet 对象。
>
> 想提升每个用户的请求速度，可以将 Servlet 实例化过程提前。

#### (2) Servlet 在容器中的特性

Servlet 在容器中是单例的、线程不安全的。

> 线程不安全：Servlet 实例的成员变量没有加锁，导致线程间自由地使用成员变量。

启发：尽量不要在 Servlet 中定义成员变量，如果非要不可，那么不要让线程自由更改成员变量的值。

## 二、Http 协议

> 在浏览器开发者工具 (F12) 点击“ 网络”，即可查看各种 Http 请求。
>
> ![image-20221011202923866](img/image-20221011202923866.png)

### 1、无状态

### 2、组成部分

Http 包含请求与响应。

#### (1) 请求

请求包含三部分：

![./images](img/img002.16e080e9.png)

- 请求行

  展示当前请求最基本的信息。

  - 请求方式
  - 访问地址
  - Http 协议版本

- 请求消息头

  包含客户端告知服务端的很多信息。

- 请求主体

  - get 方式：没有请求体，有 QueryString，跟在 url 后面。
  - post 方式：有请求体，Form Data。
  - json：有请求体，request payload。

#### (2) 响应

![./images](img/img007.a6ab27cd.png)

响应也包含三部分：

- 响应行

  此次响应的简要信息描述。

  - 协议
  - 响应状态码
    - 500：服务器内部错误
    - 200 OK：正常响应
    - 302 Found：重定向
    - 404：找不到资源
    - 405：响应状态不支持
  - 响应状态短语
    - OK

- 响应头

  包含服务器的各种信息。

- 响应体

  响应的实际内容。

# 20221013

## 一、会话

### 1、HTTP 的无状态

>HTTP 无法区分请求主体。
>
>无状态带来的问题：无法执行多次 HTTP 请求才能完成的动作。

可以通过会话跟踪技术解决 HTTP 无状态问题。

### 2、Session 会话技术原理

当客户端第一次对服务器发起请求时，服务器会给客户端发送 SessionId。当下一次客户端请求服务器时，带上这个 SessionId 便可以实现 Http 的有状态。

使用案例：

```java
public class HelloServlet03 extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 获取 Session
        HttpSession session = req.getSession();

        // 打印 Session ID
        System.out.println("Session ID: " + session.getId());
    }
}
```

### 3、会话中常用的 API

```java
// 获取会话
HttpSession session = HttpServletRequest实例.getSession();
// 获取会话，没有会话不会创建新会话
HttpSession session = HttpServletRequest实例.getSession(false);
// 获取会话Id
session.getId(); 
// 判断当前 Session 是否为新的
session.isNew();
// 获取 Session 有效时间/最大无激活时间，默认1800秒
session.getMaxInactiveInterval();
// 设置 Session 有效时间，单位：秒
session.setMaxInactiveInterval();
// 让会话立即失效
session.invalidate();
// 获取 Session 创建时间
session.getCreationTime();
```

### 4、Session 保存作用域

#### (1) 定义

会话中可以保存数据。Session 保存作用域是和某个具体的会话对应的。

#### (2) 常用 API

```java
// 设置属性
void session.setAtrribute(k, v);
// 获取属性
Object session.getAttribute(k)
// 移除属性
void session.removeAttribute(k)
```

使用案例：

`HelloServlet03` 可以设置 Session 的属性。

`HelloServlet04` 可以获取 Session 的属性。

依次访问 `HelloServlet03` 和 `HelloServlet04`，如果是同一客户端进行访问，`HelloServlet04` 便可以访问到 Session 的属性。若一个客户端访问了 `HelloServlet03`，另一个客户端访问了 `HelloServlet04`，便无法访问到 Session 属性，这就达成了用户的区分与隔离。

```java
public class HelloServlet03 extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 获取 Session
        HttpSession session = req.getSession();

        // 设置 Session 属性
        session.setAttribute("sName", "lina");
    }
}
```

```java
public class HelloServlet04 extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 获取 Session
        HttpSession session = req.getSession();

        // 获取属性
        Object sName = session.getAttribute("sName");

        System.out.println(sName);
    }
}
```

## 二、服务端转发和客户端重定向

若服务器的一个组件接收到请求后，需要其他组件对该请求进行处理，可以使用服务器转发和客户端重定向两种方式将请求发至其他组件。

#### 1、服务器内部转发

只有一次请求与响应，请求在客户端内部转发了多少次客户端不知道。最显著的特征是浏览器地址栏没有变化。

请求转发方式可以访问到 `WEB-INF` 内的文件。

原理图：

![image-20221013153244100](img/image-20221013153244100.png)

服务器内部转发用到的 API：

```java
// 获取 RequestDispatcher
请求实例.getRequestDispatcher(地址);
// 转发请求与响应
RequestDispatcher实例.forword(请求实例, 响应实例);
```

使用案例：

`HelloServlet03` 中会将请求与响应转发至 `HelloServlet04`。

```java
public class HelloServlet03 extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 服务器端内部转发
        req.getRequestDispatcher("hello04").forward(req, resp);
    }
}
```



#### 2、客户端重定向

每多一次重定向会多一次请求与响应。最显著的特征是浏览器地址栏变化。

![image-20221013153446976](img/image-20221013153446976.png)

使用到的 API：

```java
// 在响应中包含重定向地址，因此是响应实例调用
响应.sendRedirect(地址);
```

使用案例：

`HelloServlet03` 响应中包含重定向信息。

```java
public class HelloServlet03 extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("hello03...");
        // 进行重定向
        resp.sendRedirect("hello04");
    }
}
```

`HelloServlet04` 将对请求进行处理。

```java
public class HelloServlet04 extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("hello04收到请求，进行处理");
    }
}
```

## 三、ServletConfig 类

Servlet 配置信息类，ServletConfig 实例也是在 Servlet 实例创建时创建，其中封装了初始化配置信息。

### 1、三大作用

- 获取 Servlet 程序的别名 servlet-name

  使用方法：

  ```java
  类的别名 = ServletConfig实例.getServletName()
  ```

  使用案例：

  ```java
      public void init(ServletConfig servletConfig) throws ServletException {
          System.out.println(servletConfig.getServletName());
      }
  ```

- 获取初始化参数 init-param

  可以在 `web.xml` 中的 servlet 标签中进行添加。

  例如：

  ```xml
      <servlet>
          <!--类的别名-->
          <servlet-name>HelloServlet</servlet-name>
          <!--全类名-->
          <servlet-class>com.servlet.HelloServlet</servlet-class>
          
          <init-param>
              <param-name>serverNo</param-name>
              <param-value>110</param-value>
          </init-param>
      </servlet>
  ```

  使用方法：

  ```java
  String getInitParm(String name)
  ```

  使用案例：

  ```java
  System.out.println("获取到的init-param参数：" + servletConfig.getInitParameter("serverNo"));
  ```

- 获取 ServletContext 对象

  使用 API：

  ```java
  ServletContext getservletContext();
  servletContext实例 = ServletConfig实例.getservletContext()
  ```
  

# 20221014

## 一、ServletContext 类

- Servlet 上下文。

- 一个 web 工程，只有一个 ServletContext 对象。

- ServletContext 对象是一个域对象。

  - 像 Map 一样放入数据、取数据和删除数据。

    `setAttribute()`、`getAttribute` 和 `removeAttribute()`。

  - 这里的域指的是存取数据的操作范围：整个 web 工程。

- 在 web 工程部署时创建，在 web 工程结束时销毁。

创建 `ServletContext ` 对象可以使用这种方式：

```java
ServletContext servletContext = this.getServletConfig().getServletContext();
```

也可以使用更简单的方式：

```java
this.getServletContext()
```

这是因为在 `GenericServlet` 中：

```java
public ServletContext getServletContext() {
    ServletConfig sc = this.getServletConfig(); // 自动调用getServletConfig()方法
    if (sc == null) {
        throw new IllegalStateException(lStrings.getString("err.servlet_config_not_initialized"));
    } else {
        return sc.getServletContext();
    }
}
```

### 1、作用

#### (1) 获取 `web.xml` 中配置的上下文参数 `context-param`

在 `web.xml` 中进行配置：

```xml
    <!--上下文参数-->
    <context-param>
        <param-name>username</param-name>
        <param-value>context</param-value>
    </context-param>
```

上下文参数是整个 web 工程的，因此是独立的标签。

使用 ServletConfig 实例获取 ServletContext 实例。

```java
public class ContextServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 获取上下文对象
        ServletContext servletContext = this.getServletConfig().getServletContext();

        // 输出上下文参数
        System.out.println(servletContext.getInitParameter("username"));
    }

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    }
}
```

#### (2) 获取当前工程路径

使用到的 API：

```java
servletContext.getContextPath() -> String
```

使用案例：

```java
// 获取当前工程路径
System.out.println("当前工程路径为：" + servletContext.getContextPath());
```

执行结果：

```java
当前工程路径为：/web03
```

#### (3) 获取工程部署在硬盘上的绝对路径

使用到的 API：

```java
// path 是浏览器访问的虚拟地址
servletContext.getRealPath(String path) -> String
```

使用案例：

```java
// 获取当前工程在服务器硬盘地址
// 斜杠 / 会被服务器解析为 http://ip:port/工程路径
// 以下代码会返回整个工程在服务器上的硬盘地址
System.out.println("当前工程在服务器硬盘上的地址为：" + servletContext.getRealPath("/"));
```

以上代码执行结果为：

```
当前工程在服务器硬盘上的地址为：Z:\postgraduate_learning\JavaWeb\web03\target\web03-1.0-SNAPSHOT\
```

#### (4) 像 Map 一样存取数据

在某个 Serlvet 程序中可以设置键值对，一旦设置，其他 Servlet 程序也可以访问键值对。

使用案例：

在 `ContextServlet` 类中设置键值对

```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    // 获取 ServletContext 对象
    ServletContext servletContext = getServletContext();

    // 设置键值对
    servletContext.setAttribute("username", "mary");

}
```

设置完成后， `HelloServlet03` 对象也可以访问。

```java
protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    // 获取 Servlet 上下文对象
    ServletContext servletContext = getServletContext();

    // 获取键值对
    Object username = servletContext.getAttribute("username");
    System.out.println(username);
}
```

## 二、MIME 数据类型

MIME 是 HTTP 中的数据类型。格式：大类型/小类型。

![image-20221014153209007](img/image-20221014153209007.png)

## 三、HTTPServletRequest 类

### 1、作用

每次有请求进入 Tomcat 服务器，Tomcat 服务器都会将请求的所有信息封装到 Request 中，然后传递到 `doGet()` 和 `doPost()` 方法中，供我们使用。

### 2、常用 API

```java
// 获取请求的资源路径
HTTPServletRequest实例.getRequestURI() -> String
// 获取请求 url
HTTPServletRequest实例.getRequestURL() -> StringBuffer
// 获取客户端 ip 地址    
HTTPServletRequest实例.getRemoteHost()
// 获取请求头
HTTPServletRequest实例.getHeader(String) -> String;
// 获取请求参数
// 这里的请求参数就是表单中提交的参数
HTTPServletRequest实例.getParameter(String) -> String;
// 获取多个请求参数
request.getParameterValues(String) -> String[];
// 获取请求方式
request.getMethod() -> String;
// 设置域数据
request.setAttribute(String, Object) -> void;
// 获取域数据
request.getAttribute(String) -> Object;
// 获取请求转发对象
request.getRequestDispatcher(String) -> RequestDispatcher
```

### 3、请求参数的获取

小案例：在 `a.html` 中画一个表单，使用 `HelloServlet03` 类获取提交的参数。

`a.html`：

```html
<form action="http://localhost:8080/web03/hello03" method="post">
    用户名<input type="text" name="username"><br>
    密码<input type="password" name="password" ><br>
    兴趣爱好<input type="checkbox" name="hobby" value="fwc">俯卧撑
    <input type="checkbox" name="hobby" value="yt">引体向上
    <input type="checkbox" name="hobby" value="yl">哑铃
    <br>
    <input type="submit" value="提交">
</form>
```

`HelloServlet03`：

```java
public class HelloServlet03 extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String username = req.getParameter("username");
        String password = req.getParameter("password");
        String[] hobbies = req.getParameterValues("hobby");

        System.out.println("用户名: " + username);
        System.out.println("密码：" + password);
        for (String hobby : hobbies) {
            System.out.println(hobby);
        }
    }
}
```

Post 方式会出现的 bug：输入框内填入中文，控制台获取到无法输出。

可以设置一下请求的字符集：

```java
req.setCharacterEncoding("UTF-8");
```

## 四、Base 标签

base 标签是 HTML 的一个标签。

如果跳转到了某个页面，使用相对路径进行跳转时，不会将地址栏中的地址作为当前页面的地址，而是将使用 base 标签中的地址作为当前页面的地址。

使用案例：

`HelloServlet03`：

```java
public class HelloServlet03 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 请求转发
        req.getRequestDispatcher("/a/b/Hello.html").forward(req, resp);
    }
}
```

`a/b/Hello.html`：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Hello</title>
    <!--进行相对路径跳转时参照的地址-->
    <!--浏览器会将 / 解析为 http://ip:port/-->
    <base href="/web03/a/b/Hello.html">
</head>

<body>
    <p>你好</p>
    <a href="../../a.html">返回首页</a>
</body>
</html>
```

请求转发发生后，地址栏为：

```
http://ip:port/工程路径/HelloServlet03路径
```

使用 `a/b/Hello.html` 中的相对路径跳转一定会导致地址无效，因此 `a/b/Hello.html` 需要设置 base 标签规定相对路径跳转时的参考位置。

## 五、Web 中的绝对路径和相对路径

### 1、绝对路径与相对路径

相对路径：

- . 当前目录
- .. 上一级目录
- 资源名 当前目录资源名

绝对路径：

http://ip:port/工程路径/资源路径

### 2、/ 的含义

/ 是绝对路径。

/ 被浏览器解析为：localhost:8080

例如：

```html
<a href="/">斜杠</a>
```

/ 被服务器解析为：http://ip:port/工程路径。

例如：

```xml
<servlet-mapping>
	<servlet-name>HelloServlet02</servlet-name>
	<url-pattern>/hello02</url-pattern>
</servlet-mapping>
```

```java
// 获取绝对路径
servletContext.getRealPath("/");
// 获取请求分发器
request.getRequestDispatcher("/")
```

特殊情况：

```java
response.sendRedirect("/")
```

上面的代码进行重定向，会将 / 发给客户端，客户端浏览器解析 / 为 `localhost:8080`。

# 20221015

## 一、`HttpServletResponse` 类

### 1、作用

和 `HttpServletRequest` 类一样，每次服务器得到请求，都会将响应信息封装到 `HttpServletResponse` 类中。

### 2、两个输出流

- 字节流

  使用的 API：

  ```java
  ServletOutputStream outputStream = response.getOutputStream();
  ```

  传递二进制数据。下载。

- 字符流

  使用的 API：

  ```java
  PrintWriter writer = response.getWriter();
  ```

  回传字符串。常用。

只能使用其中的一个。

### 3、如何给客户端回传数据

使用案例： `Servlet05` 程序可以给浏览器回传字符串。

```java
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 获取字符输出流
        PrintWriter writer = response.getWriter();

        // 输出
        writer.write("你好，您的请求我已经收到！");
    }
```

> 会出现的 bug：中文乱码。
>
> 出现原因：`response` 默认字符集为 ISO-8859-1，此字符集不支持中文。
>
> 解决措施：
>
> ​	1、设置本次响应的编码方式为 UTF-8 
>
> ​	2、告知浏览器以 UTF-8 方式解码

```java
// 设置响应使用的字符集
response.setCharacterEncoding("UTF-8");

// 通过响应头，告知浏览器使用 UTF-8 进行解码
response.setHeader("Content-Type", "text/html; charset=UTF-8");
```

通过以上代码，可以得到如下的响应：

```
HTTP/1.1 200 OK
Server: Apache-Coyote/1.1
Content-Type: text/html;charset=UTF-8
Content-Length: 39
Date: Sat, 15 Oct 2022 09:07:29 GMT
```

> 解决措施二：同时设置服务器响应和客户端的字符集。
>
> 获取流之前使用。

```
// 服务器响应和客户端都使用 UTF-8 字符集
response.setContentType("text/html; charset=UTF-8");
```

### 4、请求重定向

原理图：

![image-20221015172128666](img/image-20221015172128666.png)

使用到的 API：

```java
response.sendRedirect(String path);
```

>参数中的 path 将交由服务器进行解析，传入 / 时会解析为 http://localhost:8080。

使用案例：`ResponseServlet1` 请求重定向至 `ResponseServlet2`。

解决方案1：直接使用上面的 API。

```java
public class ResponseServlet1 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 进行重定向
        response.sendRedirect("/web04/r2");
    }

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    }
}
```

```java
public class ResponseServlet2 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("收到请求，正在处理");
    }

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }
}
```

解决方案2：`ResponseServlet1` 收到请求后，修改响应状态码，并设置响应头中的 Location 属性，帮助客户端进行重定向。

```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 设置响应头状态码
        response.setStatus(302);

        // 设置响应头中 Location 属性
        response.setHeader("Location", "/web04/r2");
    }
```

特点：

- 使用请求重定向无法访问 `WEB-INF` 中的资源。

- 可以访问工程以外的资源。

## 二、书城项目第二阶段

实现用户注册与登录。

### 1、JavaEE 项目三层架构

![image-20221015180336782](img/image-20221015180336782.png)

- web层/视图展现层
- service 业务层
- Dao 持久层
  - Database Access Object

项目的包结构：

![image-20221015181508094](img/image-20221015181508094.png)

### 2、代码编写流程

#### (1) 创建所需数据库和表。

根据用户登录逻辑，使用到的表为 t_user。

t_user

- id 自增主键
- username not null unique
- password not null
- email 

#### (2) 编写数据库表对应的 JavaBean 对象

每一个数据库表都有一个类与之对应，放入 pojo 包中。

#### (3) 编写 Dao 持久层

需要编写工具类：JdbcUtils

## 三、补充知识：数据库连接池与德鲁伊

### 1、基本介绍

预先在数据库连接池中放入一定数量的连接，当需要建立连接时，只需要从连接池中取出一个，用完再放回去(并不是关闭连接)。

当应用程序向连接池请求的连接数超过最大连接数量，这些请求将被放入等待队列。

原理示意图：

![image-20221015210405169](img/image-20221015210405169.png)

JDBC 中的数据库连接池使用 `javax.sql.DataSource` 表示，接口由第三方实现。

### 2、德鲁伊的使用

```java
	public void druidTest() {
        Connection connection = null;
        // 1.加入配置文件

        // 2.创建 properties 对象读取配置文件
        Properties properties = new Properties();
        try {
            properties.load(new FileInputStream("src/jdbc.properties"));
            // 3.创建数据库连接池
            DataSource dataSource = DruidDataSourceFactory.createDataSource(properties);
            // 4.获取连接
            connection = dataSource.getConnection();
            System.out.println(connection);
        } catch (IOException e) {
            throw new RuntimeException(e);
        } catch (Exception e) {
            throw new RuntimeException(e);
        } finally {
            if (connection != null) {
                try {
                    // 关闭连接，不是真的关闭连接，而是放回连接池
                    connection.close();
                } catch (SQLException e) {
                    throw new RuntimeException(e);
                }
            }
        }
    }
```

### 3、德鲁伊工具类

```java
public class DruidUtils {
    // 数据库连接池
    private static DataSource ds;

    // 静态代码块
    // 在类加载时创建数据库连接池
    static {
        // 获取配置文件
        Properties properties = new Properties();
        try {
            properties.load(new FileInputStream("src/jdbc.properties"));

            // 创建数据库连接池
            ds = DruidDataSourceFactory.createDataSource(properties);
        } catch (IOException e) {
            throw new RuntimeException(e);
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }

    /**
     * 获取连接
     * @return 数据库连接池中的一个连接
     * @throws SQLException
     */
    public static Connection getConnection() throws SQLException {
        return ds.getConnection();
    }

    public static void close(ResultSet resultSet, Statement statement, Connection connection) throws SQLException {
        if (resultSet != null) {
            resultSet.close();
        }
        if (statement != null) {
            statement.close();
        }
        if (connection != null) {
            connection.close();
        }
    }
}
```

