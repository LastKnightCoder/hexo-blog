---
title: Spring MVC笔记
categories:
    - Spring MVC
tags: 
	- Java
	- SpringMVC
date: 2020-03-01
cover: https://gitee.com/lastknightcoder/blogimage/raw/master/img/bg74.jpg
---

本片文章是我记录学习 `Spring MVC` 的学习笔记，作为初学者，对于这个框架的理解可能并不深刻，所以这篇文章主要讲述的是 `Spring MVC` 框架的使用，所以对于有些内容为什么要这么做，这么做有什么好处，由于才疏学浅，却不是我能解释的，所以本篇文章以代码偏多，文字解释偏少。

## Hello Spring MVC

先简单的的把 `Spring MVC` 用起来，然后在解释一下 `Spring MVC` 的用法。首先在 `pom.xml` 中导入需要的包

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>5.2.0.RELEASE</version>
    </dependency>
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
        <scope>test</scope>
    </dependency>
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>servlet-api</artifactId>
        <version>2.5</version>
    </dependency>
    <dependency>
        <groupId>javax.servlet.jsp</groupId>
        <artifactId>jsp-api</artifactId>
        <version>2.2</version>
    </dependency>
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>jstl</artifactId>
        <version>1.2</version>
    </dependency>
</dependencies>
```

`spring-webmvc` 就是我们需要的包，而 `junit` 不用说，是用来测试用的。`Java` 底层对浏览器做出响应是基于 `Servlet`，所以 `Spring MVC` 也是基于 `Servlet` 的，所以我们还导入了 `Servlet` 的包，以及对 `JSP` 的支持和 `JSTL` 语言的支持的包。

由于我们建立的只是一个普通的 `Maven` 工程，我们要对项目添加支持，使其成为一个 `Web` 项目，单击项目右键选择 `Add Framework Support...`，如下

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/20200229235310.png"/>

接着勾选 `WebApplication`，点击 `OK`

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/20200229235625.png"/>

这时会在你的项目中为你生成一个 `web` 目录，如下

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/20200301000150.png"/>

这时我们配置 `web.xml` 如下

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <servlet>
        <servlet-name>dispatch</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc-config.xml</param-value>
        </init-param>
    </servlet>
    <servlet-mapping>
        <servlet-name>dispatch</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
    
</web-app>
```

我们知道 `JavaWeb` 的工作流程是根据浏览器的请求地址去找对应的 `Servlet` 去做处理，这里我们配置对所有的请求(`/`)都会使用 `Spring` 提供的 `DispatchServlet` 进行处理。具体的处理流程稍后会介绍，从上面的配置看出，`DispatchServlet` 还需要一个配置文件地址的参数，这里我们写为了 `classpath:springmvc-config.xml`，而这个文件还没有，所以我们在 `src/main/resources` 中新建 `springmvc-config.xml`，内容如下

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/mvc
        http://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <mvc:annotation-driven/>
    <mvc:default-servlet-handler/>
    <context:component-scan base-package="com.controller"/>
    
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="WEB-INF/jsp/"/>
        <property name="suffix" value=".jsp"/>
    </bean>

