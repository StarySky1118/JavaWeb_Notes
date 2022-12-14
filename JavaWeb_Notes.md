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

#### (1) 创建所需数据库和表

根据用户登录逻辑，使用到的表为 t_user。

t_user

- id 自增主键
- username not null unique
- password not null
- email 

#### (2) 编写数据库表对应的 JavaBean 对象

每一个数据库表都有一个类与之对应，放入 pojo 包中。

#### (3) 编写 Dao 持久层

需要编写工具类：JdbcUtils。

```java
public class DruidUtils {
    // 数据库连接池
    private static DataSource dataSource;

    // 类加载时创建数据库连接池
    static {
        // 获取配置文件中的信息
        Properties properties = new Properties();
        try {
            properties.load(new FileInputStream("src/main/resources/jdbc.properties"));

            // 创建连接池
            dataSource = DruidDataSourceFactory.createDataSource(properties);
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }

    /**
     * 获取连接
     * @return 数据库连接池中的一个连接
     */
    public static Connection getConnection() throws SQLException {
        return dataSource.getConnection();
    }

    /**
     * 归还连接
     * @param resultSet 查询结果集
     * @param statement 数据库操作对象
     * @param connection 数据库连接
     */
    public static void close(ResultSet resultSet, Statement statement, Connection connection) {
        if (resultSet != null) {
            try {
                resultSet.close();
            } catch (SQLException e) {
                throw new RuntimeException(e);
            }
        }

        if (statement != null) {
            try {
                statement.close();
            } catch (SQLException e) {
                throw new RuntimeException(e);
            }
        }

        if (connection != null) {
            try {
                connection.close();
            } catch (SQLException e) {
                throw new RuntimeException(e);
            }
        }
    }
}
```

#### (4) 编写 BaseDao

```java
public abstract class BasicDao<T> {
    // 查询执行器对象
    private final QueryRunner runner = new QueryRunner();

    /**
     * 执行更新操作
     *
     * @param sql        执行的sql语句
     * @param parameters 替换参数
     * @return 受影响的行数
     */
    public int update(String sql, Object...parameters) {
        int effectedRows; // 受影响行数
        Connection connection = null;

        try {
            // 获取连接
            connection = DruidUtils.getConnection();

            // 执行查询
            effectedRows = runner.update(connection, sql, parameters);
        } catch (SQLException e) {
            throw new RuntimeException(e);
        } finally {
            // 关闭连接
            DruidUtils.close(null, null, connection);
        }

        // 返回结果
        return effectedRows;
    }

    /**
     * 查询多行结果
     * @param sql sql语句
     * @param clazz 需要的JavaBean的Class对象
     * @param parameters 替换参数
     * @return 查询结果集
     */
    public List<T> queryMulti(String sql, Class<T> clazz, Object...parameters) {
        List<T> list; // 查询结果集
        Connection connection = null;

        try {
            // 获取连接
            connection = DruidUtils.getConnection();

            // 执行查询
            list = runner.query(sql, new BeanListHandler<>(clazz), parameters);
        } catch (SQLException e) {
            throw new RuntimeException(e);
        } finally {
            // 关闭连接
            DruidUtils.close(null, null, connection);
        }

        // 返回结果
        return list;
    }

    /**
     * 查询单个结果
     * @param sql 需要执行的sql查询语句
     * @param clazz JavaBean的Class对象
     * @param parameters 替换参数
     * @return 查询结果
     */
    public T querySingle(String sql, Class<T> clazz, Object...parameters) {
        T t; // 返回的 JavaBean 对象

        Connection connection = null;

        try {
            // 获取连接
            connection = DruidUtils.getConnection();

            // 执行查询
            t = runner.query(sql, new BeanHandler<>(clazz), parameters);
        } catch (SQLException e) {
            throw new RuntimeException(e);
        } finally {
            // 关闭连接
            DruidUtils.close(null, null, connection);
        }

        // 返回结果
        return t;
    }

    /**
     * 查询标量
     * @param sql 查询sql语句
     * @param parameters 替换参数
     * @return 查询变量值
     */
    public Object queryScalar(String sql, Object...parameters) {
        Object o; // 返回的标量

        Connection connection = null;

        try {
            // 获取连接
            connection = DruidUtils.getConnection();

            // 执行查询
            o = runner.query(sql, new ScalarHandler<>(), parameters);
        } catch (SQLException e) {
            throw new RuntimeException(e);
        } finally {
            // 关闭连接
            DruidUtils.close(null, null, connection);
        }

        // 返回结果
        return o;
    }
}
```

#### (5) 编写UserDao接口

```java
public interface UserDao {


    /**
     * 隶属于用户注册<br/>
     * 根据用户名查询用户信息
     * @param username 用户名
     * @return 用户信息
     */
    User queryUserByUsername(String username);

    /**
     * 隶属于用户注册
     * 保存用户信息到数据库
     * @param user 用户信息
     * @return 保存成功则返回1，否则返回0
     */
    int saveUser(User user);

    /**
     * 隶属于用户登录
     * @param username 用户名
     * @param password 密码
     * @return 用户信息
     */
    User queryUserByUsernameAndPassword(String username, String password);
}
```

#### (6) 编写 UserDaoImpl

```java
public class UserDaoImpl extends BasicDao<User> implements UserDao {
    @Override
    public User queryUserByUsername(String username) {
        // 查询语句
        String sql = "select * from t_user where username = ?";
        // 执行查询返回结果
        return super.querySingle(sql, User.class, username);
    }

    @Override
    public int saveUser(User user) {
        // 查询语句
        String sql = "insert into t_user (username, password, email) values (?, ?, ?)";

        // 执行查询返回结果
        return super.update(sql, user.getUsername(), user.getPassword(), user.getEmail());
    }

    @Override
    public User queryUserByUsernameAndPassword(String username, String password) {
        String sql = "select * from t_user where username = ? and password = ?";
        return super.querySingle(sql, User.class, username, password);
    }
}
```

Dao 编写完成后，需要进行测试。可以在接口类中，按快捷键 `Ctrl+Shift+T` 创建测试类。

![image-20221018074858237](img/image-20221018074858237.png)

#### (7) 编写UserService和测试

根据登录和注册功能，需要用到数据库的操作有：查询用户名是否存在、用户注册和用户登录。

```java
public interface UserService {
    /**
     * 判断用户名是否存在
     * @param username 用户名
     * @return 用户名存在返回True；不存在返回False。
     */
    boolean usernameExists(String username);

    /**
     * 用户注册
     * @param user 封装好的User对象
     */
    void userRegister(User user);

    /**
     * 用户登录
     * @param username 用户名
     * @param password 密码
     * @return 封装好的User对象
     */
    User login(String username, String password);
}
```

```java
public class UserServiceImpl implements UserService {
    private UserDao userDao = new UserDaoImpl();

    @Override
    public boolean usernameExists(String username) {
        // 如果可以查询到结果，说明用户名已经存在
        return userDao.queryUserByUsername(username) != null ? true : false;
    }

    @Override
    public void userRegister(User user) {
        userDao.saveUser(user);
    }

    @Override
    public User login(String username, String password) {
        return userDao.queryUserByUsernameAndPassword(username, password);
    }
}
```

#### (8) 实现用户注册功能

用户注册功能流程图如下：

<img src="img/注册功能.png" style="zoom:150%;" />

```java
protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    // 接收用户提交参数
    String username = request.getParameter("username");
    String password = request.getParameter("password");
    String email = request.getParameter("email");
    String code = request.getParameter("code");

    // 验证码是否正确 --> 先写死为abcde
    if ("abcde".equalsIgnoreCase(code)) { // 验证码正确
        // 查询用户名是否已经存在
        if (userService.usernameExists(username)) { // 用户名已存在
            // 跳回用户注册界面
            response.sendRedirect("/book/pages/user/regist.html");
        } else { // 不存在
            // 将获取的参数封装成为User，存入数据库
            User user = new User(null, username, password, email);
            userService.userRegister(user);

            // 跳转至注册成功页面
            response.sendRedirect("/book/pages/user/regist_success.html");
        }
    } else { // 验证码错误
        // 跳转至注册页面
        response.sendRedirect("/book/pages/user/regist.html");
    }
}
```

JavaWeb 阶段，使用 Base + 相对路径的方式进行寻址。

注册功能可能出现的异常：`java.lang.NoClassDefFoundError: Could not initialize class com.zzy.utils.DruidUtils`

产生这种异常的原因是：类加载失败。`DruidUtils` 中如果 `static` 代码块中出现异常，就会导致类加载失败。

```java
static {
    // 获取配置文件中的信息
    Properties properties = new Properties();
    try {
        properties.load(new FileInputStream("src/main/resources/jdbc.properties"));

        // 创建连接池
        dataSource = DruidDataSourceFactory.createDataSource(properties);
    } catch (Exception e) {
        throw new RuntimeException(e);
    }
}
```

将代码部署到 Tomcat 上之后，路径 `src/main/resources/jdbc.properties` 便无法找到配置文件，出现异常。

解决方案：使用绝对路径。

![image-20221018130722803](img/image-20221018130722803.png)

复制配置文件的绝对路径，并使用绝对路径寻找配置文件。

#### (9) 用户登录功能

整个流程：

![image-20221018134010894](img/image-20221018134010894.png)

`LoginServlet` 流程

```
1、获取输入参数
2、调用 UserService Login()方法
3、根据返回值判断登录情况
	成功
		跳转至登录成功页面
	失败
		回到登录页面
```

```java
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 1、获取输入参数
        String username = request.getParameter("username");
        String password = request.getParameter("password");

        // 2、调用 UserService Login()方法
        if (userService.login(username, password) != null) { // 登录成功
            // 跳转至登录成功页面
            response.sendRedirect("/book/pages/user/login_success.html");
        } else { // 登录失败
            // 回到登录页面
            response.sendRedirect("/book/pages/user/login.html");
        }
    }
```

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

德鲁伊配置文件：

```properties
username=root
password=991118
url=jdbc:mysql://localhost:3306/book
driverClassName=com.mysql.cj.jdbc.Driver
initialSize=5
maxActive=10
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

# 20221016

## 一、补充知识：BasicDao

### 1、Apache-DBUtils 引入

上面我们封装了 `DruidUtils` 中的 `close()` 方法，即一旦连接关闭，查询结果集 `resultSet` 也就不能使用了。同时，`resultSet` 中的方法也比较死板。

想法是将查询结果在 Java 程序中进行相应的建模 ，这个建模便被称为 JavaBean/Pojo/Domain。将查询结果集转换为相应建模的列表。

例如：查询结果如下

![image-20221016184443087](img/image-20221016184443087.png)

可以进行如下建模：

注意：建模时要使用包装类，因为只有包装类的值才能为 `null`。

```java
public class Dept {
    private Integer deptNo; // 部门编号
    private String dName; // 部门名称
    private String loc; // 位置
}
```

### 2、Apache-DBUtils

`commons-dbutils` 是 Apache 提供的一个开源 JDBC 工具类库。

使用到的 API：

`QueryRunner` 中的 `query()` 方法。

![image-20221016192426803](img/image-20221016192426803.png)

`BeanListHander` 类：可以为我们生成 BeanList。

```java
queryRunner.query(connection, sql, new BeanListHandler<>(Dept.class), 10);
```

`BeanHandler` 类：可以为我们生成单个 Bean 对象。例如，登录。

```java
queryRunner.query(connection, sql, new BeanHandler<>(Dept.class), 10);
```

`ScalarHandler` 类：可以为我们生成单行单列的标量对象。

```java
queryRunner.query(connection, sql, new ScalarHandler<>(), 1);
```

#### (1) 使用 `DBUtils` 进行查询

```java
public void testQueryMany() {
        Connection connection = null;

        try {
            // 获取连接
            connection = DruidUtils.getConnection();

            // 创建 QueryRunner
            QueryRunner queryRunner = new QueryRunner();

            // 执行查询
            String sql = "select * from dept where deptno = ?";
            List<Dept> list = queryRunner.query(connection, sql, new BeanListHandler<>(Dept.class), 10);

            // 输出
            System.out.println(list);
        } catch (SQLException e) {
            throw new RuntimeException(e);
        } finally {
            // 关闭连接
            DruidUtils.close(null, null, connection);
        }
    }
```

`Query()` 核心源码如下：

```java
PreparedStatement stmt = null;
ResultSet rs = null;
T result = null;

try {
    stmt = this.prepareStatement(conn, sql);
    this.fillStatement(stmt, params);
    rs = this.wrap(stmt.executeQuery());
    result = rsh.handle(rs);
} catch (SQLException var33) {
    this.rethrow(var33, sql, params);
} finally {
    try {
        // 关闭结果集
        this.close(rs);
    } finally {
        // 关闭数据库操作对象
        this.close(stmt);
        if (closeConn) {
            this.close(conn);
        }

    }
}

return result;
```

#### (2) 使用 `DBUtils` 进行 DML 操作

使用到的 API：

![image-20221016201042495](img/image-20221016201042495.png)

### 3、BasicDao

>  为什么需要 BasicDao？DBUtils + Druid 用起来不是很爽吗？
>
> DBUtils + Druid 本身的使用没有问题，但对各个表进行操作时仍然需要进行区分，使用不同的 JavaBean，此即特有 Dao，特有 Dao 中也有公共部分，将公共部分提取出来就是 BasicDao。

```java
public class BasicDao<T> {
    private QueryRunner queryRunner = new QueryRunner();

    /**
     * 通用 DML
     * @param sql 可替换 sql 语句
     * @param parameters 替换参数
     * @return 受影响的行数
     */
    public int update(String sql, Object...parameters) {
        Connection connection = null;
        int effectedRows = 0;

        try {
            // 获取连接
            connection = DruidUtils.getConnection();

            // 执行查询
            effectedRows = queryRunner.update(connection, sql, parameters);
        } catch (SQLException e) {
            throw new RuntimeException(e);
        } finally {
            // 关闭连接
            DruidUtils.close(null, null, connection);
        }

        return effectedRows;
    }

    /**
     * 查询多行结果
     * @param sql 可替换参数的 sql 语句
     * @param clazz 相应 Bean 的 Class 对象
     * @param parameters 替换参数
     * @return 结果集合
     */
    public List<T> queryMulti(String sql, Class<T> clazz, Object...parameters) {
        Connection connection = null;
        List<T> list;

        try {
            // 获取连接
            connection = DruidUtils.getConnection();

            // 执行查询
            list = queryRunner.query(connection, sql, new BeanListHandler<>(clazz), parameters);
        } catch (SQLException e) {
            throw new RuntimeException(e);
        } finally {
            // 关闭连接
            DruidUtils.close(null, null, connection);
        }

        return list;
    }

    /**
     * 查询单行结果
     * @param sql sql 语句
     * @param clazz JavaBean 的 Class 对象
     * @param parameters sql 中的替换参数
     * @return 封装好的单行结果
     */
    public T querySingle(String sql, Class<T> clazz, Object... parameters) {
        Connection connection = null;
        T t;

        try {
            // 获取连接
            connection = DruidUtils.getConnection();

            // 执行查询
            t = queryRunner.query(connection, sql, new BeanHandler<>(clazz), parameters);
        } catch (SQLException e) {
            throw new RuntimeException(e);
        } finally {
            // 关闭连接
            DruidUtils.close(null, null, connection);
        }

        return t;
    }

    /**
     * 查询标量
     * @param sql sql 语句
     * @param parameters 替换参数
     * @return 标量
     */
    public Object queryScalar(String sql, Object...parameters) {
        Connection connection = null;
        Object o;

        try {
            // 获取连接
            connection = DruidUtils.getConnection();

            // 执行查询
            o = queryRunner.query(connection, sql, new ScalarHandler<>(), parameters);
        } catch (SQLException e) {
            throw new RuntimeException(e);
        } finally {
            // 关闭连接
            DruidUtils.close(null, null, connection);
        }

        return o;
    }
}
```

### 4、特有 Dao

特有 Dao 需要继承 BasicDao，并且可能会有独立的方法。

# 20221018

## 一、断点调试

![image-20221018133242573](img/image-20221018133242573.png)

## 二、JSP

### 1、作用

全称 Java Server Pages，作用是代替 Servlet 程序回传 html 页面的数据。使用 Servlet 输出 html 页面太麻烦了。

![image-20221018165259093](img/image-20221018165259093.png)

### 2、JSP 页面的本质

jsp 页面本质上是 Servlet 程序。

当我们第一次访问 jsp 页面，Tomcat 会将其翻译为 java 源文件，并将其编译。

### 3、JSP 的 page 指令

头部 page 指令：

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
```

