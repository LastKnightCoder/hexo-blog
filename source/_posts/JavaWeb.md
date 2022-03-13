---
title: Java Web笔记
categories: 
	- Java Web
tags:
	- Java
	- Web
keywords: JavaWeb知识笔记
description:
   有关Java Web的简略笔记，包括Tomcat Servlet JSP Cookie Session等等
date: 2019-08-20
---

## Tomcat

### Web服务器软件

- 服务器：安装了服务器软件的计算机
- 服务器软件：接收用户请求，处理请求，做出响应
- Web服务器软件：用户通过浏览器进行访问我们部署的项目
  - webLogic：oracle公司，大型JavaEE服务器，收费，支持所有的JavaEE规范
  - webSphere：IBM公司，大型JavaEE服务器，收费，支持所有的JavaEE规范
  - JBOSS：JBOSS公司，大型JavaEE服务器，收费，支持所有的JavaEE规范
  - Tomcat：Apache基金组织，中小型JavaEE服务器，仅仅支持少量的JavaEE规范，开源的，免费的

### 下载、安装、卸载

下载：tomcat.apache.org(Tomcat 8)

安装：解压压缩包即可(路径不要含中文)

卸载：删除文件夹

```
bin             可执行文件
conf            配置文件
lib             依赖jar包
logs            日志文件
temp            临时文件
webapps         存放web项目的
work            运行时数据
LICENSE
NOTICE
RELEASE-NOTES
RUNNING.txt
```

启动

1. 双击bin/startup.bat
2. 访问：127.0.0.1:8080，有页面说明访问成功(127.0.0.1可写为localhost)

<center>
    <img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/1571488794890.png"/>
</center>

启动遇到的问题

1. 黑窗口一闪而过

   - 原因：没有配置JAVA_HOME

     1. 在环境变量中新建JAVA_HOME，值为jdk的路径，不包括bin目录

     2. 然后将PATH中的…/bin(jdk的路径)改为%JAVA_HOME%/bin

2. 启动报错

   - 原因：端口被占用，两种方法

     1. 找到占用8080端口的进程杀死

     2. 修改自身的端口号

        - conf/server.xml文件

        - 修改port
        - 一般会将tomcat的默认端口号修改为80(浏览器默认端口号)

关闭

1. 正常关闭
   - shutdown.bat
   - Ctrl + C
2. 强制关闭

### 部署项目

1. 直接将项目(这里假设为hello)放到webapps文件夹中即可
   - \*/hello：项目的访问路径，虚拟路径
   - 简化部署：打包为war包，将war包复制到webapps下，war包会自动解压缩
   - 缺点：必须拷贝到webapps中
2. config/server.xml

```xml
<Host ...>
    ...
    <Context docBase="项目路径" path="虚拟路径" />
</Host>
```

- 缺点：不安全，弄坏配置文件

3. /conf/Catalina/localhost
   1. 创建一个文件xxx.xml(任意名称)
   2. 编写\<Context docBase="项目路径" />
   3. 虚拟目录就是xml文件的名称

Java动态项目的目录结构

```
根目录
	WEB-INF
		web.xml：web项目的核心配置文件
		classes：放置字节码文件的目录
		lib：放置依赖的jar包
```

### Tomcat集成到IDEA中

Run –> Edit Configurations -> Defaults -> Tomcat Server -> Local -> Configure -> Tomcat安装目录

## Servlet(server applet)

### 概念

Servlet就是一个接口，定义了Java类被浏览器访问到(tomcat)识别的规则。

### 快速入门

1. 创建JavaEE项目
2. 定义一个类实现Servlet接口
3. 实现接口中的抽象方法
4. 配置Servlet

```java
package web;

import javax.servlet.*;
import java.io.IOException;

public class ServletDemo01 implements Servlet {
    @Override
    public void init(ServletConfig servletConfig) throws ServletException {

    }

    @Override
    public ServletConfig getServletConfig() {
        return null;
    }

    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        //只写了这一行
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

配置Servlet(web.xml)

```xml
<!--Servlet配置-->
<servlet>
    <servlet-name>demo01</servlet-name>
    <servlet-class>web.ServletDemo01</servlet-class>