</beans>
```

我们在这个配置文件中对 `com.controller` 中的类开启了 `mvc` 注解支持。还配置了一个 `ServletHandler` 和 `ViewResolver`，这两个东西与后面讲的 `Spring MVC` 执行流程有关，后面再说。

现在新建 `com.controller.HandleRequest.java`，内容如下

```java
package com.controller;

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HandleRequest {
    
    @RequestMapping("/hello")
    public String sayHello() {
        return "Hello Spring MVC";
    }
}
```

该类的作用是当浏览器访问 `/hello` 时，会返回浏览器一个字符串 `"Hello Spring MVC"`，现在使用 `tomcat` 服务器启动该项目，并且在地址栏后输入 `/hello`，这时我们会得到一个错误如下

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/20200301004227.png"/>

这是因为我们没有在项目中加入所依赖的包，这时我们在 `IDEA` 中的左上角找到 `File` 并点击，选择 `Project Structure`，接着在 `Project Structure` 中选择 `Artifacts`

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/20200301110550.png"/>

这时选择你的项目，比如我的是 `mvc-hello`，右键选择 `Put into Output Root`

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/2020-03-01_11-39-48.png"/>

接下来重新启动 `Tomcat`，然后在浏览器地址栏后输入 `/hello`

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/20200301114644.png"/>

就可以看到 `Hello Spring MVC` 在浏览器上显示了出来。

## Spring MVC的执行流程

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703121833.png"/>

上图演示了 `Spring MVC` 执行的流程，在这里稍作解释

1. 用户发送请求至前端控制器 `DispatcherServlet`
2. `DispatcherServlet` 收到请求调用处理器映射器 `HandlerMapping`
3. 处理器映射器根据请求 `url` 找到具体的处理器，生成处理器执行链 `HandlerExecutionChain`(包括处理器对象和处理器拦截器)一并返回给 `DispatcherServlet`。
4. `DispatcherServlet` 根据处理器 `Handler` 获取处理器适配器 `HandlerAdapter`，执行 `HandlerAdapter` 处理一系列的操作，如：参数封装，数据格式转换，数据验证等操作
5. 执行处理器 `Handler`(`Controller`，也叫页面控制器)
6. `Handler` 执行完成返回 `ModelAndView`
7. `HandlerAdapter` 将 `Handler` 执行结果 `ModelAndView` 返回到 `DispatcherServlet`
8. `DispatcherServlet` 将 `ModelAndView` 传给 `ViewReslover` 视图解析器
9. `ViewReslover` 解析后返回具体 `View`
10. `DispatcherServlet` 对 `View` 进行渲染视图(即将模型数据 `model` 填充至视图中)
11. `DispatcherServlet` 响应用户

现在稍加解释上面牵涉到的组件：

1. `DispatcherServlet`：前端控制器，用户请求到达前端控制器，它就相当于 `MVC` 模式中的 `C`，`DispatcherServlet` 是整个流程控制的中心，由它调用其它组件处理用户的请求，`DispatcherServlet` 的存在降低了组件之间的耦合性,系统扩展性提高
2. `HandlerMapping`：处理器映射器，`HandlerMapping` 负责根据用户请求的 `url` 找到 `Handler` 即处理器，`SpringMVC` 提供了不同的映射器实现不同的映射方式，根据一定的规则去查找,例如：`xml` 配置方式，实现接口方式，注解方式等
3. `Handler`：处理器，`Handler` 是继 `DispatcherServlet` 前端控制器的后端控制器，在 `DispatcherServlet` 的控制下 `Handler` 对具体的用户请求进行处理。由于 `Handler` 涉及到具体的用户业务请求，所以一般情况需要程序员根据业务需求开发 `Handler`
4. `HandlAdapter`：处理器适配器,通过 `HandlerAdapter` 对处理器进行执行，这是适配器模式的应用，通过扩展适配器可以对更多类型的处理器进行执行
5. `ModelAndView` 是 `SpringMVC` 的封装对象，将 `Model` 和 `View` 封装在一起。
6. `ViewResolver`：视图解析器，`ViewResolver` 负责将处理结果生成 `View` 视图，`ViewResolver` 首先根据逻辑视图名解析成物理视图名即具体的页面地址，再生成 `View` 视图对象，最后对 `View` 进行渲染将处理结果通过页面展示给用户。
7. `View`：是 `SpringMVC` 的封装对象，是一个接口, `SpringMVC` 框架提供了很多的 `View` 视图类型，包括：`jspview`，`pdfview`,`jstlView`、`freemarkerView`、`pdfView` 等。一般情况下需要通过页面标签或页面模版技术将模型数据通过页面展示给用户，需要由程序员根据业务需求开发具体的页面。

## Spring MVC注解

### @Controller

`@Controller` 注解表明了一个类是作为控制器的角色而存在的。`Spring` 不要求你去继承任何控制器基类，也不要求你去实现 `Servlet` 的那套 `API`。当然，如果你需要的话也可以去使用任何与 `Servlet` 相关的特性和设施。

`@Controller` 注解可以认为是被标注类的原型(`stereotype`)，表明了这个类所承担的角色。分派器(`DispatcherServlet`)会扫描所有注解了 `@Controller` 的类，检测其中通过 `@RequestMapping` 注解配置的方法(详见下一小节)。

当然，你也可以不使用 `@Controller` 注解而显式地去定义被注解的 `bean`，这点通过标准的 `Spring bean` 的定义方式，在 `dispather` 的上下文属性下配置即可做到。但是 `@Controller` 原型是可以被框架自动检测的，`Spring` 支持 `classpath` 路径下组件类的自动检测，以及对已定义 `bean` 的自动注册。

### @RestController

这里说的是与 `@Controller`，被 `@Controller` 注解的类，如果在方法中方法字符串，则会被视图解析器解析，即如下

```java
package com.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class HandleRequest {

    @RequestMapping("/hello")
    public String sayHello() {
        return "hello";
    }
}
```

当访问 `/hello` 时，会执行 `sayHello` 方法，这时返回的 `hello` 会被视图解析器解析，视图解析器会根据在 `springmvc-config.xml` 中的配置进行拼接

```xml
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <property name="prefix" value="WEB-INF/jsp/"/>
    <property name="suffix" value=".jsp"/>