可以修改 jsp 页面中一些重要的属性或行为。

![image-20221018172055934](img/image-20221018172055934.png)

具体可以查看：http://c.biancheng.net/jsp2/page.html

### 4、JSP 常用脚本

#### (1) 声明脚本

格式：

```jsp
<%! 声明java代码 %>
```

作用：给 jsp 翻译出来的 java 类定义属性与方法、甚至是静态代码块。

```jsp
    <!--声明类属性-->
    <%!
        private Integer id;
        private static Map<String, Object> map;
    %>

    <%--定义代码块--%>
    <%!
        static {
            map = new HashMap<String, Object>();
            map.put("key1", "value1");
        }
    %>

    <%--声明类方法--%>
    <%!
        public int abc() {
            return 12;
        }
    %>

    <%--声明内部类--%>
    <%!
        public static class A {
            
        }
    %>
```

#### (2) 表达式脚本(常用)

格式：

```jsp
<%=表达式%>
```

作用：在 jsp 页面上输出数据。

```jsp
<%=12%>
```

特点：

- 所有表达式脚本都被翻译到 `_jspService()` 中 `out.print()`
- `_jspService` 中的对象都可以直接使用，例如 `request`

#### (3) 代码脚本

格式：

```jsp
<%
	代码脚本
%>
```

作用：在 jsp 页面中编写自己需要的功能。

if、for和在 `_jspService()` 方法中可以写的代码。

特点：

- 代码脚本翻译后在 `_jspService()` 方法中。
- 可以使用多个代码脚本组成完整的 java 语句。
- 可以和表达式脚本组合。

#### (4) jsp 中三种注释

html 注释：

```jsp
<!--html注释-->
```

html 注释会被翻译到 java 源代码中，输出到客户端。

java 注释：写在代码脚本和表达式脚本中。

jsp 注释：

```jsp
<%--jsp注释-->
```

### 5、JSP 中九大内置对象

jsp 中的内置对象：Tomcat 翻译 jsp 成为 java 源代码后，内部提供的九大对象。

`request` 请求对象：

`response` 响应对象：

`pageContext` jsp 上下文对象：

`session` 会话对象：

`applicatoin` servletContext 对象：

`config` servletConfig 对象：

`out` jsp 输出流对象：

`page` 指向当前 jsp 对象：

`exception` 异常对象：

### 6、四大域对象

域对象：可以向 Map 一样存取数据。

`pageContext`：当前 jsp 页面内有效。

`request` ：一次请求有效。

`session` ：一次会话范围内有效。

`application`：整个 web 工程范围内有效。

# 20221019

## 一、JSP

### 6、四大域对象

```jsp
<%
	request.setAttribute("key1", "value1");
%>
```

使用上有优先顺序：先使用范围小的。

### 7、jsp 中 out 输出和 response.getWriter() 输出的区别

![image-20221019100122105](img/image-20221019100122105.png)

jsp 翻译后底层源代码都使用 out 进行输出，因此在 jsp 页面中统一使用 out 输出，避免打乱页面输出顺序。

`out.write()` 和 `out.print()`：

`print()` 底层会将任何数据转换为字符串进行输出。

浅显结论：统一使用 `print()` 进行输出。

### 8、JSP 常用标签

#### (1) 静态包含

实现如下的需求：

![image-20221019101345763](img/image-20221019101345763.png)

使用案例：

```jsp
<body>
    头部信息<br>
    主体信息<br>
    <%--这就是静态包含标签--%>
    <%@include file="/include/foot.jsp"%>
</body>
```

如果需要修改页脚信息，去 `webapp/include/foot.jsp` 中统一进行修改即可。

静态包含特点：

- 不会翻译被包含的 jsp 页面
- 实际上是将被包含的 jsp 页面代码拷贝到相应位置进行输出

#### (2) 动态包含

```jsp
<%--这就是 jsp 动态包含--%>
<jsp:include page="foot.jsp">
    <%--向包含页面传递属性--%>
    <jsp:param name="password" value="123456"/>
</jsp:include>
```

特点：

- 会将包含的 jsp 也翻译为 java 代码
- 使用代码调用页面，不是直接拷贝到相应位置
- 可以向包含页面传递属性

原理：

![image-20221019103024409](img/image-20221019103024409.png)

#### (3) 请求转发标签

```jsp
<%--这就是请求转发标签--%>
<jsp:forward page="/include/foot.jsp"></jsp:forward>
```

请求转发的流程：

![image-20221019111913748](img/image-20221019111913748.png)

## 二、Listener 监听器

是 JavaWeb 三大组件之一。Listener 是 JavaEE 的规范。

作用是监听某种事物的变化，通过回调函数，反馈给程序做相应的处理。

### 1、ServletContextListener 监听器

可以监听 ServletContext 对象的创建和销毁。ServletContext 对象在 web 工程启动的时候创建，在 web 工程停止时销毁。

只有两个重要的方法：

![image-20221019113842397](img/image-20221019113842397.png)

使用方法：

编写类实现 ServletContextListener --> 实现两个回调函数 --> web.xml 中进行配置监听器

![image-20221019114623875](img/image-20221019114623875.png)

```java
public class MyListener implements ServletContextListener, HttpSessionListener, HttpSessionAttributeListener {

    public MyListener() {
    }

    @Override
    public void contextInitialized(ServletContextEvent sce) {
        /* This method is called when the servlet context is initialized(when the Web application is deployed). */
        System.out.println("初始化ServletContext对象");
    }

    @Override
    public void contextDestroyed(ServletContextEvent sce) {
        /* This method is called when the servlet Context is undeployed or Application Server shuts down. */
        System.out.println("销毁ServletContext对象");
}
```

`web.xml` 中进行配置：

```xml
<listener>
	<listener-class>ServletContextListener_.MyListener</listener-class>
</listener>
```

# 20221020

## 一、EL 表达式和 JSTL 标签库

EL 表达式全称 Expression Language，表达式语言。主要是替代 jsp 页面的表达式脚本在 jsp 页面进行数据的输出，使表达更加简洁。

```jsp
<%--使用表达式脚本输出数据--%>
<%=request.getAttribute("key")%>

<%--使用EL表达式输出数据--%>
${key}
```

EL 表达式在输出 null 时，输出的是空串。

### 1、EL 表达式搜索域的顺序

EL 表达式主要输出的是域对象中的数据，其搜索顺序为从小到大：

`pageContext`、`request`、`session`、`application`

### 2、使用 EL 表达式输出复杂 Bean 对象

```jsp
    <%
        Person person = new Person();
        person.setName("田所浩二");
        Map<String, Object> map = new HashMap<>();
        map.put("语文", 1919);
        map.put("数学", 114514);
        person.setMap(map);

        session.setAttribute("Person", person);
    %>
    <%--输出 person--%>
    ${Person}<br>
    <%--输出person的name属性--%>
    ${Person.name}<br>
    <%--输出person的map中某个key的值--%>
    ${Person.map.语文}
    ${Person.map.数学}
```

上面所有 `.属性` 实际上是去找 get 方法，有 get 方法才会被视为一个属性。

### 3、EL 表达式运算

#### (1) 关系运算

![image-20221020094222491](img/image-20221020094222491.png)

使用案例：

```jsp
${5 == 5}
```

#### (2) 逻辑运算

![image-20221020094748175](img/image-20221020094748175.png)

#### (3) 算术运算

![image-20221020095323791](img/image-20221020095323791.png)

#### (4) empty 运算

判断一个数据是否为空，如果为空返回 `true`。

何时数据为空？

值为 `null`、值为空串、长度为 0 的数组、list 集合元素个数为 0 、map 集合元素个数为 0。

使用案例：

```jsp
<%
	session.setAttribute("name", null);
%>
${empty name}
```

#### (4) 三元运算

表达式1 ? 表达式2 : 表达式3

#### (5) . 运算和[] 运算

. 可以输出 Bean 对象中某个属性的值。

[] 可以输出有序集合中某个元素的值。或是输出 map 中某个 key 的 value。

```jsp
${map["name"]}
```

#### (6) EL 表达式11个隐藏对象

| 变量               | 类型                    | 作用                                    |
| ------------------ | ----------------------- | --------------------------------------- |
| `pageContext`      | `pageContextImpl`       | 获取 jsp 中九大内置对象                 |
| `pageScope`        | `Map<String, Object>`   | 获取 `pageContext` 域中的数据           |
| `requestScope`     | `Map<String, Object>`   | 获取 `request` 域中的数据               |
| `sessionScope`     | `Map<String, Object>`   | 获取 `session` 域中的数据               |
| `applicationScope` | `Map<String, Object>`   | 获取 `servletContext` 域中的数据        |
| `param`            | `Map<String, String>`   | 获取请求参数的值                        |
| `paramValues`      | `Map<String, String[]>` | 获取多个请求参数值                      |
| `header`           | `Map<String, String>`   | 获取请求头                              |
| `headerValues`     | `Map<String, String>`   | 获取请求头多个值                        |
| `cookie`           | `Map<String, Cookie>`   | 获取当前请求 Cookie 信息                |
| `initParam`        | `Map<String, String>`   | 获取 `web.xml` 配置的` <Context-param>` |

##### a. 四个域的使用

使用案例：

```jsp
<%
	request.setAttribute("key1", "value1");
%>
${requestScope.key1}
```

> 如果出现 `pageContext` 没有提示的情况，可以导入 Tomcat JSP API 依赖。
>
> ![image-20221020113954735](img/image-20221020113954735.png)

##### b. pageContext 的使用

使用案例：

```jsp
协议为：${pageContext.request.scheme}<br>
服务器ip为：${pageContext.request.serverName}<br>
服务器端口号为：${pageContext.request.serverPort}<br>
工程路径：${pageContext.request.contextPath}<br>
请求方法：${pageContext.request.method}<br>
客户端ip为：${pageContext.request.remoteHost}<br>
会话ID为：${pageContext.session.id}<br>
```

##### c. `param` 和 `paramValues`

```jsp
${param.username}
```

##### d. `header` 和 `headerValues`

```jsp
${header["User-Agent"]}
```

##### e. `cookie`

```
${cookie.JSESSIONID.name}
```

# 20221021

## 一、JSTL 标签库

### 1、介绍

JSTL(JSP Standard Tag libraty) 标签库，即 JSP 标准标签库。EL 表达式为了替换表达式脚本，JSTL 是为了替换代码脚本。

![image-20221021205200096](img/JSTL 标签库.png)

在使用前，还要使用 taglib 指令引入标签库。这个过程 IDEA 会自动帮我们完成。

### 2、标签库的使用

首先需要引入依赖：

```xml
<!-- https://mvnrepository.com/artifact/org.apache.taglibs/taglibs-standard-spec -->
<dependency>
    <groupId>org.apache.taglibs</groupId>
    <artifactId>taglibs-standard-spec</artifactId>
    <version>1.2.5</version>
</dependency>

<!-- https://mvnrepository.com/artifact/org.apache.taglibs/taglibs-standard-impl -->
<dependency>
    <groupId>org.apache.taglibs</groupId>
    <artifactId>taglibs-standard-impl</artifactId>
    <version>1.2.5</version>
    <scope>runtime</scope>
</dependency>
```

### 3、Core 核心库的使用

#### (1) set 标签

作用：向域中保存数据。

使用案例：

```jsp
<c:set scope="page" var="fuck" value="yes"/>
```

- scope 域的选择
- var 键
- value 值

#### (2) if 标签

作用：做 if 判断。

使用案例：

```jsp
<c:if test="${12 == 12}">
	<h1>holy shit</h1>
</c:if>
```

标签头使用 test 属性进行判断，test 属性内填入 EL 表达式。标签内是 if 成立会执行的语句。

#### (3) choose、when 和 otherwise 标签

作用：多路判断。相当于 switch。

使用案例：

```jsp
    <%--设置 pageContext 域中 amount 的值--%>
    <c:set scope="page" var="amount" value="15"></c:set>

    <c:choose>
        <c:when test="${pageScope.amount < 5}">
            <h2>行不行啊细狗</h2>
        </c:when>
        <c:when test="${pageScope.amount > 10}">
            <h2>真的牛啊</h2>
        </c:when>
        <c:otherwise>
            <h2>呵呵</h2>
        </c:otherwise>
    </c:choose>
```

使用说明：

- choose 表示开始多路判断，进行选择
- when 表示当什么时候
- otherwise 表示其他情况

#### (4) forEach 标签

作用：遍历输出。

使用案例1：遍历 1-10 输出。

```jsp
    <c:forEach begin="1" end="10" var="i">
        ${i}
    </c:forEach>
```

使用说明：

- begin：开始
- end：结束
- var：循环变量
- step：步长

使用案例2：遍历 Object 数组。

```jsp
    <%
        request.setAttribute("arr", new String[] {"hello", "world", "thing"});
    %>
    <c:forEach items="${requestScope.arr}" var="item">
        ${item}
    </c:forEach>
```

使用说明：

- items：数据源

使用案例3：遍历 Map 集合

```jsp
    <%
        Map<String, String> map = new HashMap<>();
        map.put("key1", "value1");
        map.put("key2", "value2");
        map.put("key3", "value3");
        request.setAttribute("map", map);
    %>
    <c:forEach items="${requestScope.map}" var="entry">
        <h1>${entry}</h1>
        <h2>${entry.key}</h2>
        <h2>${entry.value}</h2>
    </c:forEach>
```

使用说明：

- var 是循环变量，遍历 map 得到的是 entry(字典中的项目)。

# 20221022

## 一、JSTL 标签库

### 3、Core 核心库的使用

#### (5) forEach

forEach 上面的各种参数都可以在合适时组合使用。

- varStatus：当前遍历到数据的状态。这实际上是一个类，实现了如下方法：

  ![image-20221022085307743](img/image-20221022085307743.png)

  每个都是遍历到数据的属性。

## 二、文件上传

### 1、文件上传介绍

流程：

- form 标签，post 方法 // post 没有长度限制
- form 标签的 `encType` 属性值必须为 `multipart/form-data`  // 多段(一个表单项一个数据段)拼接，以二进制流的方式发送给服务器 --》 服务器也必须以流的方式接收
- form 标签内部使用 `<input type="file"></input>`
- 服务器代码(Servlet)编写

使用案例：

web 层

```jsp
    <form action="/JSPTest01/hello01" method="post" enctype="multipart/form-data" >
        用户名<input type="text" name="username"><br>
        头像<input type="file" name="headImg"><br>
        <input type="submit" value="提交"><br>
    </form>
```

servlet 层

### 2、上传的 HTTP 协议介绍

![image-20221022091614040](img/上传HTTP.png)

### 3、上传字节流的处理

很多第三方实现了客户端字节流处理，可以引入依赖使用写好的方法。

![image-20221022093544009](img/Apache-Commons FileUpload.png)

所需依赖：

```xml
<!-- https://mvnrepository.com/artifact/commons-fileupload/commons-fileupload -->
<dependency>
    <groupId>commons-fileupload</groupId>
    <artifactId>commons-fileupload</artifactId>
    <version>1.3.3</version>
</dependency>
```

使用 API：

![image-20221022094231279](img/uploadAPI.png)

![image-20221022094426487](img/image-20221022094426487.png)

使用案例：

```java
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 判断上传的是否为多段数据
        if (ServletFileUpload.isMultipartContent(request)) {
            // FileItemFactory 可以设置缓存位置与大小，下面代码使用默认大小与默认位置
            // 默认缓存大小是  10240(10k).
			// 临时文件默认存储在系统的临时文件目录下.
            FileItemFactory fileItemFactory = new DiskFileItemFactory();
            // 创建 ServletFileUpload 实例，用于解析数据
            ServletFileUpload servletFileUpload = new ServletFileUpload(fileItemFactory);
            // 解析数据
            try {
                List<FileItem> fileItems = servletFileUpload.parseRequest(request);

                for (FileItem fileItem : fileItems) {
                    // 判断是普通表单项还是上传的数据
                    if (fileItem.isFormField()) { // 普通表单项
                        System.out.println("普通表单项的name属性值：" + fileItem.getFieldName());
                        System.out.println("普通表单项的value属性值："+ fileItem.getString("UTF-8"));
                    } else { // 上传文件
                        System.out.println("上传文件项的name属性值：" + fileItem.getFieldName());
                        System.out.println("上传的文件名：" + fileItem.getName());

                        fileItem.write(new File("z:/" + fileItem.getName()));
                        System.out.println("文件保存成功！");
                    }
                }
            } catch (FileUploadException e) {
                throw new RuntimeException(e);
            } catch (Exception e) {
                throw new RuntimeException(e);
            }
        }
    }
```