</servlet>

<servlet-mapping>
    <servlet-name>demo01</servlet-name>
    <url-pattern>/demo01</url-pattern>
</servlet-mapping>
```

### Servlet执行原理

1. 解析URL请求路径，获取Servlet的资源路径
2. 查找web.xml中是否有对应的\<url-pattern>标签内容
3. 查找\<servlet-class>全类名
4. tomcat将字节码文件加载进内存，并且创建其名称
5. 调用其方法

### Servlet生命周期方法

init

- 在Servlet创建时执行，只会执行一次
- 默认情况第一次被访问时创建Servlet
  - 配置\<servlet>下的\<load-on-startup>可修改Servlet创建时机
    - 负数，第一次被访问时创建
    - 0或正数，服务器启动时创建
- Servlet是单例的
  - 多个用户同时访问时，可能存在线程安全问题
  - 解决：尽量不要在Servlet中定义成员变量，定义了成员变量，也不要进行赋值

service

- 提供服务的方法，每一次Servlet访问时执行

destroy

- Servlet被杀死时被执行(服务器被正常关闭时)
- 在Servlet被销毁之前执行

getServletConfig(了解)

- 获取Servlet的配置对象

getServletInfo(了解)

- 获取Servlet的信息(版本、作者…)

### Servlet3.0注解配置

Servlet3.0

- 支持注解配置

1. 创建JavaEE项目，勾选servlet3.0以上版本，可以不勾选web.xml
2. 定义类实现Servlet接口，实现方法
3. 在类上使用@WebServlet注解

@WebServlet("资源路径")

### Servlet体系结构

Servlet –> GenericServlet –> HttpServlet

- GenericServlet：将Servlet接口除service()外进行了空实现，只对service()进行了抽象
- HttpServlet：对http协议封装，简化操作
  - doGet()
  - doPost()

### Servlet url_pattern配置

可以定义多个访问路径：@WebServlet({"/d4","/dd4","/ddd4"})

1. /xxx
2. /xxx/xxx(目录结构)
   - /xxx/\*(\*是通配符，优先级较低)
3. \*.do(\*表示任意，.do代表后缀名，如/demo4.do)

## HTTP

规定请求消息和响应消息的格式。

特点：

1. 基于TCP/IP的高级协议
2. 默认端口号：80
3. 基于请求响应模型，一次请求对应一次响应
4. 无状态的：每次请求相互独立

历史版本：

- 1.0：每一次请求响应会建立新的连接
- 1.1：复用连接

### 请求消息数据格式

1. 请求行
2. 请求头
3. 请求空行
4. 请求体

请求方式(七种，只介绍常见两种)：

- GET：
  - 参数在请求行中，在url后
  - 请求的url长度有限制
  - 不太安全
- POST
  - 参数在请求体中
  - 请求的url长度没有限制
  - 相对安全

请求头：

- Host：主机
- User-Agent：使用的浏览器版本信息
- Accept：可以接受什么格式
- Accept-Language：可接受的语言
- Referer：告诉服务器我从哪里来，作用
  1. 防盗链
  2. 统计信息
- Connection：keep-alive(连接不会断开，可复用)

请求正文：

- 封装POST请求消息的请求参数的

### Request

Request和Response原理：

1. tomcat根据url创建对应的Servlet对象
2. tomcat服务器会创建request和response对象，request对象中封装的是请求消息数据
3. tomcat将request和response传递给service方法，并调用service方法
4. 程序员通过request对象获取请求消息数据，通过response对象设置响应数据
5. 服务器在做出响应之前，会用response对象中拿程序员设置的响应消息数据

Request继承结构：

ServletRequest(接口) –> HttpServletRequest(接口) –> RequestFacade(类，Tomcat编写)

#### 获取请求消息数据

1. 获取请求行

格式：GET 虚拟路径/Servlet路径?参数 HTTP/1.1

- 获取请求方式：getMethod

- **获取虚拟目录：getContextPath**

- 获取Servlet路径：getServletPath

- 获取请求参数：getQueryString

- **获取URI：getURI**

- 获取URL：getURL

- 获得协议及版本：getProtocol

- 获取客户机的IP地址：getRemoteAddr

> URL < URI

2. 获取请求头

- 通过请求头的名称获取请求头的值：**getHeader(String name)**
- 获取所有的请求头名称：Enumeration\<String\> getHeaderNames()

3. 获取请求体
   1. 获取流数据
      1. 字符流：BufferedReader getReader()
      2. 字节流：ServletInputStream getInputStream()
   2. 再从流对象中

#### 其他功能

- 获取请求方法通用方式(get和post都可以)
  1. **String getParameter(String name)**：根据参数名称获取参数值
  2. String[] getParameterValues(String name)：根据参数名称获取参数值的数组(多用于复选框)
  3. Enumeration getParameterNames()：获取所有请求的参数名称
  4. **Map\<String, String[]\> getParameterMap**：获取所有参数的Map集合

> 中文乱码问题：
>
> get：tomcat 8已经解决
>
> post：设置流的编码
>
> - resquest.setCharacterEncoding(“utf-8”)

- 请求转发：一种在服务器内部的资源跳转的方式

  1. 通过request对象获取请求转发器对象：RequestDispatcher getRequestDispatcher(String path)
  2. 使用RequestDispatcher进行转发：forward(ServletRequest request, ServletResponse response)

  > 特点：
  >
  > 1. 浏览器地址栏路径没有发生变化
  > 2. 只能转发到当前服务器内部资源中
  > 3. 转发是一次请求

- 共享数据
  - 域对象：一个有作用范围的对象，可以在范围内共享对象
  - request域：代表一次请求的范围，一般用于请求转发的多个资源中共享数据
  - 方法
    1. setAttribute(String name, Obkect obj)：存储数据
    2. Object getAttribute(String name)：通过键获取值
    3. void removeAttribute(String name)：删除键值对

- 获取ServletContext

  - ServletContext getServletContext()

> 开发步骤：
>
> 1. 创建项目，导入html，配置文件，jar包
> 2. 创建数据库环境，User表
> 3. 创建实体类User
> 4. 创建UserDao，操作数据库中User表的类

#### BeanUtils

简化数据封装

```java
Map<String String[]) map = req.getParameterMap();
User user = new User();
BeanUtils.populate(loginUser, map);
```

### 响应消息数据格式

1. 响应行
2. 响应头
3. 响应空行
4. 响应体

响应行：

1. 协议以及版本：HTTP/1.1

2. 响应状态码：200

   - 1xx：服务器接客户端消息但是没有接收完成，一段时间后服务器向客户端询问是否还有数据发送

   - 2xx：成功。
     - 200

   - 3xx：重定向。
     - 302(重定向)

     - 304(访问缓存)

   - 4xx：客户端错误。
     - 404(请求路径没有对应的资源)

     - 405(请求方式没有对应的doXxx方法)

   - 5xx：服务器端错误。
     - 500(服务器内部出现异常)

3. 状态码的描述：OK

响应头：

1. 格式
   - 名称：值
2. 常见响应头
   - Content-Type：服务器告诉客户端响应体数据格式以及编码格式
   - Content-disposotion：服务器告诉客户端以什么格式打开我的响应体
     - in-line：默认值，在当前页面打开
     - attachment：以附件形式打开(文件下载)

响应体：

- 真实传输的数据

### Response

功能：设置响应消息的

1. 设置响应行
   - 设置状态码：setStatus(int sc)
2. 设置响应头：
   - setHeader(String name, String value)
3. 设置响应体
   1. 获取输出流
      1. 字符输出流：PrintWriter getWriter()
      2. 字节输出流：ServletOutputStream getOutputStream()
   2. 使用输出流，将数据输出到客户端浏览器

> 重定向：
>
> 1. 设置状态码302
> 2. 设置响应头location
>
> 简单重定向方法：
>
> - sendRedirect(String url)
>
> 重定向的特点：
>
> 1. 地址栏发生变化
> 2. 可以访问其他站点(服务器)的资源
> 3. 两次请求(不可用Request域对象共享数据)

> 路径写法：
>
> - 相对路径：当前资源与访问资源的路径关系(./index.html = index.html)
> - 绝对路径：(/responseDemo2)
>   - 给客户端浏览器使用(a标签，表单，重定向等)：需要加虚拟目录(项目访问路径)
>     - req.getContextPath：动态获取虚拟目录
>   - 给服务器端使用：不需要加虚拟目录(如转发)

> 输出字符/字节：
>
> - 乱码问题：浏览器默认以GBK解码(与操作系统有关，Windows在中文环境下是GBK)
>
> ```java
> response.setHeader("content-type", "text/html;charset=utf-8");
> //简便写法
> response.setContentType("text/html;charset=utf-8");
> ```

> 验证码：
>
> ```java
> int width = 100;
> int height = 50;
> //设置图片的宽高和类型
> BufferedImage image = new BufferedImage(width, height, BufferedImage.TYPE_INT_RGB);
> 
> //美化图片
> //获取画图对象
> Graphics g = image.getGraphics();
> //设置背景色
> g.setColor(Color.PINK);
> g.fileRect(0, 0, width, height);
> 
> //设置边框色以及绘制
> g.setColor(Color.BLUE);
> g.drawRect(0,0 width - 1, height - 1);
> 
> //获取随机字符并绘制
> String str = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789";
> Random ran = new Random();
> for (int i = 1; i <= 4; i++) {
>     index = ran.nextInt(str.length());
>     char ch = str.charAt(index);
>     g.drawString(ch+"", 20*i, 25);
> }
> 
> //设置干扰线
> g.setColor(Color.GREEN);
> for (int i = 0; i < 10; i++) {
>     int x1 = ran.nextInt(width);
>     int x2 = ran.nextInt(width);
>     int y1 = ran.nextInt(height);
>     int y2 = ran.nextIne(height);
>     g.drawLine(x1, y1, x2, y2);
> }
> 
> //绘制到浏览器中
> ImageIO.writer(image, "jpg", response.getOutputStream());
> ```

### ServletContext

概念：代表整个Web应用，可以和程序的容器来通信。

获取：

1. request对象获取：request.getServletContext()
2. HttpServlet的方法：this.getServletContext()

功能：

1. 获取MIME类型

   - MIME：互联网通信过程中定义的一种文件数据类型

     - 格式：大类型/小类型(text/html image/jpeg)

   - String getMimeType(String file)

     - ```java
       String fileName = "a.jpg";
       String mimeType = servletContext.getMimeType(fileName);
       ```

2. 域对象：共享数据

   - 方法：
     - setAttribute(String name, Object value)
     - getAttribute(String name)
     - removeAttribute(String name)
   - 范围：
     - 所有用户所有请求的数据

3. 获取文件的真实路径(服务器路径)

   - 方法：
     - String getRealPath(String path)

     - ```java
       //web目录下资源访问
       getRealPath("/a.txt");
       //WEB-INf目录下资源访问
       getRealPath("/WEB-INF/b.txt");
       //src目录下资源访问
       getReadPath("/WEB-INf/classes/c.txt");
       ```

## Cookie

> 会话：
>
> - 概念：一次会话(浏览器第一次发生请求，直到一方断开)包含多次请求和响应
>
> - 功能：一次会话的多次请求间共享数据
>
> - 方式：
>   1. 客户端：Cookie
>   2. 服务器：Session

将数据保存到客户端，使用步骤：

1. 创建Cookie对象，绑定数据
   - new Cookie(String name, String value)
2. 发送Cookie对象
   - response.addCookie(Cookie cookie)
3. 获取Cookie,得到数据
   - Cookie[] request.getCookies()

```java
package web;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet("/cookieDemo")
public class CookieDemo extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        Cookie c = new Cookie("msg", "hello");

        response.addCookie(c);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}