</bean>
```

所以上面方法的 `hello` 会被拼接成 `WEB-INF/jsp/hello.jsp`，接着会返回该 `jsp` 页面。

而使用 `@RestController` 注解，则不会经过视图解析器，而是会将该结果直接返回，就像在前一节演示的例子一样。因为最近流行前后端分离，所以后端不需要写页面了，只需要将前端请求的数据返回，而前端负责渲染展示数据，所以 `@RestController` 类用的还是比较多的。

### @RequestMapping

你可以使用 `@RequestMapping` 注解来将请求 `URL`，如 `/appointments` 等，映射到整个类上或某个特定的处理器方法上。一般来说，类级别的注解负责将一个特定(或符合某种模式)的请求路径映射到一个控制器上，同时通过方法级别的注解来细化映射，即根据特定的 `HTTP` 请求方法（`"GET" "POST"`方法等）、`HTTP` 请求中是否携带特定参数等条件，将请求映射到匹配的方法上。

现在来讲一下 `@RequestMapping` 中的属性，`@RequestMapping` 的源码如下

```java
public @interface RequestMapping {
    String name() default "";

    @AliasFor("path")
    String[] value() default {};

    @AliasFor("value")
    String[] path() default {};

    RequestMethod[] method() default {};

    String[] params() default {};

    String[] headers() default {};

    String[] consumes() default {};

    String[] produces() default {};
}
```

- `name, value, path`：是用来设置匹配的 `url` 路径的
- `method`：指定请求的 `method` 类型, `GET、POST、PUT、DELETE` 等
- `consumes`：指定处理请求的提交内容类型(`Content-Type`)，例如 `application/json, text/html`
- `produces`：指定返回的内容类型，仅当 `request` 请求头中的(`Accept`)类型中包含该指定类型才返回
- `params`：指定 `request` 中必须包含某些参数值时，才让该方法处理
- `headers`：指定 `request` 中必须包含某些指定的 `header` 值，才能让该方法处理请求

给个例子：

```java
@RequestMapping(value="/hello",
                method = RequestMethod.GET,
                params = "myParam=myValue",
                headers="Referer=www.baidu.com",
                consumes="application/json",
                produces="application/json")