> 中文乱码 bug 还没有解决。

## 三、文件下载

### 1、总体流程

![image-20221022102653784](img/下载流程.png)

服务器程序：

> 需要注意：响应头的内容需要提前设置。

```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 获取要下载的文件名
        String downloadFileName = "lappland.jpg";

        // 读取要下载的内容
        ServletContext servletContext = getServletContext();
        InputStream resourceAsStream = servletContext.getResourceAsStream("/files/" + downloadFileName);

        // 告知客户端要回传的数据类型
        String mimeType = servletContext.getMimeType("/files/" + downloadFileName);
        System.out.println("下载的文件类型为：" + mimeType);
        response.setContentType(mimeType);

        // 告知客户端提供的数据适用于下载
        // Content-Disposition 内容的处理方式
        // attachment 以附件的方式处理
        // filename 下载的文件名
        response.setHeader("Content-Disposition", "attachment;filename=22.jpg");

        // 获取响应的输出流
        ServletOutputStream outputStream = response.getOutputStream();

        // 流内容复制
        IOUtils.copy(resourceAsStream, outputStream);
    }
```

### 2、文件附件中文显示问题

服务器程序存在的问题：无法识别中文。

谷歌浏览器：可以先使用 `URLEncocder` 类对含有中文的字符串进行 UTF -8 编码，浏览器会以 UTF-8 方式进行解码。

解决方案：

```java
response.setHeader("Content-Disposition", "attachment;filename=" + URLEncoder.encode(downloadFileName, "UTF-8"));
```

火狐浏览器：可以使用 Base64 方式对中文进行编码，火狐浏览器会以 Base64 方式进行解码。

使用方式：

`Content-Disposition: attachment; filename==?charset?B?xxxxx?=`

![image-20221022114847740](img/image-20221022114847740.png)

使用案例：

```java
response.setHeader("Content-Disposition", "attachment;filename==?UTF-8?B?" + new BASE64Encoder().encode(downloadFileName.getBytes(StandardCharsets.UTF_8)) + "?=");
```

综上所述，可以根据浏览器型号进行响应的编解码：

```java
        if (request.getHeader("User-Agent").contains("FireFox")) { // 使用火狐浏览器
            // 使用 Base64 编码
            response.setHeader("Content-Disposition", "attachment;filename==?UTF-8?B?" + new BASE64Encoder().encode(downloadFileName.getBytes(StandardCharsets.UTF_8)) + "?=");
        } else { // 其他浏览器
            // 使用URL编码
            response.setHeader("Content-Disposition", "attachment;filename=" + URLEncoder.encode(downloadFileName, "UTF-8"));
        }
```

# 20221023

## 一、书城项目第三阶段

### 1、页面 jsp 动态化

将所有的 html 页面转换为 jsp 页面。

- 在页面开始添加头部 page 指令：

  ```jsp
  <%@ page contentType="text/html;charset=UTF-8" language="java" %>
  ```

- 更改后缀名
- 如果出现重命名后未进行 refactor，还需要进行批量替换

> 批量替换操作：Ctrl + Shift + R
>
> ![image-20221023081952169](img/批量替换.png)

### 2、抽取页面中相同的内容

抽取各个页面中公共的部分，写成 jsp 页面，进行静态包含即可。

#### (1) 韩总部分

以下代码在四个页面中出现：

```HTML
    <div>
      <span>欢迎<span class="um_span">韩总</span>光临尚硅谷书城</span>
      <a href="../order/order.jsp">我的订单</a>
      <a href="../../index.jsp">注销</a>&nbsp;&nbsp;
      <a href="../../index.jsp">返回</a>
    </div>
```

将代码写入一个 jsp 页面中，使用如下代码进行静态包含：

```jsp
<%@include file="/pages/common/action_success.jsp"%>
```

#### (2) 相对路径 base、jQuery、CSS样式

注意：jsp 页面归根是 html 页面，由浏览器解析，因此 / 只会翻译到端口号。

`base.jsp` 应该这样写：

```jsp
<base href="/book/">
<link type="text/css" rel="stylesheet" href="static/css/style.css" >
<script type="text/javascript" src="static/script/jquery-1.7.2.js"></script>
```

使用 base 标签，link 标签和 script 标签中的 href 就不能以 / 开始了，为了让地址连起来，在 base 标签中的 href  后添加一个 /。

#### (3) 页脚的抽取

将页脚抽取，其余页面静态包含。

#### (4) 抽取 manager 页面中的相同部分

将以下代码抽离：

```jsp
<div>
    <a href="pages/manager/book_manager.jsp">图书管理</a>
    <a href="pages/manager/order_manager.jsp">订单管理</a>
    <a href="index.jsp">返回商城</a>
</div>
```

### 3、动态 base 标签

/ 会解析到工程路径，但一旦代码放到服务器上，浏览器仍解析 / 成为 localhost:8080 就不对了，应该使用动态标签，根据在地址栏输入的请求生成 base 。

生成动态 base 路径

```jsp
    <%
        String basePath = request.getScheme()
                + "://"
                + request.getServerName()
                + ":"
                + request.getServerPort()
                + request.getContextPath()
                + "/";
    %>
    <base href="<%=basePath%>">
```

# 20221024

## 二、书城项目第三阶段

### 1、登录表单提交错误回显

![image-20221024085041759](img/image-20221024085041759.png)

登录失败时，使用请求转发方式回到 `Login.jsp`

```java
// 请求转发回到登录页面
request.getRequestDispatcher("/pages/user/login.jsp").forward(request, response);
```

因此，需要回显的信息可以放到 request 域中。

#### (1) 提示信息

在 `login.jsp` 中：

```jsp
<span class="errorMsg">
	${requestScope.msg == null ? "请输入用户名和密码" : requestScope.msg}
</span>
```

#### (2) 用户名信息回写

在 `login.jsp` 中，根据 `requestScope.username` 是否为 `null` 决定是否写上默认数据。 

```jsp
<input class="itxt" type="text" placeholder="请输入用户名" autocomplete="off" tabindex="1" name="username" value="${empty requestScope.username ? "" : requestScope.username}"/>
```

### 2、注册表单错误信息回显

错误信息包括：用户名不能重复、验证码错误。

验证码错误需要回显的信息为：用户名、邮箱

用户名重复需要回显的信息：邮箱

**验证码错误**

servlet 程序：

```java
// 错误信息
request.setAttribute("msg", "验证码错误！");

// 需要回显的信息
request.setAttribute("username", username);
request.setAttribute("email", email);

// 请求转发方式跳转至注册页面
request.getRequestDispatcher("/pages/user/regist.jsp").forward(request, response);
```

前端：

错误信息显示：

```jsp
<span class="errorMsg">
	${empty requestScope.msg ? "" : requestScope.msg}
</span>
```

用户名回显：

```jsp
<input class="itxt" type="text" placeholder="请输入用户名" autocomplete="off" tabindex="1" name="username" id="username" value="${empty requestScope.username ? "" : requestScope.username}"/>
```

邮箱回显也是类似。

**用户名错误**

只需要更改，Servlet 程序即可。

### 3、代码优化——将注册 Servlet 和登录 Servlet 合并为一个 用户 Servlet

![image-20221024094525757](img/Servlet合并.png)

思路：使用隐藏表单项 hidden 的值，标识请求来向。

在 `login.jsp` 和 `regist.jsp` 中添加 hidden 表单项：

```html
<input type="hidden" name="action" value="regist">
```

添加 `UserServlet` 程序，能够根据 `action` 值的不同进行处理。

改变 `login.jsp` 和 `regist.jsp` 中的请求方向。

```html
<form action="userServlet" method="post">
```

将原来 `LoginServlet` 和 `RegistServlet` 中的代码复制到 `UserServlet` 中。根据需要将重复的代码写成方法。

将 `LoginServlet` 和 `RegistServlet` 删除，注意 `web.xml` 中也要删除。

### 4、代码优化——使用反射替代 if...else...

使用隐藏域提交参数，在 Servlet 程序中进行分支会导致大量的 if else。

![image-20221024102131771](img/存在问题.png)

优化方案：使用反射直接调用方法。

使用方法：

```java
public class UserServletTest {
    public void login() {
        System.out.println("登录方法...");
    }

    public void regist() {
        System.out.println("注册方法执行...");
    }

    public void updatePassword() {
        System.out.println("更改密码...");
    }

    public static void main(String[] args) {
        String action = "login";
        try {
            Method method = UserServletTest.class.getDeclaredMethod(action);
            method.invoke(new UserServletTest());
        } catch (NoSuchMethodException e) {
            throw new RuntimeException(e);
        } catch (InvocationTargetException e) {
            throw new RuntimeException(e);
        } catch (IllegalAccessException e) {
            throw new RuntimeException(e);
        }
    }
}
```

具体使用：

```java
protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 根据 request 中 action 参数的值决定分支
        String action = request.getParameter("action");
        System.out.println(action);

        try {
            Method method = this.getClass().getDeclaredMethod(action, HttpServletRequest.class, HttpServletResponse.class);
            method.invoke(this, request, response);
        } catch (NoSuchMethodException e) {
            throw new RuntimeException(e);
        } catch (InvocationTargetException e) {
            throw new RuntimeException(e);
        } catch (IllegalAccessException e) {
            throw new RuntimeException(e);
        }
    }
```

### 5、代码优化——抽取 BaseServlet

![image-20221024111352837](img/image-20221024111352837.png)

各种 Servlet 程序共有的逻辑：获取 hidden 隐藏域的 action，使用反射根据 action 调用方法。

抽取的 BaseServlet 如下：

```java
protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 根据 request 中 action 参数的值决定分支
        String action = req.getParameter("action");
        System.out.println(action);

        try {
            Method method = this.getClass().getDeclaredMethod(action, HttpServletRequest.class, HttpServletResponse.class);
            method.invoke(this, req, resp);
        } catch (NoSuchMethodException e) {
            throw new RuntimeException(e);
        } catch (InvocationTargetException e) {
            throw new RuntimeException(e);
        } catch (IllegalAccessException e) {
            throw new RuntimeException(e);
        }
    }
```

`UserServlet` 接收到请求后，会自动调用父类 `BaseServlet` 中的 `doPost()` 方法，实际运行类型还是 `UserServlet` 。

可能会出现的问题：如果反射获取的是子类中的 `private` 方法，将无法获取与调用，将权限改为 `protected` 是一个比较好的选择。

### 6、BeanUtils 的使用

优化背景：用户注册表单会提交大量数据，如果在 Servlet 程序中使用 `getParameter()` 一个一个获取，再封装到 Bean 对象中，有些麻烦。

```java
// 接收用户提交参数
String username = request.getParameter("username");
String password = request.getParameter("password");
String email = request.getParameter("email");
String code = request.getParameter("code");

// 封装
User user = new User(null, username, password, email);
```

BeanUtils 是第三方工具类。

使用到的依赖：

```xml
<!-- https://mvnrepository.com/artifact/commons-beanutils/commons-beanutils -->
<dependency>
    <groupId>commons-beanutils</groupId>
    <artifactId>commons-beanutils</artifactId>
    <version>1.9.3</version>
</dependency>
```

使用到的 API：

```java
BeanUtils.populate(user, request.getParameterMap());
```

使用案例：

在 `regist.jsp` 中，使用上述方法封装一个 User 对象。

```java
User user = new User();
try {
    BeanUtils.populate(user, request.getParameterMap());
} catch (IllegalAccessException e) {
    throw new RuntimeException(e);
} catch (InvocationTargetException e) {
    throw new RuntimeException(e);
}
```

将上述代码提取，进行复用：

```java
public class WebUtils {

    /**
     * 
     * @param request Http 请求
     * @param bean bean 对象
     */
    public static void copyParamToBean(HttpServletRequest request, Object bean) {
        
        try {
            BeanUtils.populate(bean, request.getParameterMap());
        } catch (IllegalAccessException e) {
            throw new RuntimeException(e);
        } catch (InvocationTargetException e) {
            throw new RuntimeException(e);
        }
    }
    
}
```

> 注入时调用标准 set 方法。

复用改进：

```java
public class WebUtils {
    
    public static void copyParamToBean(Map paramMap, Object bean) {

        try {
            BeanUtils.populate(bean, paramMap);
        } catch (IllegalAccessException e) {
            throw new RuntimeException(e);
        } catch (InvocationTargetException e) {
            throw new RuntimeException(e);
        }
    }

}
```

进一步优化：使用泛型，直接返回封装好的对象。

```java
public class WebUtils {

    public static<T> T copyParamToBean(Map paramMap, T bean) {

        try {
            BeanUtils.populate(bean, paramMap);
        } catch (IllegalAccessException e) {
            throw new RuntimeException(e);
        } catch (InvocationTargetException e) {
            throw new RuntimeException(e);
        }

        return bean;
    }

}
```

# 20221025

## 一、书城项目第四阶段

### 1、使用 EL 表达式改造回显

```html
<input class="itxt" type="text" placeholder="请输入用户名" autocomplete="off" tabindex="1" name="username" id="username" value="${requestScope.username}"/>
```

由于 EL 对于空串的输出为空，因此 `value="${requestScope.username}` 无需额外判断。

## 二、书城项目第五阶段-图书模块

### 1、MVC 概念

MVC：Model 模型、View 视图、Controller 控制器。

![image-20221025084854963](img/image-20221025084854963.png)

![image-20221025085047135](img/image-20221025085047135.png)

### 2、图书模块

#### (1) 创建图书数据库表

涉及到的图书属性有：id(自增主键)、名称、作者、价格、销量、库存、封面路径。

建表的 SQL 语句：

```sql
-- ----------------------------
-- Table structure for t_book
-- ----------------------------
DROP TABLE IF EXISTS `t_book`;
CREATE TABLE `t_book`  (
  `id` int NOT NULL AUTO_INCREMENT,
  `name` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL DEFAULT NULL,
  `author` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL DEFAULT NULL,
  `price` decimal(10, 2) NULL DEFAULT NULL,
  `sales` int NULL DEFAULT NULL,
  `stock` int NULL DEFAULT NULL,
  `img_path` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL DEFAULT NULL,
  PRIMARY KEY (`id`) USING BTREE
) ENGINE = InnoDB AUTO_INCREMENT = 21 CHARACTER SET = utf8mb4 COLLATE = utf8mb4_general_ci ROW_FORMAT = Dynamic;

-- ----------------------------
-- Records of t_book
-- ----------------------------
INSERT INTO `t_book` VALUES (1, 'java从入门到放弃', '国哥', 80.00, 9999, 9, 'static/img/default.jpg');
INSERT INTO `t_book` VALUES (2, '数据结构与算法', '严敏君', 78.50, 6, 13, 'static/img/default.jpg');
INSERT INTO `t_book` VALUES (3, '怎样拐跑别人的媳妇', '龙伍', 68.00, 99999, 52, 'static/img/default.jpg');
INSERT INTO `t_book` VALUES (4, '木虚肉盖饭', '小胖', 16.00, 1000, 50, 'static/img/default.jpg');
INSERT INTO `t_book` VALUES (5, 'C++编程思想', '刚哥', 45.50, 14, 95, 'static/img/default.jpg');
INSERT INTO `t_book` VALUES (6, '蛋炒饭', '周星星', 9.90, 12, 53, 'static/img/default.jpg');
INSERT INTO `t_book` VALUES (7, '赌神', '龙伍', 66.50, 125, 535, 'static/img/default.jpg');
INSERT INTO `t_book` VALUES (8, 'Java编程思想', '阳哥', 99.50, 47, 36, 'static/img/default.jpg');
INSERT INTO `t_book` VALUES (9, 'JavaScript从入门到精通', '婷姐', 9.90, 85, 95, 'static/img/default.jpg');
INSERT INTO `t_book` VALUES (10, 'cocos2d-x游戏编程入门', '国哥', 49.00, 52, 62, 'static/img/default.jpg');
INSERT INTO `t_book` VALUES (11, 'C语言程序设计', '谭浩强', 28.00, 52, 74, 'static/img/default.jpg');
INSERT INTO `t_book` VALUES (12, 'Lua语言程序设计', '雷丰阳', 51.50, 48, 82, 'static/img/default.jpg');
INSERT INTO `t_book` VALUES (13, '西游记', '罗贯中', 12.00, 19, 9999, 'static/img/default.jpg');
INSERT INTO `t_book` VALUES (14, '水浒传', '华仔', 33.05, 22, 88, 'static/img/default.jpg');
INSERT INTO `t_book` VALUES (15, '操作系统原理', '刘优', 133.05, 122, 188, 'static/img/default.jpg');
INSERT INTO `t_book` VALUES (16, '数据结构 java版', '封大神', 173.15, 21, 81, 'static/img/default.jpg');
INSERT INTO `t_book` VALUES (17, 'UNIX高级环境编程', '乐天', 99.15, 210, 810, 'static/img/default.jpg');
INSERT INTO `t_book` VALUES (18, 'javaScript高级编程', '国哥', 69.15, 210, 810, 'static/img/default.jpg');
INSERT INTO `t_book` VALUES (19, '大话设计模式', '国哥', 89.15, 20, 10, 'static/img/default.jpg');
INSERT INTO `t_book` VALUES (20, '人月神话', '刚哥', 88.15, 20, 80, 'static/img/default.jpg');

SET FOREIGN_KEY_CHECKS = 1;
```