```

```java
package web;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet("/cookieDemo2")
public class CookieDemo2 extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        Cookie[] cookies = request.getCookies();
        if (cookies != null) {
            for (Cookie cookie : cookies) {
                String name = cookie.getName();
                String value = cookie.getValue();
                System.out.println(name + ": " + value);
            }
        }
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}
```

实现原理：

response发送响应头：set-cookie:msg=hello

request发送请求头：Cookie:mas=hello

细节：

1. 一次可不可以发送多个Cookie
   - 可以
2. Cookie能在浏览器中保存多长的时间
   - 默认：浏览器关闭时Cookie数据被销毁
   - 持久化存储：
     - cookie.setMaxAge(int seconds)
       - 正数：将Cookie数据写到硬盘的文件中，数值表示存活时间
       - 负数：默认值
       - 0：删除Cookie信息
3. Cookie能不能存中文
   - Tomcat 8之前不能直接存储中文信息，Tomcat 8之后可以
4. Cookie共享问题
   - 一个Tomcat服务器部署了多个web项目，在这些项目中cookie能不能共享
     - 默认不能共享
     - setPath(String path)：设置Cookie的获取范围，默认情况设置当前的虚拟目录
   - 不同的Tomcat服务器间Cookie共享问题
     - setDomain(String path)：如果一级域名相同，那么多个服务器之间Cookie可以共享

特点：

1. Cookie存储数据在浏览器

2. 浏览器对于单个Cookie的大小有限制(4KB)，数目也有限制(20个)


作用：

- 一般用于存在少量不太敏感的数据
- 在不登录的情况下，完成服务器对客户端的身份识别

案例：记住上一次登录时间

```java
package web;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.net.URLDecoder;
import java.net.URLEncoder;
import java.text.SimpleDateFormat;
import java.util.Date;