```

除了可以使用 `method` 属性指定请求办法外，还可以使用注解：

- `@GetMapping("/hello")`
- `@PostMapping("/hello")`
- `......`

这些注解与 `RequestMapping` 具有相同的属性，除了 `method` 属性。

### @RequestParam

该注解类型用于将指定的请求参数赋值给方法中的形参，那么 `@RequestParam` 注解有什么属性呢? 它有 `4` 种属性，下面将逐一介绍这四种属性：
- `name` 属性：该属性的类型是 `String` 类型，它可以指定请求头绑定的名称
- `value` 属性：该属性的类型是 `String` 类型，它可以设置是 `name` 属性的别名
- `required` 属性：该属性的类型是 `boolean` 类型，它可以设置指定参数是否必须绑定
- `defalutValue` 属性：该属性的类型是 `String` 类型，它可以设置如果没有传递参数可以使用默认值

```java
@RequestMapping(value="/hello")
public String hello(
	@RequestParam("loginname") String loginname,
	@RequestParam("password") String password) {
	
    return "hello";
}
```

### @PathVaribale

`@PathVaribale` 注解，该注解类型可以非常方便的获得请求 `url` 中的动态参数。`@PathVaribale` 注解只支持一个属性 `value`，类型 `String`，表示绑定的名称，如果省略则默认绑定同名参数

```java
@RequestMapping(value="/pathVariableTest/{userId}")
public void pathVariableTest(@PathVaribale Integer userId)
```

### @CookieValue

绑定 `cookie` 的值到 `Controller` 方法参数

```java
@RequestMapping ( "/hello" )
public String testCookieValue( @CookieValue("hello") String cookieValue) {
   return "cookieValue" ;
}
```

### @RequestHeader

`@RequestHeader` 注解，该注解类型用于将请求的头的信息区域数据映射到功能处理方法的参数上。那么`@RequestHeader` 注解有什么属性呢? 它和 `@RequestParam` 注解一样，也有 `4` 种属性，分别如下

- `name` 属性：该属性的类型是 `String` 类型，它可以指定请求头绑定的名称
- `value` 属性：该属性的类型是 `String` 类型，它可以设置是 `name` 属性的别名
- `required` 属性：该属性的类型是 `boolean` 类型，它可以设置指定参数是否必须绑定
- `defalutValue` 属性：该属性的类型是 `String` 类型，它可以设置如果没有传递参数可以使用默认值

```java
@RequestMapping(value="/requestHeaderTest")
public void requestHeaderTest(
	@RequestHeader("User-Agent") String userAgent,
	@RequestHeader(value="Accept") String[] accepts) {
}
```

### @RequestBody

`@RequestBody` 注解是将 `HTTP` 请求正文插入方法中

```java
@RequestMapping(value = "/login")
public String login(@RequestBody Person person) {

	return "..."; 
}
```

`@RequestBody` 注解常用来处理 `Content-type` 不是默认的 `application/x-www-form-urlcoded` 编码的内容，比如说：`application/json` 或者是 `application/xml` 等。一般情况下来说常用其来处理 `application/json` 类型。

对于前端使用而言，`form` 表单的 `enctype` 属性为编码方式，常用有两种：`application/x-www-form-urlencoded` 和 `multipart/form-data`，默认为 `application/x-www-form-urlencoded`，所以在前端传输数据时，需要将 `Content-type` 显示指定为 `application/json`。

**总结：**

- `ReuqestBody` 主要是处理 `json` 串格式的请求参数，要求使用方指定 `header Content-type:application/json`
- `RequestBody` 通常要求调用方使用 `post` 请求


### @ResponseBody

`@ResponseBody` 注解的作用是将 `Controller` 的方法返回的对象通过适当的转换器转换为指定的格式之后，写入到 `response` 对象的 `body` 区，通常用来返回 `JSON` 数据或者是 `XML` 数据，需要注意的呢，在使用此注解之后不会再走视图处理器，而是直接将数据写入到输入流中，它的效果等同于通过 `response` 对象输出指定格式的数据。

```java
@RequestMapping("/login")
@ResponseBody
public User login(User user){
    //User字段：userName pwd
    //那么在前台接收到的数据为：'{"userName":"xxx","pwd":"xxx"}'
　　return user;
}

//效果等同于如下代码：
@RequestMapping("/login")
public void login(User user, HttpServletResponse response){
　　　response.getWriter.write(JSONObject.fromObject(user).toString());
    }
}
```

## 参考资料

- [SpringMVC执行流程及工作原理](https://www.jianshu.com/p/8a20c547e245)
- [RequestMapping 属性解释](https://wuyujia.github.io/2017/01/18/requestMapping/)
- [Spring之RequestBody的使用姿势小结](https://juejin.im/post/5b5efff0e51d45198469acea#heading-0)
- [@responseBody注解的使用](https://www.jianshu.com/p/1233b22738d8)