> 如何将表导出为 SQL 语句？
>
> ![image-20221025090759208](img/image-20221025090759208.png)

#### (2) 创建 Bean

> 注：基本数据类型要封装一下，以便接收 null。

#### (3) 编写 Dao 并测试

需要实现的功能：

​	DML：添加图书、删除图书和修改图书。

​	DQL：查询单个图书、查询多个图书。

`BookDao` 中规定了这些功能。

`BookDaoImpl` 实现 `BookDao` 继承 `BasicDao`。

#### (4) 编写 Service 并测试

需要实现的功能：

​	图书的增、删、改

​	查询单个图书、查询多个图书

`BookService` 规定了这些功能。

`BookServiceImpl` 利用 `BookDao` 实现了这些功能。

#### (5) 编写 web 层与页面联调

首先实现列表显示功能，其流程如下：

![image-20221025111857908](img/图书显示流程.png)

请求 `book_manager.jsp` 的页面首先发送请求到 `BookServlet`。

```html
<a href="bookServlet?action=listBook">图书管理</a>
```

`BookServlet` 继承 `BaseServlet`，`doPost()` 可以直接使用反射机制，使用 action 的值调用方法，只需要写 `listBook()` 方法即可。

```java
protected void listBook(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 使用 BookService 查询图书列表
        List<Book> books = bookService.queryBooks();

        // 2、将图书列表放入 request 域中
        request.setAttribute("books", books);

        // 请求转发到 /pages/manager/book_manager.jsp
        request.getRequestDispatcher("/pages/manager/book_manager.jsp").forward(request, response);
}
```

但是 `BaseServlet` 中 `doGet()` 方法没有使用反射调用方法，直接调用 `doPost()` 即可。

```
protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doPost(req, resp);
    }
```

# 20221026

## 一、前台与后台

![image-20221026153646806](img/image-20221026153646806.png)

## 二、书城项目第五阶段——图书模块

### 1、图书模块

#### (1) 添加图书

添加图书流程：

![image-20221026154409513](img/添加图书.png)

**`book_edit.jsp` 页面的修改**

修改 `book_edit.jsp` 提交地址：

```html
<form action="manager/bookServlet" method="post">
```

设置隐藏域，提交 `action` 属性：

```html
<input type="hidden" name="action" value="addBook">
```

修改各个输入框 `name` 与 Java Bean 对象一致：

```html
<td><input name="name" type="text" value="时间简史"/></td>
```

**`BookServlet` 程序的修改**

```java
    protected void addBook(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 获取请求参数，封装成为Book bean 对象
        Book book = WebUtils.copyParamToBean(request.getParameterMap(), new Book());

        // 调用 BookService 中的 addBook 方法
        bookService.addBook(book);

        // 跳转到图书列表页面
        request.getRequestDispatcher("/manager/bookServlet?action=listBook").forward(request, response);
    }
```

存在的 bug 1：使用请求转发方式，如果刷新，请求会重复发送，导致再次添加图书。

修改：使用请求重定向方式。

```java
response.sendRedirect("/book/manager/bookServlet?action=listBook");
```

存在的 bug 2：中文乱码问题。

数据库中保存的数据存在 `æ¶é´ç®å²` 样式的乱码，说明解码方式出错，要制定使用 UTF-8 模式解码请求。

解决：在 `BaseServlet.java` 中，设置请求的解码方式：

```java
req.setCharacterEncoding("UTF-8");
```

#### (2) 删除图书

总体流程：

![image-20221026171150910](img/image-20221026171150910.png)

疑问1：为什么可以获得 id？

![image-20221026173011252](img/image-20221026173011252.png)

添加图书创建的 Java Bean 对象是没有 id 的，但是访问数据库回传的 Java Bean 对象有 id。

**修改 `book_manager.jsp` 页面 “删除” 超链接请求 url：**

```html
<td><a href="manager/bookServlet?action=deleteBook&id=${book.id}">删除</a></td>
```

**编写 `BookServlet` 中的 `deleteBook()` 方法：**

```java
    protected void deleteBook(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 获取删除图书 id
        String idStr = request.getParameter("id");
        int id = Integer.parseInt(idStr);

        // 删除图书
        bookService.deleteBookById(id);

        // 重定向到 /book/manager/book_manager.jsp
        response.sendRedirect(request.getContextPath() + "/manager/bookServlet?action=listBook");
    }
```

改进：删除键添加确认

在  `book_manager.jsp` 页面中：

```javascript
<script type="text/javascript">
        $(function () {
            // 给删除按钮绑定单击事件
            $("a.deleteBtn").click(function () {
                // false 会取消元素的默认行为
                // this 是发生此事件的 DOM 对象，$(this) 可以将 DOM 对象封装为 jQuery 对象
                return confirm("您确定要删除【"+$(this).parent().parent().children().first().text()+"】吗？")
            })
        })
</script>
```

#### (3) 修改图书

**a. 图书信息的回显**

总体流程图：

![image-20221026181525787](img/image-20221026181525787.png)

`book_manager.jsp` 中：

```html
<td><a href="manager/bookServlet?action=getBook&id=${book.id}">修改</a></td>
```

`BookServlet` 中：

```
    protected void getBook(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 获取图书编号
        String idStr = request.getParameter("id");
        int id = Integer.parseInt(idStr);

        // 根据图书编号获取图书对象
        Book book = bookService.queryBookById(id);

        // 将图书对象放入 request 域
        request.setAttribute("Book", book);

        // 请求转发
        request.getRequestDispatcher("//pages/manager/book_edit.jsp").forward(request, response);
    }
```

`book_edit.jsp` 中：

```html
<td><input name="name" type="text" value="${requestScope.Book.name}"/></td>
```

**b. 保存修改**

整体流程：

![image-20221026184135839](img/image-20221026184135839.png)

**`book_edit.jsp`** 

根据 request 域中是否存在查询到的 book 对象判断进行的操作，将其中的隐藏域修改如下：

```html
<input type="hidden" name="action" value="${empty requestScope.Book ? "addBook" : "updateBook"}">
```

**`BookServlet`**

```java
protected void updateBook(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 获取请求参数，封装成为 Book 对象
        Book book = WebUtils.copyParamToBean(request.getParameterMap(), new Book());

        // 调用 Service update 方法
        bookService.updateBook(book);

        // 重定向到 /book/manager/book_manager.jsp
        response.sendRedirect(request.getContextPath() + "/manager/bookServlet?action=listBook");
    }
```

存在的问题：封装的 Book 对象没有 id 属性，无法执行 update。

解决方案：在 `book_edit` 中添加一个隐藏域，放入 id 的值。

```html
<input type="hidden" name="id" value="${requestScope.Book.id}">
```

# 20221027

## 一、书城第五阶段——图书模块

### 1、图书模块

#### (4) 图书分页

总体流程图：

![image-20221027082748324](img/image-20221027082748324.png)

##### **a. Page 类的编写**

使用泛型可以使分页模型用于更多的场合。

```java
public class Page<T> {
    public static final Integer DEFAULT_PAGE_SIZE = 4;

    private Integer pageNo; // 当前页码
    private Integer pageTotal; // 总页码
    private Integer pageTotalCount; // 总记录数
    private Integer pageSize = DEFAULT_PAGE_SIZE; // 每页显示记录数
    private List<T> items; // 当前页数据

    // 构造器
    public Page() {
    }

    public Page(Integer pageNo, Integer pageTotal, Integer pageTotalCount, Integer pageSize, List<T> items) {
        this.pageNo = pageNo;
        this.pageTotal = pageTotal;
        this.pageTotalCount = pageTotalCount;
        this.pageSize = pageSize;
        this.items = items;
    }

    // getter and setter
    public Integer getPageNo() {
        return pageNo;
    }

    public void setPageNo(Integer pageNo) {
        this.pageNo = pageNo;
    }

    public Integer getPageTotal() {
        return pageTotal;
    }

    public void setPageTotal(Integer pageTotal) {
        this.pageTotal = pageTotal;
    }

    public Integer getPageTotalCount() {
        return pageTotalCount;
    }

    public void setPageTotalCount(Integer pageTotalCount) {
        this.pageTotalCount = pageTotalCount;
    }

    public Integer getPageSize() {
        return pageSize;
    }

    public void setPageSize(Integer pageSize) {
        this.pageSize = pageSize;
    }

    public List<T> getItems() {
        return items;
    }

    public void setItems(List<T> items) {
        this.items = items;
    }
}
```

##### **b. `BookServlet`** 

```java
protected void page(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 获取请求参数 pageSize 和 pageNo
        int pageSize = WebUtils.parseInt(request.getParameter("pagesize"), Page.DEFAULT_PAGE_SIZE);
        int pageNo = WebUtils.parseInt(request.getParameter("pageno"), 1);

        // 调用 Service 的 page() 获取分页对象
        Page<Book> page = bookService.page(pageNo, pageSize);

        // 将分页对象放入 request 域中
        request.setAttribute("page", page);

        // 请求转发
        request.getRequestDispatcher("/pages/manager/book_manager.jsp").forward(request, response);
    }
```

##### **c. `manager.jsp`**

将 “图书管理” 的超链接修改为：

```html
<a href="manager/bookServlet?action=page">图书管理</a>
```

##### **d. `BookServiceImpl.java`** 

`page()` 方法：

```java
    public Page<Book> page(int pageNo, int pageSize) {
        // 查询总图书数
        int pageTotalCount = bookDao.queryTotalCount();
        // 总页码
        int flag = pageTotalCount / pageSize;
        int totalPage = pageTotalCount % pageSize == 0 ? flag : flag + 1;
        // 当前页数据
        int begin = (pageNo - 1) * pageSize;
        List<Book> items = bookDao.queryBooksForCurrentPage(begin, pageSize);
        // 创建 Page 对象
        Page<Book> page = new Page<>(pageNo, totalPage, pageTotalCount, pageSize, items);

        return page;
    }
```

##### **e. `BookDaoImpl.java`**

```java
    public int queryTotalCount() {
        String sql = "select count(*) from t_book";
        Number number = (Number) queryScalar(sql);
        return number.intValue();
    }

    @Override
    public List<Book> queryBooksForCurrentPage(int begin, int pageSize) {
        String sql = "select * from t_book limit ?, ?";
        return queryMulti(sql, Book.class, begin, pageSize);
    }
```

写完后进行测试，Dao 和 Service 测试可以单独测试，Web 测试需要和页面联调。

##### 引入页码导引：

![image-20221027100723275](img/image-20221027100723275.png)

**页码导引的实现：**

使用 EL 表达式进行有选择的输出：

```html
<div id="page_nav">
        <%--只有非首页才能显示 “首页” 和 “上一页”--%>
        <c:if test="${requestScope.page.pageNo > 1}">
            <a href="manager/bookServlet?action=page&pageno=1">首页</a>
            <a href="manager/bookServlet?action=page&pageno=${requestScope.page.pageNo - 1}">上一页</a>
            <a href="manager/bookServlet?action=page&pageno=${requestScope.page.pageNo - 1}">${requestScope.page.pageNo - 1}</a>
        </c:if>

        【${requestScope.page.pageNo}】

        <%--只有非末页才能显示 “末页” 和 “下一页”--%>
        <c:if test="${requestScope.page.pageNo < requestScope.page.pageTotal}">
            <a href="manager/bookServlet?action=page&pageno=${requestScope.page.pageNo + 1}">${requestScope.page.pageNo + 1}</a>
            <a href="manager/bookServlet?action=page&pageno=${requestScope.page.pageNo + 1}">下一页</a>
            <a href="manager/bookServlet?action=page&pageno=${requestScope.page.pageTotal}">末页</a>
        </c:if>

        共${requestScope.page.pageTotal}页，${requestScope.page.pageTotalCount}条记录 到第<input value="${param.pageno}" name="pn"
                                                                                         id="pn_input"/>页
        <input type="button" id="searchPageBtn" value="确定">
    </div>
```

搜索页码的实现：

```javascript
            // 给搜索页码按钮绑定单击事件
            $("#searchPageBtn").click(function () {
                // 获取输入框中的值
                var pageNo = $("#pn_input").val();

                // 修改地址栏的 href 属性
                location.href = "manager/bookServlet?action=page&pageno=" + pageNo;

            })
```

##### 数据有效边境检查

前端和后端都要进行有效性检查。

前端使用 js 进行有效性检查：

前端校验防止用户输入无效数据。

`book_manager.jsp` 中

```js
var pageTotal = ${requestScope.page.pageTotal}

                // 数据有效性判断
                if (pageNo < 1 || pageNo > pageTotal) {
                    alert("请输入合法的页码值！")
                    return false;
                }
```

后端校验防止黑客。

`BookServiceImpl` 中

```java
        // pageNo 边界校验
        if (pageNo < 1) {
            pageNo = 1;
        }
        if (pageNo > totalPage) {
            pageNo = totalPage;
        }
```

# 20221028

## 一、书城第五阶段——图书模块

### 1、图书模块

#### (4) 图书分页

需求：显示 5 个连续页码，当前页码在中间。

情况一：总页码小于等于5 页码范围：1-5

```html
<c:when test="${requestScope.page.pageTotal <= 5}">
                <c:forEach begin="1" end="${requestScope.page.pageTotal}" var="i">
                    <%--当前页--%>
                    <c:if test="${i == requestScope.page.pageNo}">
                        【${i}】
                    </c:if>
                    <c:if test="${i != requestScope.page.pageNo}">
                        <a href="manager/bookServlet?action=page&pageno=${i}">${i}</a>
                    </c:if>
                </c:forEach>
            </c:when>
```

> 注意：地址栏书写没有任何空格！！！

情况二：总页码大于 5

```html
<%--情况二：总页码大于 5--%>
<c:when test="${requestScope.page.pageTotal > 5}">
    <c:choose>
        <%--当前页码为前三个，页码范围 1-5--%>
        <c:when test="${requestScope.page.pageNo < 4}">
            <c:forEach begin="1" end="5" var="i">
                <%--当前页--%>
                <c:if test="${i == requestScope.page.pageNo}">
                    【${i}】
                </c:if>
                <%--非当前页--%>
                <c:if test="${i != requestScope.page.pageNo}">
                    <a href="manager/bookServlet?action=page&pageno=${i}">${i}</a>
                </c:if>
            </c:forEach>
        </c:when>
        <%--当前页码为最后三个，页码范围 总页码-4~总页码--%>
        <c:when test="${requestScope.page.pageNo >= requestScope.page.pageTotal - 2}">
            <c:forEach begin="${requestScope.page.pageTotal - 4}" end="${requestScope.page.pageTotal}" var="i">
                <%--当前页--%>
                <c:if test="${i == requestScope.page.pageNo}">
                    【${i}】
                </c:if>
                <%--非当前页--%>
                <c:if test="${i != requestScope.page.pageNo}">
                    <a href="manager/bookServlet?action=page&pageno=${i}">${i}</a>
                </c:if>
            </c:forEach>
        </c:when>
        <%--其他情况 页码范围 当前页码-2 ~ 当前页码+2--%>
        <c:otherwise>
            <c:forEach begin="${requestScope.page.pageNo - 2}" end="${requestScope.page.pageNo + 2}" var="i">
                <%--当前页--%>
                <c:if test="${i == requestScope.page.pageNo}">
                    【${i}】
                </c:if>
                <%--非当前页--%>
                <c:if test="${i != requestScope.page.pageNo}">
                    <a href="manager/bookServlet?action=page&pageno=${i}">${i}</a>
                </c:if>
            </c:forEach>
        </c:otherwise>
    </c:choose>
</c:when>
```