@WebServlet("/cookieTest")
public class CookieTest extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html;charset=utf-8");
        Cookie[] cookies = request.getCookies();
        boolean flag = false;

        if (cookies != null && cookies.length > 0) {
            for (Cookie cookie: cookies) {
                String name = cookie.getName();
                if ("lastTime".equals(name)) {
                    flag = true;

                    String time = cookie.getValue();
                    time = URLDecoder.decode(time, "utf-8");

                    Date date = new Date();
                    SimpleDateFormat sdf = new SimpleDateFormat("yyyy:MM:dd HH:mm:ss");
                    String str_date = sdf.format(date);
                    str_date = URLEncoder.encode(str_date, "utf-8");
                    cookie.setValue(str_date);
                    cookie.setMaxAge(60 * 60 * 24 * 30);
                    response.addCookie(cookie);

                    response.getWriter().write("lastTime: " + time);

                    break;
                }
            }
        }

        if (flag == false) {
            Date date = new Date();
            SimpleDateFormat sdf = new SimpleDateFormat("yyyy:MM:dd HH:mm:ss");
            String str_date = sdf.format(date);
            str_date = URLEncoder.encode(str_date, "utf-8");
            Cookie cookie = new Cookie("lastTime", str_date);
            cookie.setMaxAge(60 * 60 * 24 * 30);
            response.addCookie(cookie);

            response.getWriter().write("欢迎首次访问");
        }
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}
```

## Session

概念：服务器端会话技术，用以保存一次请求的数据，相关类为HttpSession

- HttpSession session = request.getSession();

方法(Session是域对象)：

- setAttribute
- getAttribute
- removeAttribute

原理：Session的实现是依赖于Cookie的

细节：

1. 客户端关闭，服务器不关闭获取的Session是同一个吗

   - 不是

   - ```java
     //如果希望是同一个，可以这么写
     Cookie c = new Cookie("JSESSIONID", session.getId());
     c.setMaxAge(60 * 60);
     response.addCookie(c);
     ```

2. 服务器关闭，客户端关闭，获取的Session是同一个吗

   - 不是，但是要确保数据不丢失(Tomcat已经实现了)
     - Session的钝化：保存在硬盘中
     - Session的活化：由硬盘加载到内存中

3. Session什么时候被销毁

   1. 服务器关闭
   2. Session对象调用invalidate()
   3. Session的默认失效时间(30分钟)

特点：

1. 存储在服务器
2. 可以存储任意类型，任意大小的数据

> Session与Cookie的区别：
>
> 1. Session存储于服务器，Cookie存储于浏览器
> 2. Session没有数据大小限制，Cookie有
> 3. Session数据相对安全，Cookie相对不安全

## JSP

JSP(Java Server Page)：

- 既可以写html，又可以写Java。

原理：

- index.jsp –> index_jsp.java –> index_jsp.class
- JSP本质上就是一个Servlet

JSP脚本：JSP定义Java代码的方式

1. \<% 代码 %\>：转换后在servive方法中，service方法中能写什么，就能写什么
2. \<%! 代码 %\>：JSP转换后在的Java类的成员位置，所以只能写成员变量和成员方法
3. \<%= 代码 %\>：转换为输出语句，输出语句能什么，该脚本就可以定义什么
   - \<%= i %\>

JSP内置对象

- 在JSP页面中不需要获取和创建，可以直接使用的对象

- 一共有9个内置对象

  - request(域对象)：一次请求间共享数据

  - response：响应对象

  - out：字符输出流，可以将数据输出到页面上

    - > response.getWriter()与out的区别：
      >
      > - Tomcat服务器响应时，先找response缓冲区，然后找out缓冲区
      > - response.getWriter()输出永远在out之前

  - pageContext(域对象)：当前页面共享数据，还可以获取其他8个内置对象

  - session(域对象)：一次会话的多个请求间共享数据

  - application(域对象)：所有用户间共享数据

  - page：当前页面的对象

  - config：Servlet的配置对象

  - exception：异常对象

指令：

1. 作用：用于配置JSP页面，导入资源文件
2. 格式：\<%@ 属性名=属性值 …\>
3. 分类：
   1. page：配置页面
      1. contentType：等同于response.setContentType()
      2. language：java
      3. buffer：缓冲区大小，默认8KB
      4. import：导包
      5. errorPage：错误页面，发送异常后跳转到指定的错误页面
      6. isErrorPage：标识当前页面是否为errorPage(设为true可以使用exception内置对象)
   2. include：导入资源文件
      - \<%@ include file="top.jsp"\>
   3. taglib：导入资源
      - <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core">

注释：

1. HTML注释
   - \<!-- --\>
2. JSP注释(推荐使用)
   - \<%-- --%\>

## MVC开发模式

演变：

1. Servlet
2. JSP，在JSP中写Java代码和HTML标签，难以维护，难以分工
3. Java Web借鉴MVC开发模式

MVC：

1. M：Model(JavaBean)

   - 业务逻辑操作，如查询数据库

2. V：View(JSP)

   - 展示数据

3. C：Controller(Servlet)

   - 获取客户端输入

   - 调用Model

   - 将数据交给View

优点:

1. 耦合性低，方便维护，利于分工协作
2. 重用性高

缺点：

- 使得项目结构复杂，对开发人员要求高，需要精心设计

## EL

概念：Expression Language：表达式书写，替换和简化JSP页面中Java代码的编写

语法：${表达式}

注意：JSP默认支持EL表达式

- 在$前加入\可忽略EL表达式
- 在page指令中设置isELIgnored="true"该JSP种EL表达式全部忽略

使用：

1. 运算

   1. +, -, *, /(div), %(mod)
   2. \> < >= <= == !=
   3. &&(and) ||(or) !(not)
   4. empty：用以判断字符串、集合、数组对象是否为null或长度是否为0

2. 获取值

   1. 语法：
      1. ${域名称.键名}
         1. pageScope –> pageContext
         2. requestScope –> request
         3. sessionScope –> session
         4. applicationScope –> application
      2. ${键名}
         - 顺序：pageContext –> request –> session –> application
   2. 获取对象
      1. 对象：${域名.键名.属性名}
         - u.name(u.getName())
      2. List集合：\${域名.键名[索引]}
         - \${list[0]}，越界什么都不显示
      3. Map集合：\${域名.键名.key名称}或\${域名.键名["key名称"]}
         - ${map.gender}
         - ${map["gender"]}

3. 隐式对象(11个)

   - pageContext：获取其他八个内置对象

     - ```jsp
       ${pageContext.request.contextPath}
       ```

## JSTL

概念：JavaServer Pages Tag Library

作用：简化和替换JSP页面上的Java代码

使用步骤：

1. 导入jar包
2. 引入标签库：taglib指令(<%@ taglib %>)
3. 使用标签

常用标签：

1. if

   ```jsp
   <c:if test="">
       
   </c:if>
   ```

2. choose

   ```jsp
   <c:choose>
       <c:when test=""></c:when>
       ... ...
       <c:otherwise></c:otherwise>
   </c:choose>
   ```

3. forEach

   ```jsp
   <c:forEach begin="" end="" var="" step="">
       
   </c:forEach>
   
   <c:forEach items="${list}" var="str" varStatus="s">
       ${s.index}
       ${s.count}
   </c:forEach>
   ```

## Filter

作用：

- 一般用于完成通用的操作，如登录验证、统一编码处理，敏感字符过滤

入门：

1. 定义类实现接口Filter
2. 实现方法
3. 配置拦截路径

```java
package web;