代码优化：forEach 标签内部的代码都是一样的，只是 begin 和 end 不一样，将其提取出来，分支中只需要设置值。

分支中：

```html
<c:when test="${requestScope.page.pageTotal <= 5}">
                <c:set var="begin" value="1"></c:set>
                <c:set var="end" value="${requestScope.page.pageTotal}"></c:set>
            </c:when>
```

最后统一赋值：

```html
        <c:forEach begin="${begin}" end="${end}" var="i">
            <%--当前页--%>
            <c:if test="${i == requestScope.page.pageNo}">
                【${i}】
            </c:if>
            <%--非当前页--%>
            <c:if test="${i != requestScope.page.pageNo}">
                <a href="manager/bookServlet?action=page&pageno=${i}">${i}</a>
            </c:if>
        </c:forEach>
```

# 20221029

## 一、书城第五阶段——图书模块

### 1、图书模块

#### (4) 图书分页

添加分页后，添加/删除图书都应该重定向到分页功能。

在进行修改时，还要记录当前页码。

`manager.jsp` 向 `book_edit.jsp` 发送请求时，还要附带当前页码：

```html
<td><a href="manager/bookServlet?action=getBook&id=${book.id}&pageNo=${requestScope.page.pageNo}">修改</a></td>
```

`book_edit.jsp` 要将当前页码附带在隐藏域中：

```html
<input type="hidden" name="pageNo" value="${param.pageNo}">
```

`BookServlet` 的 `updateBook()` 中，调用 `page()` 时附带页码：

```java
int pageNo = WebUtils.parseInt(request.getParameter("pageNo"), 1);
response.sendRedirect(request.getContextPath() + "/manager/bookServlet?action=page" + "&pageno=" + pageNo);
```

#### (5) 前台分页

创建 `ClientBookServlet` 实现前台分页等功能：

```java
public class ClientBookServlet extends BaseServlet {
    BookService bookService = new BookServiceImpl();


    protected void page(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 获取请求参数 pageSize 和 pageNo
        int pageSize = WebUtils.parseInt(request.getParameter("pagesize"), Page.DEFAULT_PAGE_SIZE);
        int pageNo = WebUtils.parseInt(request.getParameter("pageno"), 1);

        // 调用 Service 的 page() 获取分页对象
        Page<Book> page = bookService.page(pageNo, pageSize);

        // 将分页对象放入 request 域中
        request.setAttribute("page", page);

        // 请求转发
        request.getRequestDispatcher("/pages/client/index.jsp").forward(request, response);
    }
}
```

`index.jsp` 只进行请求转发：

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<jsp:forward page="/client/clientBookServlet?action=page"></jsp:forward>
```

`/pages/client/index.jsp` 负责展示：

```HTML
<%--遍历 page.items 中的项--%>
			<c:forEach items="${requestScope.page.items}" var="book">
				<div class="b_list">
					<div class="img_div">
						<img class="book_img" alt="" src="static/img/default.jpg" />
					</div>
					<div class="book_info">
						<div class="book_name">
							<span class="sp1">书名:</span>
							<span class="sp2">${book.name}</span>
						</div>
						<div class="book_author">
							<span class="sp1">作者:</span>
							<span class="sp2">${book.author}</span>
						</div>
						<div class="book_price">
							<span class="sp1">价格:</span>
							<span class="sp2">￥${book.price}</span>
						</div>
						<div class="book_sales">
							<span class="sp1">销量:</span>
							<span class="sp2">${book.sales}</span>
						</div>
						<div class="book_amount">
							<span class="sp1">库存:</span>
							<span class="sp2">${book.stock}</span>
						</div>
						<div class="book_add">
							<button>加入购物车</button>
						</div>
					</div>
				</div>
			</c:forEach>
```

前台的分页只要复制后台分页，再改一下请求的 Servlet 程序地址即可。

#### (6) 分页条的抽取

分页条只有请求地址不同，因此可以抽取出来，这个请求 url 可以提取为 Page 中的变量，在第一次请求 Servlet 时将其放入 request 域中。

`Page` 中添加 `url` 变量，并添加 Setter and getter

```java
private String url; // 请求地址
```

在 `ClientBookServlet` 和 `BookServlet` 中分别设置放入 request 域中的 url 值：

`ClientBookServlet` 中：

```java
page.setUrl("client/clientBookServlet?action=page");
```

`BookServlet` 中：

```java
page.setUrl("manager/bookServlet?action=page");
```

在 `client/index.jsp` 中，将 `client/clientBookServlet?action=page` 替换为 `${requestScope.page.url}`：

```html
<a href= "${requestScope.page.url}&pageno=1">首页</a>
```

在 `/pages/manager/book_manager.jsp` 中进行相似操作。

将分页条抽取到 `/pages/common/page_bar.jsp` 中，进行静态包含。

#### (7) 使用价格查找

总体流程：

![](img/价格分页流程.png)

基本流程实现：

`index.jsp` 中数据有效性判断：

```javascript
<script type="text/javascript">
		$(function () {
			/*给提交按钮绑定单击事件，检验数据正确性*/
			$("#searchSubmitBtn").click(function () {
				// 获取区间下限
				var min = $("#min").val()

				// 获取区间上限
				var max = $("#max").val()

				// 如果区间区间下限大于区间上限
				if (min > max) {
					// 提示错误信息
					alert("非法区间，请重新输入")
					// 取消提交
					return false
				}
			})
		})
	</script>
```

修改请求地址：

```html
<form action="client/clientBookServlet?action=pageByPrice" method="post">
```

`ClientBookServlet.java`

```java
protected void pageByPrice(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 获取请求参数 pageSize, pageNo, min 和 max
        int pageSize = WebUtils.parseInt(request.getParameter("pagesize"), Page.DEFAULT_PAGE_SIZE);
        int pageNo = WebUtils.parseInt(request.getParameter("pageno"), 1);
        int min = WebUtils.parseInt(request.getParameter("min"), 0);
        int max = WebUtils.parseInt(request.getParameter("max"), 1000);

        // 数据正确性校验
        if (min > max) {
            min = 0;
            max = 1000;
        }

        // 调用 Service 的 page() 获取分页对象
        Page<Book> page = bookService.pageByPrice(pageNo, pageSize, min, max);

        // 设置 url
        page.setUrl("client/clientBookServlet?action=pageByPrice");

        // 将分页对象放入 request 域中
        request.setAttribute("page", page);

        // 请求转发
        request.getRequestDispatcher("/pages/client/index.jsp").forward(request, response);
    }
```

`BookService`

```java
Page<Book> pageByPrice(int pageNo, int pageSize, int min, int max);
```

`BookServiceImpl`

```java
public Page<Book> pageByPrice(int pageNo, int pageSize, int min, int max) {

        // 查询价格区间内总图书数
        int pageTotalCount = bookDao.querySearchCount(min, max);
        // 总页码
        int flag = pageTotalCount / pageSize;
        int totalPage = pageTotalCount % pageSize == 0 ? flag : flag + 1;

        // pageNo 边界校验
        if (pageNo < 1) {
            pageNo = 1;
        }
        if (pageNo > totalPage) {
            pageNo = totalPage;
        }

        // 当前页数据
        int begin = (pageNo - 1) * pageSize;
        List<Book> items = bookDao.queryBooksForCurrentPageByPrice(begin, pageSize, min, max);
        // 创建并返回 Page 对象
        return new Page<Book>(pageNo, totalPage, pageTotalCount, pageSize, items);
    }
```

`BookDao`

```java
int querySearchCount(int min, int max);

List<Book> queryBooksForCurrentPageByPrice(int begin, int pageSize, int min, int max);
```

`BookDaoImpl`

```java
    public int querySearchCount(int min, int max) {
        String sql = "select count(*) from t_book where price between ? and ?";
        Number number = (Number) queryScalar(sql, min, max);
        return number.intValue();
    }

    @Override
    public List<Book> queryBooksForCurrentPageByPrice(int begin, int pageSize, int min, int max) {
        String sql = "select * from t_book where price between ? and ? limit ?, ?";
        return queryMulti(sql, Book.class, min, max, begin, pageSize);
    }
```

优化：

1、查询价格的回显

`index.jsp` min 和 max 参数提交后，`ClientBookServlet` 再次放入 `requestScope` 中即可。

`index.jsp`

```html
<input id="min" type="text" name="min" value="${requestScope.min}"> 元 -
```

 `ClientBookServlet`

```java
request.setAttribute("min", min);
request.setAttribute("max", max);
```

改进：

request 会进行转发，请求参数 min 和max 就会保留，因此 `index.jsp` 中直接使用 `${param.min}` 获取即可。

```html
<input id="min" type="text" name="min" value="${param.min}"> 元 -
```

存在的问题：

1、价格区间内没有图书，会出现异常。

经过排查，错误出现在 Service 层，由于价格区间内没有数据，连锁反应导致 begin = -4，会出现 SQL 语法错误

```java
// 查询价格区间内总图书数
int pageTotalCount = bookDao.querySearchCount(min, max); // pageTotalCount = 0
// 总页码
int flag = pageTotalCount / pageSize; // flag = 0
int totalPage = pageTotalCount % pageSize == 0 ? flag : flag + 1; // totalPage = 0

// pageNo 边界校验
if (pageNo < 1) { 
	pageNo = 1;
}// pageNo = 1
if (pageNo > totalPage) {
	pageNo = totalPage;
}// pageNo = 0

// 当前页数据
int begin = (pageNo - 1) * pageSize; // begin = -4
List<Book> items = bookDao.queryBooksForCurrentPageByPrice(begin, pageSize, min, max);
```

将 16 行进行如下修改：

```java
int begin = (pageNo - 1) * pageSize >= 0 ? (pageNo - 1) * pageSize : 0;
```

这样 begin 就不会为负数了。

2、点击页码，并不会显示应有的内容，而是其他区间范围的很多内容

经过排查，错误出现在 web 层，`ClientBookServlet` 在设置 url 时没有附带 min 和 max，如下所示：

```java
protected void pageByPrice(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 获取请求参数 pageSize, pageNo, min 和 max
        int pageSize = WebUtils.parseInt(request.getParameter("pagesize"), Page.DEFAULT_PAGE_SIZE);
        int pageNo = WebUtils.parseInt(request.getParameter("pageno"), 1);
        int min = WebUtils.parseInt(request.getParameter("min"), 0);
        int max = WebUtils.parseInt(request.getParameter("max"), 1000);

        // 数据正确性校验
        if (min > max) {
            min = 0;
            max = 1000;
        }

        // 调用 Service 的 page() 获取分页对象
        Page<Book> page = bookService.pageByPrice(pageNo, pageSize, min, max);

        // 设置 url
        page.setUrl("client/clientBookServlet?action=pageByPrice");

        // 将分页对象放入 request 域中
        request.setAttribute("page", page);

        // 将 min 和 max 也放入 request 域中
        request.setAttribute("min", min);
        request.setAttribute("max", max);

        // 请求转发
        request.getRequestDispatcher("/pages/client/index.jsp").forward(request, response);
    }
```

这样会导致显示价格区间在 0~1000 相应页码的条目。

将 18 行修改如下：

```java
page.setUrl("client/clientBookServlet?action=pageByPrice&min=" + min + "&max=" + max);
```

# 20221030

## 一、Cookie

### 1、概念

Cookie 是服务器通知客户端保存键值对的一种技术，客户端有了 Cookie 后，每次请求都发送给服务器。

Cookie 大小限制为 4 kb。

### 2、创建 Cookie

总体流程：

![image-20221030094436735](img/image-20221030094436735.png)

```java
    protected void createCookie(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 创建 Cookie 对象
        Cookie cookie = new Cookie("key1", "value1");
        
        // 解决乱码问题
        response.setContentType("text/html; charset=UTF-8");

        // 通知客户端保存
        response.addCookie(cookie);

        response.getWriter().write("Cookie 创建成功！");
    }
```

### 3、服务器 Cookie 的获取

在请求中获取 Cookie。

```java
    protected void getCookie(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 获取 Cookie
        Cookie[] cookies = request.getCookies();

        for (Cookie cookie : cookies) {
            System.out.print(cookie.getName() + "-" + cookie.getValue());
            System.out.println();
        }
    }
```

没有找特定 Cookie 的方法，只能在遍历过程中进行判断，这种需求十分常见，可以抽取到工具类中。

`CookieUtils.java`

```java
public class CookieUtils {
    /**
     * 获取指定名称的 Cookie
     * @param name Cookie 名称
     * @param cookies Cookie 数组
     * @return 指定名称的 Cookie 对象
     */
    public static Cookie findCookie(String name, Cookie[] cookies) {
        if (name == null || cookies == null || cookies.length == 0) {
            return null;
        }

        for (Cookie cookie : cookies) {
            if (name.equals(cookie.getName())) {
                return cookie;
            }
        }

        return null;
    }
}
```

### 4、Cookie 值的修改

方案一：直接创建同名 Cookie 对象，直接添加覆盖。

```java
    protected void updateCookie(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 创建同名 Cookie 对象
        Cookie cookie = new Cookie("key1", "newValue1");

        // 传给浏览器
        response.addCookie(cookie);
    }
```

方案二：查找到需要修改的 Cookie 对象，调用 setValue() 方法，再传给浏览器。

### 5、谷歌浏览器 Cookie 的查看

![image-20221030095603826](img/image-20221030095603826.png)

### 6、Cookie 生命控制

管理 Cookie 何时销毁。

使用到的 API：

![image-20221030100018445](img/image-20221030100018445.png)

正数——在指定秒数后销毁。

负数——浏览器关闭时删除。

0——马上删除。

```java
protected void deleteCookie(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 获取指定条件的 Cookie
        Cookie cookie = CookieUtils.findCookie("defaultKey", request.getCookies());

        // 设置其存活时间为0
        cookie.setMaxAge(0);

        // 传给客户端
        response.addCookie(cookie);
    }
```

### 7、Cookie 的有效路径 path

path 属性可以过滤 Cookie。

![image-20221030102305236](img/image-20221030102305236.png)

浏览器可以根据 cookie 的 path 属性与地址栏地址是否匹配，过滤服务器发来的 Cookie。

```java
    protected void setCookiePath(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 创建 Cookie
        Cookie cookie = new Cookie("pathCookie", "pathValue");

        // 设置路径
        cookie.setPath(request.getContextPath() + "/abc");

        // 传给客户端
        response.addCookie(cookie);
    }
```

在地址 `http://localhost:8080/cookie_session/cookie.html` 中不会保存上述 Cookie。

### 8、免用户名登录

流程：

![image-20221030103349158](img/image-20221030103349158.png)

## 二、Session

### 1、概念

会话，由服务器存储。用户登录后的信息经常放到 Session 中保存。

### 2、Session 的创建和获取

使用的 API：`request.getSession()`

Session 是单例模式，第一次调用将创建 Session 会话，后面是获取。

判断会话是否是新创建的：`session.isNew()`

每个会话都有一个 id 值：`session.getId()`

使用案例：

```java
protected void createOrGetSession(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 创建或获取 Session
        HttpSession session = request.getSession();

        // 判断 Session 是否为新创建的
        System.out.println(session.isNew() ? "Session 是新创建的" : "Session 非新创建");

        // 获取会话 id
        System.out.println("会话id：" + session.getId());
    }
```

### 3、Session 域数据的存取

```java
    protected void setAttribute(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        request.getSession().setAttribute("key1", "value1");
        System.out.println("向 Session 域中存储数据");
    }

    protected void getAttribute(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        Object key1 = request.getSession().getAttribute("key1");
        System.out.println(key1);
    }
```

### 4、Session 生命控制

使用到的 API：

![image-20221030163312729](img/image-20221030163312729.png)

Session 默认存活时间：1800s。

如果希望修改 web 工程的默认超时时长，可以在 `web.xml` 中进行修改。

```xml
    <session-config>
        <session-timeout>10</session-timeout>
    </session-config>
```

```java
    protected void setLife3(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 设置会话超时时间为 3 秒
        request.getSession().setMaxInactiveInterval(3);
    }
```

Session 超时概念：

![image-20221030172508464](img/image-20221030172508464.png)

主动让会话超时：

![image-20221030172803010](img/image-20221030172803010.png)

使用案例：

```java
    protected void deleteNow(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        request.getSession().invalidate();
    }
```

### 5、Session 技术原理

Session 技术基于 Cookie。

![image-20221030174321848](img/image-20221030174321848.png)

# 20221031

## 一、书城第六阶段

### 1、登录信息显示

登录成功后，将用户名显示到 “韩总” 位置。

![image-20221031081922168](img/image-20221031081922168.png)

实现：登录成功，将 User 对象放入 Session 域中。

`UserServlet.java` 中 `login()` 方法：

```java
        if (loginUser != null) { // 登录成功
            // 将用户名放入 Session 域中
            request.getSession().setAttribute("user", loginUser);

            // 请求转发方式跳转至登录成功页面
            request.getRequestDispatcher("/pages/user/login_success.jsp").forward(request, response);
        }
```

`action_success.jsp` 中：

```html
<span>欢迎<span class="um_span">${sessionScope.user.username}</span>光临尚硅谷书城</span>
```

登录成功后，在首页保留登录信息，并在登录成功后停止显示 “登录” 和 “注册”：

`client/index.jsp` 中：

```html
        <%--没有登录--%>
        <c:if test="${empty sessionScope.user}">
            <a href="pages/user/login.jsp">登录</a> |
            <a href="pages/user/regist.jsp">注册</a> &nbsp;&nbsp;
        </c:if>
        <%--已经登录--%>
        <c:if test="${not empty sessionScope.user}">
            <span>欢迎<span class="um_span">${sessionScope.user.username}</span>光临尚硅谷书城</span>
        </c:if>
```

### 2、注销

流程：

销毁 Session，重定向到登录界面。

`UserServlet.java` 中：

```java
    protected void logout(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 销毁 Session
        request.getSession().invalidate();

        // 重定向到登录界面
        response.sendRedirect(request.getContextPath());
    }
```

`client/index.jsp` 中：

```html
<a href="userServlet?action=logout">注销</a>&nbsp;&nbsp;
```

### 3、表单重复提交的三种情况

情况一：使用请求转发方式，用户使用 F5 刷新页面，就会发起最后一次请求。

​	使用请求重定向来解决。

情况二：多次点击提交，但服务器未响应。

情况三：用户回退，重复提交。

情况二、三可以使用验证码进行解决。

验证码基本原理：

![image-20221031085910483](img/image-20221031085910483.png)

用户第一次访问表单时，会有一个 KaptchaServlet 程序生成验证码并保存到 Session 域中，同时这个程序可以生成验证码图片显示在界面中。用户将数据与验证码一并提交给另一个 Servlet 程序，这个 Servlet 程序会首先获取验证码，并立即在域中将其删除，只有在验证码正确时才会允许操作。

如果用户回退，验证码便为 null，无论如何都无法提交操作。

**谷歌验证码 Kaptcha 的使用**

引入依赖：

```xml
<!-- https://mvnrepository.com/artifact/com.github.penggle/kaptcha -->
<dependency>
    <groupId>com.github.penggle</groupId>
    <artifactId>kaptcha</artifactId>
    <version>2.3.2</version>
</dependency>
```

在 `web.xml` 中配置生成验证码的 Servlet 程序：

```xml
    <servlet>
        <servlet-name>KaptchaServlet</servlet-name>
        <servlet-class>com.google.code.kaptcha.servlet.KaptchaServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>KaptchaServlet</servlet-name>
        <url-pattern>/kaptcha.jpg</url-pattern>
    </servlet-mapping>
```

在表单中使用 img 标签使用验证码：

```html
<img alt="" src="kaptcha.jpg" style="float: right; margin-right: 40px">
```

在服务器中比较生成验证码和用户输入验证码：

```java
 // 获取生成的验证码，并立即删除
        String token = (String) request.getSession().getAttribute(KAPTCHA_SESSION_KEY);
        request.getSession().removeAttribute(KAPTCHA_SESSION_KEY);
if (token != null && token.equalsIgnoreCase(code)) { // 验证码正确
```

验证码的切换：

给验证码图片绑定单击事件，修改其 src 属性，重新请求 KaptchaServlet。

```javascript
$("#code_img").click(function () {
	this.src = ${basePath}"kaptcha.jpg"
})
```

由于浏览器有缓存，缓存原理图如下：

![image-20221031100722272](img/image-20221031100722272.png)

浏览器为了让请求更快，设置了缓存，缓存就是将请求路径及参数信息和资源进行绑定存储。如果想要绕过缓存向服务器发送请求，可以添加每次都不相同的参数，比如时间戳。

```javascript
// 给验证码图片绑定单击事件
$("#code_img").click(function () {
	this.src = ${basePath}"kaptcha.jpg?d=" + new Date()
})
```

### 4、购物车模块

购物车模型分析：

![image-20221031113551305](img/image-20221031113551305.png)

购物车(Cart)模型：

​	商品项列表 items

​	商品总数 cartCount

​	商品总金额 cartTotalPrice

商品项(CartItem)模型：

​	商品编号 id

​	商品名称 name

​	数量 count

​	单价 price

​	总价 totalPrice

![image-20221031114115656](img/image-20221031114115656.png)

这次使用 Session 版本，因此不涉及数据库。



购物车功能分析：

​	加入购物车

​	删除商品项

​	清空购物车

​	修改商品数量

![image-20221031114530520](img/image-20221031114530520.png)

因为不涉及数据库，因此没有 Service 和 Dao，增删改查在 Cart 中实现。

#### (1) 购物车模型的实现

`CartItem.java`

```java
private Integer id; // 商品编号
private String name; // 商品名称
private Integer count; // 商品数量
private Double price; // 商品单价
private Double totalPrice; // 总价
```

`Cart.java`

```java
private List<CartItem> items = new ArrayList<>(); // 商品项列表
private Integer cartCount; // 商品总数
private Double cartTotalPrice; // 商品总金额
```

#### (2) 购物车功能实现

`Cart.java`

```java
public void add(CartItem item) {
        // 更新商品总数和商品总金额
        cartCount ++;
        cartTotalPrice += item.getPrice();

        CartItem cartItem = items.get(item.getId());
        if (cartItem != null) { // 该商品已经添加到购物车
            // 更新商品项商品数量和总价
            cartItem.setCount(cartItem.getCount() + 1);
            cartItem.setTotalPrice(cartItem.getTotalPrice() + item.getPrice());
        } else { // 商品未添加到购物车
            items.put(item.getId(), item);
        }
    }

    /**
     * 删除指定编号的商品项
     * @param id
     */
    public void delete(Integer id) {
        // 购物车中删除商品项
        CartItem removeItem = items.remove(id);
        if (removeItem != null) {
            // 购物车总金额和总商品数减少
            cartTotalPrice -= removeItem.getTotalPrice();
            cartCount -= removeItem.getCount();
        }

    }

    /**
     * 清空购物车
     */
    public void clear() {
        // 清空商品项
        items.clear();
        // 总金额和商品项数量清零
        cartCount = 0;
        cartTotalPrice = 0.0;
    }

    /**
     * 修改商品项数量
     * @param id 商品项编号
     * @param count 更新后的商品项数量
     */
    public void updateCount(Integer id, Integer count) {
        CartItem cartItem = items.get(id);
        if (cartItem != null) {
            // 获取修改前去除该商品项的购物车商品数量和总金额
            int cartCount_minus = cartCount - cartItem.getCount();
            double cartTotalPrice_minus = cartTotalPrice - cartItem.getTotalPrice();

            // 修改商品项数量和总金额
            cartItem.setCount(count);
            cartItem.setTotalPrice(cartItem.getPrice() * count);

            // 修改购物车商品数量和总金额
            cartCount += cartItem.getCount();
            cartTotalPrice += cartItem.getTotalPrice();
        }
    }
```

**加入购物车功能**

`index.jsp` 

将图书编号上传给 Servlet 程序。

```html
<button bookid="${book.id}" class="addToCartBtn">加入购物车</button>
```

```javascript
/*给加入购物车按钮绑定单击事件*/
$("button.addToCartBtn").click(function () {
    // 获取图书编号
    var bookid = $(this).attr("bookid");

    // 请求地址
    location.href = "http://localhost:8080/book/cartServlet?action=add&bookid=" + bookid
})
```

`CartServlet.java` 中 `add()` 方法：

```java
    protected void add(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 获取请求参数id
        int id = WebUtils.parseInt(request.getParameter("bookid"), 0);

        // 根据 id 获取图书信息
        Book book = bookService.queryBookById(id);

        // 根据获取的图书信息转换为 CartItem 对象
        CartItem cartItem = new CartItem(id, book.getName(), 1, book.getPrice(), book.getPrice());

        // 从 Session 域中获取购物车对象，如果没有则需要创建
        Cart cart = (Cart) request.getSession().getAttribute("cart");
        if (cart == null) {
            cart = new Cart();
        }

        // 向购物车中添加商品项
        cart.add(cartItem);

        // 将购物车对象放入 Session 域
        request.getSession().setAttribute("cart", cart);

        // 重定向回首页
        response.sendRedirect(request.getContextPath());
    }
```

首页修改

![image-20221101113027143](img/image-20221101113027143.png)

商品数量显示：

```html
<span>您的购物车中有${empty sessionScope.cart.cartCount ? "0" : sessionScope.cart.cartCount}件商品</span>
```

刚刚添加的商品显示：

`CartServlet.java` 中的 `add()`：

向购物车添加商品后，将刚刚添加商品的名称也添加到 Session 域中。

```java
// 删除 Session 域中的 cartItem 属性，将该属性设置为新创建的 CartItem 对象
HttpSession session = request.getSession();
session.removeAttribute("cartItemName");
session.setAttribute("cartItemName", cartItem.getName());
```

`index.jsp`：

根据 Session 域中是否含有 cartItemName 属性，对其进行输出。

```html
<c:if test="${not empty sessionScope.cartItemName}">
    <div>
        您刚刚将<span style="color: red">${sessionScope.cartItemName}</span>加入到了购物车中
    </div>
</c:if>
```

**购物车显示**

![image-20221101114210398](img/购物车显示.png)

商品项的显示：由于 cart 已经存储在 Session 域中，遍历显示 cart.items 的值即可。

`cart.jsp` 中：

```html
<%--遍历 Session 域中 购物车商品项--%>
<c:forEach items="${sessionScope.cart.items}" var="item">
    <tr>
        <td>${item.value.name}</td>
        <td>${item.value.count}</td>
        <td>${item.value.price}</td>
        <td>${item.value.totalPrice}</td>
        <td><a href="#">删除</a></td>
    </tr>
</c:forEach>
```

购物车商品数和总金额的显示，`cart.jsp` 中：

```html
<span class="cart_span">购物车中共有<span class="b_count">${sessionScope.cart.cartCount}</span>件商品</span>
<span class="cart_span">总金额<span class="b_price">${sessionScope.cart.cartTotalPrice}</span>元</span>
```

**删除商品项**

前端页面 `cart.jsp`，删除时要将图书 id 一并上传：

```html
<td><a href="cartServlet?action=delete&bookid=${item.value.id}">删除</a></td>
```

`CartServlet.java`：

```java
protected void delete(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    // 获取图书 id
    int id = WebUtils.parseInt(request.getParameter("bookid"), 0);

    // 获取 Session 域中的 Cart 对象
    Cart cart = (Cart) request.getSession().getAttribute("cart");

    // 删除购物车中的商品项
    cart.delete(id);

    // 将新的购物车对象放入 Session 域中
    request.removeAttribute("cart");
    request.setAttribute("cart", cart);

    // 重定向到购物车页面
    response.sendRedirect(request.getContextPath() + "/pages" + "/cart" + "/cart.jsp");
}
```

**清空购物车**

`cart.jsp` 没有需要上传的参数

```html
<span class="cart_span"><a href="cartServlet?action=clear">清空购物车</a></span>
```

`CartServlet.java`

```java
protected void clear(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    // 获取 Session 域中 Cart 对象，将其清空并重新放回
    Cart cart = (Cart) request.getSession().getAttribute("cart");
    cart.clear();
    request.removeAttribute("cart");
    request.setAttribute("cart", cart);

    // 重定向到购物车界面
    response.sendRedirect(request.getContextPath() + "/pages" + "/cart" + "/cart.jsp");

}
```

bug 修改与功能完善：

1.加入购物车后，重定向回首页，并不是原来浏览的那一页。

解决方案：在前端页面点击 ”加入购物车“ 后，同时将当前页面 `Page` 对象的 `pageNo` 属性一并上传，在 `CartServlet` 中 `add()` 方法进行请求重定向时，直接重定向到 `BookServlet` 中的 `page()` 方法，并上传 `pageNo` 属性。

`index.jsp`

```html
<button bookid="${book.id}" pageNo="${requestScope.page.pageNo}" class="addToCartBtn">加入购物车</button>
```

`CartServlet` 中 `add()`：

```java
// 重定向回首页
response.sendRedirect(request.getContextPath() + "/client/clientBookServlet?action=page&pageno=" + pageNo );
```

解决方案二：浏览器发起请求，其请求头中包含 `Referer`属性，其中存储请求前地址栏中地址，可以重定向到这个地址。

`CartServlet` 中 `add()`：

```java
String location = request.getHeader("Referer");
response.sendRedirect(location);
```

2.商品显示问题

![image-20221101144235300](img/image-20221101144235300.png)

如上图，加入购物车的商品项名称作为属性放入 `Session` 域中，这样前往其他页面回来后，仍然会显示这条信息，应该将商品项名称作为属性放入 `request` 域，这样前往其他页面回来便不会显示这条信息。

解决方案：在 `CartServlet` 的 `add()` 中，将重定向到首页的请求中加入商品项名称作为参数。

`CartServlet` 中 `add()`：

```java
// 重定向回首页
String location;
if (request.getHeader("User-Agent").contains("FireFox")) { // 使用火狐浏览器
    // 使用 Base64 编码
    location = request.getContextPath() + "/client/clientBookServlet?action=page&pageno=" + pageNo + "&cartItemName=" + new BASE64Encoder().encode(cartItem.getName().getBytes());
} else { // 其他浏览器
    // 使用URL编码
    location = request.getContextPath() + "/client/clientBookServlet?action=page&pageno=" + pageNo + "&cartItemName=" + URLEncoder.encode(cartItem.getName(), "UTF-8");
}

response.sendRedirect(location);
```

**购物车中商品项数量的修改**

![image-20221101145649348](img/image-20221101145649348.png)

分析：将直接输出数量改为输入框，输入框中默认显示点击 “加入购物车” 添加的商品项数量。给输入框绑定失焦事件，失焦时给 `CartServlet` 发请求执行 `update()` 方法，需要上传修改后的数量和商品项 `id`。`CartServlet` 中 `update()` 方法，修改 Session 域中的 `Cart` 对象。

`cart.jsp`

输入框

```html
<td><input type="text" value="${item.value.count}" class="countText" bookId="${item.key}"></td>
```

事件绑定

```javascript
$(function () {
    $(".countText").blur(function () {
        // 获取修改后的数量
        var count = $(this).val();

        // 获取商品项 id
        var id = $(this).attr("bookId");

        // 请求
        location.href = "http://localhost:8080/book/cartServlet?action=update&count=" + count + "&id=" + id
    })
})
```

`CartServlet.java`

```java
protected void update(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    // 获取请求参数
    int count = WebUtils.parseInt(request.getParameter("count"), 1);
    int id = WebUtils.parseInt(request.getParameter("id"), 0);

    // 更新 Session 域中的 cart 对象并重新上传
    HttpSession session = request.getSession();
    Cart cart = (Cart) session.getAttribute("cart");
    cart.updateCount(id, count);
    session.removeAttribute("cart");
    session.setAttribute("cart", cart);

    // 重定向到购物车界面
    response.sendRedirect(request.getContextPath() + "/pages" + "/cart" + "/cart.jsp");
}
```

改进点：输入框的数值校验。输入 “0” 与删除等效，不能输入负数。输入 “0” 在 `CartServlet` 中进行校验。负数输入使用 javascript 进行校验。

`CartServlet` 校验

```java
// 如果修改数量为0
if (count == 0) {
    request.getRequestDispatcher("/cartServlet?action=delete").forward(request, response);
} else {
    // 更新 Session 域中的 cart 对象并重新上传
    HttpSession session = request.getSession();
    Cart cart = (Cart) session.getAttribute("cart");
    cart.updateCount(id, count);
    session.removeAttribute("cart");
    session.setAttribute("cart", cart);

    // 重定向到购物车界面
    response.sendRedirect(request.getContextPath() + "/pages" + "/cart" + "/cart.jsp");
}
```