import javax.servlet.*;
import javax.servlet.annotation.WebFilter;
import java.io.IOException;

@WebFilter("/*")
public class FilterDemo implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {

    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        System.out.println("... ...");

        filterChain.doFilter(servletRequest,servletResponse);
    }

    @Override
    public void destroy() {

    }
}
```

web.xml配置

```xml
<filter>
        <filter-name>demo</filter-name>
        <filter-class>web.FilterDemo</filter-class>
    </filter>

    <filter-mapping>
        <filter-name>demo</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
```

生命周期：

- init
- doFilter
- destroy

过滤器配置：

1. 拦截路径配置

   1. 具体路径：/index.jsp
   2. 目录拦截：/user/\*
   3. 后缀名拦截：\*.jsp
   4. 拦截所有资源：/\*

2. 拦截方式配置：资源被访问的方式

   1. 注解配置：设置dispatcherTypes属性

      1. REQUEST：默认值，浏览器直接请求资源
      2. FORWORD：转发访问资源
      3. INCLUDE：包含访问资源
      4. ERROR：错误跳转资源
      5. ASYNC：异步访问资源

   2. web.xml配置

      ```xml
      <filter-mapping>
          <filter-name>demo</filter-name>
          <url-pattern>/*</url-pattern>
          <dispatcher>...</dispatcher>
      </filter-mapping>
      ```

3. 过滤器链的配置

   1. 如果有两个过滤器
      - Filter1 –> Filter2 –> 资源执行 –> Filter2 –> Filter1
   2. 执行顺序问题
      1. 注解配置：按照类名的字符串比较规则进行比较，最小的先则执行
         - AFilter < BFilter(AFilter先执行)
      2. web.xml配置：谁定义在上面，谁先执行

> 代理：
>
> - 概念
>   - 真实对象
>   - 代理对象
>   - 代理模式：代理对象代理真实对象，达到增强真实对象功能的目的
> - 实现方式
>   - 静态代理：有一个类文件描述代理模式
>   - 动态代理：在内存中形成代理类
>     1. 代理对象和真实对象实现相同的接口
>     2. 代理对象：Proxy.newProxyInstance()
>        1. 第一个参数：类加载器
>        2. 第二个参数：接口数组
>        3. 第三个参数：处理器
>           - invoke
>             - proxy
>             - method：代理对象调用的方法，被封装为对象
>             - args：代理对象调用方法时，传递的实际对象
>     3. 使用代理对象调用方法

## Listener

事件监听机制

- 事件源
- 监听器
- 注册监听

ServletContextLinstener：监听ServletContext对象的创建和监听

- contextDestroyed(ServletXontextEvent sec)
- contextImitialized(ServletContextEvent sec)

步骤：

1. 定义一个类，实现ServletContextLinstener接口
2. 实现方法
3. 配置

```java
package web;

import javax.servlet.ServletContextEvent;
import javax.servlet.ServletContextListener;
import javax.servlet.annotation.WebListener;

@WebListener
public class ListenerDemo implements ServletContextListener {
    @Override
    public void contextInitialized(ServletContextEvent servletContextEvent) {
        System.out.println("服务器启动自动被调用");
    }

    @Override
    public void contextDestroyed(ServletContextEvent servletContextEvent) {
        System.out.println("服务器正常关闭后被调用");
    }
}
```