前端 js 校验

```javascript
if (count >= 0) {
    // 请求
    location.href = "http://localhost:8080/book/cartServlet?action=update&count=" + count + "&bookid=" + id
} else {
    alert("你他吗的！")
    return false
}
```

存在的 bug 及优化：

1.修改购物车数量时，购物车总金额计算错误。

错误原因：`Cart` 中 `updateCount` 逻辑错误。

2.

![image-20221101202524981](img/image-20221101202524981.png)

如果购物车已经空了，没必要显示上述信息。

解决方案：在 `cart.jsp` 中，根据 `Session` 域中 `items` 是否为空输出上述信息。

`cart.jsp`

```html
<c:if test="${not empty sessionScope.cart.items}">
    <div class="cart_info">
        <span class="cart_span">购物车中共有<span class="b_count">${sessionScope.cart.cartCount}</span>件商品</span>
        <span class="cart_span">总金额<span class="b_price">${sessionScope.cart.cartTotalPrice}</span>元</span>
        <span class="cart_span"><a href="cartServlet?action=clear">清空购物车</a></span>
        <span class="cart_span"><a href="pages/cart/checkout.jsp">去结账</a></span>
    </div>
</c:if>
```

3.删除的提示信息

解决方案：在 `cart.jsp` 中，给 “删除” 超链接绑定单击事件，提示用户进行重复确认。

4.清空购物车的提示信息

解决方案：给输入框绑定内容改变事件，若内容修改，首先征求用户确认，若用户取消，输入框的值应该等于原来的值。

```js
$(".countText").change(function () {
    // 提示用户是否确认输入
    var ok = confirm("您确定修改【 " + $(this).parent().parent().find("td:first").text() + "】数量为" + $(this).val() + "吗")
    if (!ok) {
        this.value = this.defaultValue
        return false
    }
```

# 20221102

## 一、书城第六阶段

### 1、订单模块

#### (1) 订单模块分析

![image-20221102101321344](img/image-20221102101321344.png)

#### (2) 创建数据库表

t_order 表和 t_orderitem 表。

#### (3) 创建数据模型 Order 和 OrderItem

`Order.java`

```java
public class Order {
    private String orderId; // 订单编号
    private Date createTime; // 创建时间
    private BigDecimal price; // 订单金额
    // 0 表示未发货 1 表示已发货 2 表示已签收
    private Integer status = 0;
    private Integer userId; // 用户编号
```

注意：使用包装类，可以接收 `null`。

`OrderItem.java`

```java
public class OrderItem {
    private Integer id; // 订单项编号
    private String name; // 商品名称
    private Integer count; // 商品数量
    private BigDecimal price; // 商品单价
    private BigDecimal totalPrice; // 商品总价
    private String orderId; // 订单编号
```

注意：创建的模型参数要和数据库中模型参数相同，使用 `BeanListHandler` 进行对象注入时更加方便。

#### (4) 编写订单 Dao 并测试

注意：日期类使用第三代日期类。

`OrderDao`

```java
public interface OrderDao {
    /**
     * 保存订单
     *
     * @param order 订单对象
     * @return
     */
    int saveOrder(Order order);

    /**
     * 查询全部订单
     * @return 订单列表
     */
    List<Order> queryOrders();

    /**
     * 改变订单状态
     *
     * @param orderId 订单编号
     * @param status  状态
     * @return
     */
    int changeOrderStatus(String orderId, int status);

    /**
     * 根据用户编号查询订单
     *
     * @param userId 用户编号
     * @return 订单对象
     */
    List<Order> QueryOrdersByUserId(Integer userId);
}
```

`OrderDaoImpl.java`

```java
public class OrderDaoImpl extends BasicDao<Order> implements OrderDao{
    @Override
    public int saveOrder(Order order) {
        String sql = "insert into t_order (id, create_time, price, status, user_id) values (?, ?, ?, ?, ?)";
        return update(sql, order.getId(), order.getCreate_time(), order.getPrice(), order.getStatus(), order.getUser_id());
    }

    @Override
    public List<Order> queryOrders() {
        String sql = "select * from t_order";
        return queryMulti(sql, Order.class);
    }

    @Override
    public int changeOrderStatus(String orderId, int status) {
        String sql = "update t_order set status = ? where id = ?";
        return update(sql, status, orderId);
    }

    @Override
    public List<Order> QueryOrdersByUserId(Integer userId) {
        String sql = "select * from t_order where user_id = ?";
        return queryMulti(sql, Order.class, userId);
    }
}
```

#### (5) 编写订单项 Dao 并测试

`OrderItemDao.java`

```java
public interface OrderItemDao {
    /**
     * 保存订单项
     * @param orderItem 订单项对象
     * @return 受影响的行数
     */
    int saveOrderItem(OrderItem orderItem);

    /**
     * 通过订单号查询订单明细
     * @param orderId 订单号
     * @return 订单项列表
     */
    List<OrderItem> queryOrderItemsByOrderId(String orderId);
}
```

`OrderItemDaoImpl.java`

```java
public class OrderItemDaoImpl extends BasicDao<OrderItem> implements OrderItemDao {
    @Override
    public int saveOrderItem(OrderItem orderItem) {
        String sql = "insert into t_orderitem (id, name, count, price, total_price, order_id) values (?, ?, ?, ?, ?, ?)";
        return update(sql, orderItem.getId(), orderItem.getName(), orderItem.getCount(), orderItem.getPrice(), orderItem.getTotal_price(), orderItem.getOrder_id());
    }

    @Override
    public List<OrderItem> queryOrderItemsByOrderId(String orderId) {
        String sql = "select * from t_orderitem where order_id = ?";
        return queryMulti(sql, OrderItem.class, orderId);
    }
}
```

#### (6) 编写 `OrderService.java` 并测试

`OrderService.java`

> 创建订单时，订单编号由用户编号和当前时间的哈希值拼接而成。

```java
public interface OrderService {
    /**
     * 将购物车中商品结账，形成订单
     * @param cart 购物车对象
     * @param userId 用户编号
     */
    void createOrder(Cart cart, Integer userId);

    /**
     * 展示所有订单
     * @return 订单列表
     */
    List<Order> showAllOrders();

    /**
     * 发货
     * @param orderId 订单编号
     */
    void sendOrder(String orderId);

    /**
     * 查询订单明细
     * @param orderId 订单编号
     * @return 订单项列表
     */
    List<OrderItem> showOrderDetail(String orderId);

    /**
     * 查看某用户的订单
     * @param userId 用户 id
     * @return 用户订单列表
     */
    List<Order> showMyOrders(Integer userId);

    /**
     * 确认收货
     * @param orderId 订单编号
     */
    void receiveOrder(String orderId);
}
```

`OrderServiceImpl.java`

```java
public class OrderServiceImpl implements OrderService {
    OrderDao orderDao = new OrderDaoImpl();
    OrderItemDao orderItemDao = new OrderItemDaoImpl();

    @Override
    public void createOrder(Cart cart, Integer userId) {
        // 创建 Order 对象并保存到数据库
        Order order = new Order(userId.toString() + Integer.valueOf(LocalDateTime.now().hashCode()).toString(), LocalDateTime.now(), cart.getCartTotalPrice(), 0, userId);
        orderDao.saveOrder(order);

        // 将购物车中的商品项封装成为 OrderItem 项并保存到数据库
        for (CartItem item : cart.getItems().values()) {
            OrderItem orderItem = new OrderItem(null, item.getName(), item.getCount(), item.getPrice(), item.getTotalPrice(), order.getId());
            orderItemDao.saveOrderItem(orderItem);
        }
    }

    @Override
    public List<Order> showAllOrders() {
        return orderDao.queryOrders();
    }

    @Override
    public void sendOrder(String orderId) {
        // 将指定编号的订单状态修改为1：已发货
        orderDao.changeOrderStatus(orderId, Order.SENT);
    }

    @Override
    public List<OrderItem> showOrderDetail(String orderId) {
        return orderItemDao.queryOrderItemsByOrderId(orderId);
    }

    @Override
    public List<Order> showMyOrders(Integer userId) {
        return orderDao.QueryOrdersByUserId(userId);
    }

    @Override
    public void receiveOrder(String orderId) {
        orderDao.changeOrderStatus(orderId, Order.RECEIVED);
    }
}
```

#### (7) 编写 `OrderServlet` 并与前端页面联调

##### a. 结账功能

在 `cart.jsp` 中，点击 “去结账” 进行结算。结算之前，首先获取用户确认，接着进行用户登录判断，如果用户没有登录，跳转至登录界面，此确认过程由前端 js 代码进行实现。校验完成，跳转至 `OrderServlet.java`，其 `createOrder()` 方法创建订单并在结束后跳至结账成功界面。

`cart.jsp`

```html
<span class="cart_span"><a href="cartServlet?action=clear" class="clearCartBtn">清空购物车</a></span>
```

```js
// 给结账超链接绑定单击事件
$("#checkOutBtn").click(function () {
    // 获取用户确认
    if (!confirm("您确定要结账吗？")) {
        return false
    }

    // 如果用户没有登录，取消提交并前往登录界面
    if (${empty sessionScope.user}) {
        location.href = "pages/user/login.jsp"
        return false
    }
})
```

`OrderServlet.java`

```java
protected void createOrder(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    // 获取创建订单所需的购物车 Cart 对象和用户 id
    HttpSession session = req.getSession();
    Cart cart = (Cart) session.getAttribute("cart");
    User user = (User) session.getAttribute("user");
    Integer id = user.getId();

    // 创建订单
    String orderId = orderService.createOrder(cart, id);

    // 重定向至结账成功界面
    resp.sendRedirect(req.getContextPath() +"/pages/cart/checkout.jsp?orderId=" + orderId);
}
```

优化及 bug 解决：

1.结账成功后，跳转至 `checkout.jsp` 页面，需要显示订单号。

![image-20221102163615982](img/image-20221102163615982.png)

分析：在创建 `Order` 对象那一层返回 `id` 属性即可。

解决方案：修改 `OrderService` 中 `CreateOrder()` 方法的返回值，将订单编号返回。

```java
String createOrder(Cart cart, Integer userId);
```

2.结账成功后，购物车应该清空。

解决方案：直接在 `OrderServlet` 中 `createOrder()` 方法中添加相应逻辑代码：

```java
// 删除 Session 域中的购物车
session.removeAttribute("cart");
```

##### b. 订单显示

![image-20221102171738566](img/订单显示.png)

在页面中点击 “我的订单” 后跳转至 `OrderServlet` 中 `showMyOrder()` 方法。没有登录时，不会显示 “我的订单”。在 `action_success.jsp` 中，对上方按钮进行控制：

```html
<c:if test="${not empty sessionScope.user.username}">
    <span>欢迎<span class="um_span">${sessionScope.user.username}</span>光临尚硅谷书城</span>
    <a href="userServlet?action=logout">注销</a>&nbsp;&nbsp;
    <a href="orderServlet?action=showMyOrder">我的订单</a>
</c:if>
```

`showMyOrder` 中选择将订单列表放入 `Session` 域中：

```java
protected void showMyOrder(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    // 调用 OrderService 方法获取订单列表
    User user = (User) req.getSession().getAttribute("user");
    List<Order> orders = orderService.showMyOrders(user.getId());

    // 将订单列表放入 session 域中
    req.getSession().setAttribute("userOrders", orders);

    // 请求重定向到订单显示页面
    resp.sendRedirect(req.getContextPath() + "/pages/order/order.jsp");
}
```

**查看详情**

![image-20221102193553141](img/image-20221102193553141.png)

点击 “查看详情” 后，需要提交该订单的 `id`，但这个参数并不在页面上显示，将参数放到一个并不会显示的元素中：

```html
<td style="display: none">${order.id}</td>
```

使用 js 绑定单击事件方式发送参数：

```js
// 给显示详情绑定单击事件
$("a.showOrderDetails").click(function () {
    // 获取 orderId
    var orderId = $(this).parent().parent().find("td:first").text();
    var totalPrice = $(this).parent().parent().find("td:eq(2)").text();

    // 请求
    location.href = "orderServlet?action=showOrderDetails&orderId=" + orderId + "&totalPrice=" + totalPrice

    return false
})
```

`OrderServlet` 中 `showOrderDetails()` 方法获取订单 `id` 并查询订单项列表，放入 `Session` 域中：

```java
protected void showOrderDetails(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    // 获取订单编号和订单金额
    String orderId = req.getParameter("orderId");
    String totalPrice = req.getParameter("totalPrice");

    // 查询订单商品项列表
    List<OrderItem> orderItems = orderService.showOrderDetail(orderId);

    // 将商品项列表放入 Session 域
    req.getSession().setAttribute("orderItems", orderItems);
    req.getSession().setAttribute("orderTotalPrice", totalPrice);

    // 请求重定向到订单详情页面
    resp.sendRedirect(req.getContextPath() + "/pages/order/OrderDetails.jsp");
}
```

`OrderDetails.jsp` 显示订单项：

```html
<c:forEach items="${sessionScope.orderItems}" var="orderItem">
    <tr>
        <td>${orderItem.name}</td>
        <td>${orderItem.count}</td>
        <td>${orderItem.price}</td>
        <td>${orderItem.total_price}</td>
    </tr>
</c:forEach>
```

优化与 bug 解决：

1.订单日期的显示：日期显示到天，不要显示时间。未解决。

![image-20221102204113223](img/image-20221102204113223.png)

##### c. 订单管理

![image-20221102204956050](img/image-20221102204956050.png)

点击 ”订单管理“ 后，请求 `OrderServlet` 中 `showAllOrders()` 方法：

```html
<a href="orderServlet?action=showAllOrders">订单管理</a>
```

`OrderServlet` 查询所有订单并放入 `Session` 域中：

```java
protected void showAllOrders(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    // 查询所有订单
    List<Order> orders = orderService.showAllOrders();

    // 放入 Session 域
    req.getSession().setAttribute("orders", orders);

    // 重定向到订单管理页面
    resp.sendRedirect(req.getContextPath() + "/pages/manager/order_manager.jsp");
}
```

`order_manager.jsp` 进行显示，显示详情功能依然使用用户显示订单详情方式解决：

```html
<c:forEach items="${sessionScope.orders}" var="order">
    <tr>
        <td style="display: none">${order.id}</td>
        <td>${order.create_time}</td>
        <td>${order.price}</td>
        <td><a href="#" class="showOrderDetails">查看详情</a></td>
        <td><a href="#">点击发货</a></td>
    </tr>
</c:forEach>
```

**发货功能**

`order_manager` 要将订单的 `id` 也发送给 `OrderServlet`，使用 js 的方式，`id` 属性并不会显示在页面中，将其放入隐藏域：

```html
<td style="display: none">${order.id}</td>
```

使用 js 给发送订单绑定单击事件：

```js
// 给 “点击发货” 按钮绑定单击事件
$("a.sendOrderBtn").click(function () {
    // 获取订单 id
    var id = $(this).parent().parent().find("td:first").text();

    // 请求
    location.href = "orderServlet?action=sendOrder&orderId=" + id

    return false
})
```

`OrderServlet.java` 中 `sendOrder()`：

```java
protected void sendOrder(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    // 获取订单 id
    String orderId = req.getParameter("orderId");

    // 修改订单状态
    orderService.sendOrder(orderId);

    // 重定向到订单管理界面
    resp.sendRedirect(req.getContextPath() + "/pages/manager/order_manager.jsp");
}
```

优化与 bug 解决：

1.点击发货后，订单状态不会实时更新：

![image-20221103082901199](img/image-20221103082901199.png)

产生原因：点击 “发货” 后，只修改了数据库中订单的状态，没有修改 `Session` 域中订单的状态。

解决措施：重新查询订单并放入 Session 域。

```java
protected void sendOrder(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    // 获取订单 id
    String orderId = req.getParameter("orderId");

    // 修改订单状态
    orderService.sendOrder(orderId);
    
    // 查询所有订单
    List<Order> orders = orderService.showAllOrders();

    // 放入 Session 域
    req.getSession().setAttribute("orders", orders);

    // 重定向到订单管理界面
    resp.sendRedirect(req.getContextPath() + "/pages/manager/order_manager.jsp");
}
```

# 20221103

## 一、Filter 过滤器

### 1、概念

JavaWeb 三大组件之一。是接口。主要作用是拦截/过滤请求。

原理：服务器在接收到请求时，将请求进行过滤，控制资源访问。

![image-20221103092344305](img/image-20221103092344305.png)

### 2、使用案例

在进行权限控制时，jsp 页面中可以根据 `Session` 域中是否有用户对象进行控制。但其他静态资源无法使用这种方式判断。

### 3、filter 生命周期

构造器方法

`init()` 方法

​	前两步在 web 工程启动时执行。

`doFilter()` 方法

​	拦截到请求就会执行。

`destroy()` 方法

​	停止 web 工程时执行。

### 4、`FilterConfig` 类

Filter 过滤器配置类。

![image-20221103100027353](img/image-20221103100027353.png)

![image-20221103100038273](img/image-20221103100038273.png)

### 5、`FilterChain` 过滤器链类

将多个过滤器向一个链条一样连接起来。

![image-20221103102106952](img/image-20221103102106952.png)

`web.xml` 中配置的先后顺序决定了过滤器在链中的顺序。

![image-20221103153620131](img/image-20221103153620131.png)

### 6、Filter 的拦截路径

- 精确匹配

  ![image-20221103152356982](img/image-20221103152356982.png)

- 目录匹配

  ![image-20221103152432578](img/image-20221103152432578.png)

- 后缀名匹配

  ![image-20221103152531380](img/image-20221103152531380.png)

## 二、书城第八阶段

### 1、使用 Filter 过滤器拦截 /pages/manager 全部请求，实现权限检查

创建 `ManagerFilter.java` 设置其拦截 `/pages/manager/*` 下的请求：

```xml
<filter>
    <filter-name>ManagerFilter</filter-name>
    <filter-class>com.zzy.filter.ManagerFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>ManagerFilter</filter-name>
    <url-pattern>/pages/manager/*</url-pattern>
</filter-mapping>
```

`ManagerFilter.java` ，获取 `Session` 域中用户对象的用户名，必须用户名为 `admin` 才允许访问：

```java
public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws ServletException, IOException {
    // 获取 Session 域中用户名对象
    HttpServletRequest httpServletRequest = (HttpServletRequest) request;
    User user = (User) httpServletRequest.getSession().getAttribute("user");

    // 判断用户是否登录
    if (user == null) { // 未登录
        // 跳转至登录界面
        request.getRequestDispatcher("/pages/user/login.jsp");
    } else { // 已登录
        if ("admin".equals(user.getUsername())) { // 是管理员登录
            // 放行
            chain.doFilter(request, response);
        }
    }
```

改进：

直接在地址栏请求 Servlet 程序不会拦截。

解决方案：

在 `web.xml` 中控制直接使用地址请求 Servlet 程序。

```xml
<url-pattern>/manager/bookServlet</url-pattern>
```

### 2、使用 Filter 和 ThreadLocal 组合管理事务

`ThreadLocal` 的作用是解决多线程数据安全问题。

基本原理：在线程中(局部变量)存放一个数据，只要还是这个线程，今后在任何地方都能访问这个数据。

![image-20221103155401931](img/image-20221103155401931.png)

项目中没有使用事务，并且有一个操作需要操作多次数据库的情形，这样会造成数据不安全，例如：

在 `OrderServiceImpl.java` 中，如果在将订单保存到数据库和将订单项保存到数据库之间出现异常，就会出现只保存订单，不保存订单项的情况。

```java
public String createOrder(Cart cart, Integer userId) {
    // 创建 Order 对象并保存到数据库
    Order order = new Order(userId.toString() + Integer.valueOf(LocalDateTime.now().hashCode()).toString(), LocalDateTime.now(), cart.getCartTotalPrice(), 0, userId);
    orderDao.saveOrder(order);
    
    int i = 12 / 0;

    // 将购物车中的商品项封装成为 OrderItem 项并保存到数据库
    for (CartItem item : cart.getItems().values()) {
        OrderItem orderItem = new OrderItem(null, item.getName(), item.getCount(), item.getPrice(), item.getTotalPrice(), order.getId());
        orderItemDao.saveOrderItem(orderItem);
    }
```

订单已经保存到数据库：

![image-20221103165132867](img/image-20221103165132867.png)

但没有关联的订单项：

![image-20221103165212673](img/image-20221103165212673.png)

需要使用事务机制。

jdbc 中的事务管理机制如下：

![image-20221103165337523](img/image-20221103165337523.png)

如果要使用事务，首先要确保的是，不可分割的操作序列使用同一个连接。

> 如果保存订单和保存订单项都不使用同一连接，谈何事务。

可以将连接放入 `ThreadLocal` 对象中，这样线程内部使用的都是同一个连接，即修改 `DruidUtils` 中获取连接的方式。

需要进行两次数据库操作的层为 `Service` 层，异常可以在 `Servlet` 层捕获，产生任何异常都会导致事务回滚。

`DruidUtils.java` 中，从 `ThreadLocal` 对象中获取连接，如果对象中没有连接，再获取连接并将该连接放入 `ThreadLocal` 对象中，同时设置手动提交事务：

```java
private static ThreadLocal<Connection> connection = new ThreadLocal<>();
```

```java
public static Connection getConnection() throws SQLException {
    Connection conn = connection.get();
    if (conn == null) { // 没有连接，说明第一次访问数据库
        conn = dataSource.getConnection();
        connection.set(conn);
        conn.setAutoCommit(false);
    }
    return conn;
}
```

事务的提交与回滚与关闭连接放到一起：

```java
public static void commitAndClose() {
    // 获取 ThreadLocal 对象中保存的连接
    Connection conn = connection.get();

    if (conn != null) { // 保存有连接
        try {
            // 提交事务
            conn.commit();
        } catch (SQLException e) {
            throw new RuntimeException(e);
        } finally {
            try {
                conn.close();
            } catch (SQLException e) {
                throw new RuntimeException(e);
            }
        }
    }

    // 将 ThreadLocal 中保存的连接移除
    connection.remove();
}
```

事务回滚与关闭连接代码也是类似。

`BasicDao.java`

连接关闭将不在此层实现，并且捕获的异常要抛出。

```java
public Object queryScalar(String sql, Object... parameters) throws SQLException {
    Object o; // 返回的标量

    Connection connection = null;

    // 获取连接
    connection = DruidUtils.getConnection();

    // 执行查询
    o = runner.query(connection, sql, new ScalarHandler<>(), parameters);

    // 返回结果
    return o;
}
```

`Service` 层异常继续向上抛。

`Servlet` 层捕获异常，并在出现异常时进行回滚，未出现异常时进行提交。

### 3、使用 Filter 过滤器给所有的 Service 方法加上 try-catch

Servlet 程序也是一种目标资源，Filter 过滤器中 `doFilter()` 方法便可以访问所有 Servlet 程序，将 Servlet 程序中的异常也统一向上抛，在 Filter 中进行 try-catch。

![image-20221104090156464](img/image-20221104090156464.png)

在 `web.xml` 中，将 `Transaction.java` 设置为所有请求进行拦截，所有请求都具有事务管理。

```xml
<filter-mapping>
    <filter-name>TransactionFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

`Transaction.java` 中：

```java
public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws ServletException, IOException {
    try {
        chain.doFilter(request, response);
        DruidUtils.commitAndClose();
    } catch (Exception e) {
        DruidUtils.rollbackAndClose();
    } 
}
```

将所有 Servlet 中异常的捕获该为抛出，这样代码也更加简洁。

### 4、Tomcat 展示友好错误界面

在 `web.xml` 中配置错误信息展示界面：

```xml
<error-page>
    <error-code>500</error-code>
    <location>/pages/error/500error.jsp</location>
</error-page>
```

`error500.jsp`：服务器内部错误页面。

在 `Transaction.java` 中将异常再抛给 Tomcat。

# 20221104

## 一、JSON

![image-20221104095138002](img/image-20221104095138002.png)

轻量级：和 xml 相比。数据交换：**客户端**和**服务器**之间。

### 1、JSON 在 js(客户端) 中的使用

#### (1) 定义

创建 json 对象。

```json
// json的定义
var jsonObj = {
    "name": "zzy",
    "age": 22,
    "gil": [{
            "name": "myj",
            "age": 22
        }, {
            "name": "ljj",
            "age": 22
        }, {
            "name": "cs",
            "age": 22
        }]
}
```

#### (2) 访问

json 是一个对象，其属性使用 `.` 的方式访问。

```js
// json的访问
alert(jsonObj.name)
alert(jsonObj.gil[1].name)
```

#### (3) js 中 json 常用方法

json 存在有两种形式：

​	对象形式(JSON对象)：操作数据。

​	字符串形式(JSON字符串)：数据交换。

`JSON.stringify()`：对象 --> 字符串

```js
// json对象转字符串
var jsonString = JSON.stringify(jsonObj);
console.log(jsonString)
```

`JSON.parse()`：字符串 --> 对象

```js
// json字符串转json对象
var jsonObj1 = JSON.parse(jsonString);
console.log(jsonObj1)
```

### 2、JSON 在 java(服务器) 中的使用

首先需要导包：谷歌 gson 包。

```xml
<!-- https://mvnrepository.com/artifact/com.google.code.gson/gson -->
<dependency>
    <groupId>com.google.code.gson</groupId>
    <artifactId>gson</artifactId>
    <version>2.8.9</version>
</dependency>
```

#### (1) JavaBean 和 JSON 相互转换

JavaBean --> Json：使用 `gson.toJson()`，将 JavaBean 对象转化为 JSON 字符串。

```java
Person person = new Person(24, "田所浩二");

Gson gson = new Gson();
String personJSON = gson.toJson(person);
```

JSON 字符串 --> JavaBean：使用 `gson.from(json字符串, javaBean.class)`：

```java
String jsonString = new Gson().toJson(new Person(24, "田所浩二"));

Person person = new Gson().fromJson(jsonString, Person.class);
System.out.println(person);
```

#### (2) List 集合和 JSON 相互转换

List 集合会转换为 JSON 列表。

List --> JSON：

```java
personList.add(new Person(24, "田所浩二"));
personList.add(new Person(24, "德川裕泰"));

String json = new Gson().toJson(personList);
```

JSON String --> List：

如果 JSON String 中存放的是 JSON 列表，转换而成的列表每个元素是 Map，如下图所示：

```java
List<Person> personList = new ArrayList<>();
personList.add(new Person(24, "田所浩二"));
personList.add(new Person(24, "德川裕泰"));
String json = new Gson().toJson(personList);

List list = new Gson().fromJson(json, personList.getClass());
System.out.println(list);
```

```
[{age=24.0, name=田所浩二}, {age=24.0, name=德川裕泰}]
```

使用步骤：

编写类继承 `TypeToken<>` 类，泛型中填写希望 `fromJson()` 转换得到的类型：

```java
public class PersonListType extends TypeToken<List<Person>> {
}
```

使用 `fromJson()`时，按如下方式使用：

```java
List list = new Gson().fromJson(json, new PersonListType().getType());
```

#### (3) Map 和 JSON 相互转换

Map --> JsonString：

```java
Map<Integer, Person> map = new HashMap<>();
map.put(1, new Person(24, "田所浩二"));
map.put(2, new Person(24, "德川"));

String jsonString = new Gson().toJson(map);
```

JsonString --> Map：

与 List 转换类似。

```java
Map<Integer, Person> map1 = new Gson().fromJson(jsonString, new PersonMapType().getType());
```

使用匿名内部类简化：

```java
List<Person> list = new Gson().fromJson(json, new TypeToken<Map<Integer, Person>>() {}.getType());
```

# 20221105

## 一、AJAX请求

### 1、定义

![image-20221104114922233](img/image-20221104114922233.png)

![image-20221105082819185](img/image-20221105082819185.png)

局部更新页面的技术。

### 2、原生 AJAX 使用

![image-20221105083049523](img/image-20221105083049523.png)

前端页面，使用步骤：创建请求，初始化请求，发送请求，接收响应数据。

```js
//           1、我们首先要创建XMLHttpRequest
            var xmlHttpRequest = new XMLHttpRequest()

//              2、调用open方法设置请求参数
            xmlHttpRequest.open("GET", "http://localhost:8080/json/ajaxServlet?action=doAjax", true)

//              4、在send方法前绑定onreadystatechange事件，处理请求完成后的操作。
            xmlHttpRequest.onreadystatechange = function () {
               if (xmlHttpRequest.readyState == 4 && xmlHttpRequest.status == 200) {
                  var jsonObj = JSON.parse(xmlHttpRequest.responseText);
                  document.getElementById("div01").innerText = "年龄：" + jsonObj.age + "，姓名：" + jsonObj.name
               }
            }

//              3、调用send方法发送请求
            xmlHttpRequest.send()
```

后端：

```java
protected void doAjax(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    System.out.println("接收到了 ajax 请求");

    Person person = new Person(24, "田所浩二");
    String personJsonString = new Gson().toJson(person);

    response.getWriter().write(personJsonString);
}
```

### 3、AJAX 说明

- 局部更新：
  - 浏览器地址不变
  - 只是使用服务器发来的数据更新局部内容

- 异步请求：

  浏览器向服务器请求数据时并不是干等着，而是将请求交由代理对象 `XMLHttpRequest` 取处理，在此期间，仍然可以进行其他操作。

### 4、JQuery 中使用 AJAX

#### (1) $.ajax

在 JQuery 中使用 $.ajax 发送异步请求，以 json 方式填入各种参数。

![image-20221105103918253](img/image-20221105103918253.png)

使用案例：

```js
// ajax请求
$("#ajaxBtn").click(function(){
   
   $.ajax({
      url:"http://localhost:8080/json/ajaxServlet",
      // 请求参数
      data:"action=JQueryAjax",
      // 请求方法
      type:"GET",
      // 请求成功的函数
      success: function (data) {
         $("#msg_div").html("年龄：" + data.age + "，姓名：" + data.name)
      },
      // 响应的数据类型：text/xml/json
      dataType: "json"
   })
```

#### (2) $.get 和 $.post

在 $.ajax 基础上进一步封装，像函数一样进行异步请求发送。

![image-20221105104249260](img/image-20221105104249260.png)

$.get 使用案例：

```js
$.get("http://localhost:8080/json/ajaxServlet", "action=JQueryAjax", function (data) {
   $("#msg_div").html("年龄：" + data.age + "，姓名：" + data.name)
}, "json")
```

$.post 使用案例：

```js
$("#postBtn").click(function(){
   $.post("http://localhost:8080/json/ajaxServlet", "action=JQueryAjax", function (data) {
      $("#msg_div").html("年龄：" + data.age + "，姓名：" + data.name)
   }, "json")
   
});
```

#### (3) $.getJSON

在 $.get 基础上进一步封装，获取 JSON 格式的数据。

使用案例：

```js
$("#getJSONBtn").click(function(){
   $.getJSON("http://localhost:8080/json/ajaxServlet", "action=JQueryAjax", function (data) {
      $("#msg_div").html("年龄：" + data.age + "，姓名：" + data.name)
   })

});
```

#### (4) `serialize()` 方法

表单序列化方法，获取表单中所有内容，以 name=value&name=value 方式进行拼接。

使用案例：

```js
var formData = $("#form01").serialize();
```

## 二、书城第九阶段

### 1、使用 AJAX 验证用户名是否可用

在注册界面中，用户名首先发送给服务器，检查是否可用，并输出提示信息。

![image-20221105112219036](img/image-20221105112219036.png)

总体流程：

![image-20221105113133165](img/image-20221105113133165.png)

具体实现：

`regist.jsp` 中，在用户名合法后，向服务器发送用户名：

```js
	 // 3.验证
    if (!usernameRegExp.test(usernameVal)) {
        // 4.提示结果
        $(".errorMsg").text("用户名不合法！")
        return
    }

    // 向服务器发送 username
    $.post("userServlet", {"action" : "usernameVerify", "username" : usernameVal}, function (data) {

    $(".errorMsg").text("true" == data ? "用户名已存在" : "用户名可用")
    }, "text")
```

`userServlet.java` 中 `usernameVerify()` 方法，获取用户名参数后进行存在验证，并向客户端回传布尔字符串：

```java
protected void usernameVerify(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException, SQLException {
    // 获取发送到服务器的用户名
    String username = request.getParameter("username");

    // 进行验证
    Boolean usernameExists = userService.usernameExists(username);


    // 向客户端发送响应数据
    response.getWriter().write(usernameExists.toString());
}
```

`regist.jsp` 中，根据服务器传来的 `true/false` 输出相应信息。

# 20221106

## 一、i18n 国际化

国际化三元素：

![image-20221106135553837](img/image-20221106135553837.png)

# 完结

![image-20221106142005376](img/image-20221106142005376.png)
