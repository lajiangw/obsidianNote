[**源文档链接**](https://mowangblog.github.io/SpringMVC-Demo/#/)
# MVC 基础概念

^c286b2

什么是 MVC？
MVC 是一种软件加架构的思想，将软件按照模型，视图，控制器，来划分。 ^6b3c2c

- M： Model 模型层，指的是工程中的 javaBean，**作用是处理数据** ^b6826d
  - javaBean 分为两类： ^1d7343
    - 一类称为实体类 Bean：专门存储 Student，User 等
    - 一类称为业务处理 Bean：指的 Service 或者 Dao 对象，转为用于处理业务逻辑和访问处理。
- V：View，视图层，指工程中的 Html 或者 jsp 等页面，作用是与用户进行交互展示数据。
- Controller，控制层，指工程中的 servlet，作用是接受请求和响应浏览器。

MVC 的工作流程： **用户通过视图层发送请求到服务器，在服务器中请求被 Controller 接收，Controller 调用相应的 Model 层处理请求，处理完毕将结果返回到 Controller，Controller 再根据请求处理的结果找到相应的 View 视图，渲染数据后最终响应给浏览器**

# 什么是 SpringMVC

SpringMVC 是 Spring 的一个后续产品，是 Spring 的一个子项目

SpringMVC 是 Spring 为表述层开发提供的一整套完备的解决方案。在表述层框架历经 Strust、WebWork、Strust2 等诸多产品的历代更迭之后，目前业界普遍选择了 SpringMVC 作为 Java EE 项目表述层开发的**首选方案**。

>三层架构分别是：表述层（表示层），业务逻辑层，数据访问层，表述层表示前台页面和后台 servlet
# SpringMVC 特点

-   **Spring 家族原生产品**，与 IOC 容器等基础设施无缝对接
-   **基于原生的Servlet**，通过了功能强大的**前端控制器DispatcherServlet**，对请求和响应进行统一处理
-   表述层各细分领域需要解决的问题**全方位覆盖**，提供**全面解决方案**
-   **代码清新简洁**，大幅度提升开发效率
-   内部组件化程度高，可插拔式组件**即插即用**，想要什么功能配置相应组件即可
-   **性能卓著**，尤其适合现代大型、超大型互联网项目要求


# 第一个 SpingMVC 程序 
使用 Maven 创建工程，导入依赖即可。
Thymeleaf 是在服务器端使用的，用来产生视图页面的。
## 配置 WEB. xml 文件
- 默认配置方式（一般不使用，此配置方式文件名字方式都是默认，利于我们管理）
```xml
<!-- 配置SpringMVC的前端控制器，对浏览器发送的请求统一进行处理 -->
<servlet>
    <servlet-name>springMVC</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>springMVC</servlet-name>
    <!--
        设置springMVC的核心控制器所能处理的请求的请求路径
        /所匹配的请求可以是/login或.html或.js或.css方式的请求路径
        但是/不能匹配.jsp请求路径的请求
    -->
    <url-pattern>/</url-pattern>
</servlet-mapping>

```
**注意 /可以匹配所有请求，但是不包括. Jsp 的请求。/* 包括这个工程下所有的请求。** [[web中斜杠的含义]]    

```xml

- 扩展配置方式（文件路径放在 Maven 工程下的 resources 下）
```xml
<!-- 配置SpringMVC的前端控制器，对浏览器发送的请求统一进行处理 -->
<servlet>
    <servlet-name>springMVC</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <!-- 通过初始化参数指定SpringMVC配置文件的位置和名称 -->
    <init-param>
        <!-- contextConfigLocation为固定值 -->
        <param-name>contextConfigLocation</param-name>
        <!-- 使用classpath:表示从类路径查找配置文件，例如maven工程中的src/main/resources -->
        <param-value>classpath:springMVC.xml</param-value>
    </init-param>
    <!-- 
         作为框架的核心组件，在启动过程中有大量的初始化操作要做
        而这些操作放在第一次请求时才执行会严重影响访问速度
        因此需要通过此标签将启动控制DispatcherServlet的初始化时间提前到服务器启动时
    -->
    <load-on-startup>1</load-on-startup>
</servlet>
<servlet-mapping>
    <servlet-name>springMVC</servlet-name>
    <!--
        设置springMVC的核心控制器所能处理的请求的请求路径
        /所匹配的请求可以是/login或.html或.js或.css方式的请求路径
        但是/不能匹配.jsp请求路径的请求
    -->
    <url-pattern>/</url-pattern>
</servlet-mapping>
```
**使用 classpath: 表示从类路径查找配置文件，例如 maven 工程中的 src/main/resources。**
## 配置 Maven 依赖文件
```xml
<dependencies>  
        <!-- SpringMVC -->  
        <dependency>  
            <groupId>org.springframework</groupId>  
            <artifactId>spring-webmvc</artifactId>  
            <version>5.3.9</version>  
        </dependency>  
  
        <!-- 日志 -->  
        <dependency>  
            <groupId>ch.qos.logback</groupId>  
            <artifactId>logback-classic</artifactId>  
            <version>1.2.3</version>  
        </dependency>  
  
        <!-- ServletAPI -->  
        <dependency>  
            <groupId>javax.servlet</groupId>  
            <artifactId>javax.servlet-api</artifactId>  
            <version>3.1.0</version>  
            <scope>provided</scope>  
        </dependency>  
  
        <!-- Spring5和Thymeleaf整合包 -->  
        <dependency>  
            <groupId>org.thymeleaf</groupId>  
            <artifactId>thymeleaf-spring5</artifactId>  
            <version>3.0.12.RELEASE</version>  
        </dependency>  
</dependencies>
```
## 创建请求控制器
使用注解的方式创建 Controller 创建 IOC 容器
```java

@Controller
//作为控制器
public class HelloController {

}

```
## 创建 SpringMVC 的配置文件 
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<beans  xmlns="http://www.springframework.org/schema/beans"  
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
              xmlns:context="http://www.springframework.org/schema/context"  
              xsi:schemaLocation="http://www.springframework.org/schema/beans  
       http://www.springframework.org/schema/beans/spring-beans.xsd       http://www.springframework.org/schema/context       http://www.springframework.org/schema/context/spring-context.xsd ">  
  
    <!-- 自动扫描包 -->  
    <context:component-scan base-package="com.zml.mvc.controller"/>  
  
  
    <!-- 配置Thymeleaf视图解析器 -->  
    <bean id="viewResolver" class="org.thymeleaf.spring5.view.ThymeleafViewResolver">  
        <property name="order" value="1"/>  
        <property name="characterEncoding" value="UTF-8"/>  
        <property name="templateEngine">  
            <bean class="org.thymeleaf.spring5.SpringTemplateEngine">  
                <property name="templateResolver">  
                    <bean class="org.thymeleaf.spring5.templateresolver.SpringResourceTemplateResolver">  
                        <!-- 视图前缀 -->  
                        <property name="prefix" value="/WEB-INF/templates/"/>  //展示视图的地址。 
                        <!-- 视图后缀 -->  
                        <property name="suffix" value=".html"/>  
                        <property name="templateMode" value="HTML5"/>  
                        <property name="characterEncoding" value="UTF-8" />  
                    </bean>  
                </property>  
            </bean>  
        </property>  
    </bean>  
  
</beans>

<!-- 
   处理静态资源，例如html、js、css、jpg
  若只设置该标签，则只能访问静态资源，其他请求则无法访问
  此时必须设置<mvc:annotation-driven/>解决问题
 -->
<mvc:default-servlet-handler/>
<!-- 开启mvc注解驱动 -->
<mvc:annotation-driven>
    <mvc:message-converters>
        <!-- 处理响应中文内容乱码 -->
        <bean class="org.springframework.http.converter.StringHttpMessageConverter">
            <property name="defaultCharset" value="UTF-8" />
            <property name="supportedMediaTypes">
                <list>
                    <value>text/html</value>
                    <value>application/json</value>
                </list>
            </property>
        </bean>
    </mvc:message-converters>
</mvc:annotation-driven>
```

^04c204

## 在控制器中配置
```java
// @RequestMapping注解：处理请求和控制器方法之间的映射关系
// @RequestMapping注解的value属性可以通过请求地址匹配请求，/表示的当前工程的上下文路径
// localhost:8080/springMVC/
@RequestMapping("/")
public String index() {
    //设置视图名称
    return "index";
}

```
**@RequestMapping 注解：处理请求和控制器方法之间的映射关系，当 RequestMapping 中只有 value 一个属性时，可以不用写 value。**
 
## 总结
浏览器发送请求，若请求地址符合前端控制器的 url-pattern，该请求就会被前端控制器 DispatcherServlet 处理。前端控制器会读取 SpringMVC 的核心配置文件，通过扫描组件找到控制器，将请求地址和控制器中@RequestMapping 注解的 value 属性值进行匹配，若匹配成功，该注解所标识的控制器方法就是处理请求的方法。处理请求的方法需要返回一个字符串类型的视图名称，该视图名称会被视图解析器解析，加上前缀和后缀组成视图的路径，通过 Thymeleaf 对视图进行渲染，最终转发到视图所对应页面
# @RequestMapping 注解
RequestMapping 翻译为：==请求映射==
@RequestMapping 注解的作用就是将请求和处理请求的控制器方法关联起来，建立映射关系。
SpringMVC 接收到指定的请求，就会来找到在映射关系中对应的控制器方法来处理这个请求。
**RequestMapping 中的 values 属性必须是唯一的，否则会引起冲突。**
## RequestMapping 注解的位置
他可以放在类和方法上面。
*@RequestMapping 标识一个类：设置映射请求的请求路径的初始信息，如果用在类上面之后，就是首先会匹配类上面 RequestMapping 中 value 的值，再去匹配方法上面的 RequestMapping 信息。访问的地址也就变成了 http：//localhost: port/工程路径/类上直接 value 值/方法注解 value 值
@RequestMapping 标识一个方法：设置映射请求请求路径的具体信息。*
注解中的 value 信息是访问路径设置，和类名方法名都无关。
使用场景： 当同一工程下，有两个模块的名字冲突时，就可以使用在类名上加 @RequestMapping 来解决。例如一个是 http: //localhost: port/工程路径/user/login，一个是  
http: //localhost: port/工程路径/book/login
```java
@Controller
@RequestMapping("/test")
public class RequestMappingController {

    //此时请求映射所映射的请求的请求路径为：/test/testRequestMapping
    @RequestMapping("/testRequestMapping")
    public String testRequestMapping(){
        return "success";
    }

}
```
## @RequestMapping 注解中的 value 属性
@RequestMapping 注解的 value 属性通过请求的请求地址匹配请求映射
也是访问的路径，value 值需要和访问地址匹配才会成功！ [[value属性值详解]] 

@RequestMapping 注解的 value 属性是一个**字符串类型的数组**，**表示该请求映射能够匹配多个请求地址所对应的请求。**

@RequestMapping 注解的 value 属性必须设置的，可以和其他的值配合使用，至少通过请求地址匹配请求映射。
Html 文件的链接
```xml
<a th:href="@{/testRequestMapping}">测试@RequestMapping的value属性-->/testRequestMapping</a><br>
<a th:href="@{/test}">测试@RequestMapping的value属性-->/test</a><br>
```
控制器的注解设置
```java
//上面两条超链接都会被 testRequestMapping 接收，因为 value 是一个字符串集合。
@RequestMapping(
        value = {"/testRequestMapping", "/test"}
)
public String testRequestMapping(){
    return "success";
}

```

## @RequestMapping 注解中的 method 属性
@RequestMapping 注解的 method 属性通过请求的**请求方式（get 或 post）匹配请求映射**

@RequestMapping注解的method属性是一个RequestMethod类型的**数组**，**表示该请求映射能够匹配多种请求方式的请求**

若当前请求的请求地址满足请求映射的 value 属性，但是请求方式不满足 method 属性，则浏览器报错 405：**Request method ‘POST’ not supported**

如果不进行 method 属性的设置，说明请求方式不受限制，任何的请求方式。都可以接收。
```java
<a th:href="@{/test}">测试@RequestMapping的value属性-->/test</a><br>
<form th:action="@{/test}" method="post">
    <input type="submit">
</form>
```

```java
@RequestMapping(
        value = {"/testRequestMapping", "/test"},
        method = {RequestMethod.GET, RequestMethod.POST}
)
public String testRequestMapping(){
    return "success";
}
1234567
```
### 派生子类
对于处理指定请求方式的控制器方法，SpringMVC中提供了@RequestMapping的派生注解
处理get请求的映射–>@GetMapping
处理post请求的映射–>@PostMapping
处理put请求的映射–>@PutMapping
处理delete请求的映射–>@DeleteMapping
2、常用的请求方式有get，post，put，delete
**但是目前浏览器只支持 get 和 post，若在 form 表单提交时，为 method 设置了其他请求方式的字符串（put 或 delete），则按照默认的请求方式 get 处理**
若要发送 put 和 delete 请求，则需要通过 spring 提供的过滤器 HiddenHttpMethodFilter，在 RESTful 部分会讲到

```java
@RequestMapping(value = "/")  
@GetMapping  
public String test3() {  
    return "succus";  
}
```

## RequestMapping 注解中 params 属性（了解）
@RequestMapping 注解的 params 属性通过请求的请求参数匹配请求映射
@RequestMapping 注解的 params 属性是一个**字符串类型的数组，可以通过四种表达式设置请求参数和请求映射的匹配关系**
若当前请求满足@RequestMapping 注解的 value 和 method 属性，但是不满足 params 属性，此时页面回报错 400
**params 里面的参数只有都满足才会进行匹配映射**
“username”：要求请求映射所匹配的请求必须携带 username 请求参数
“! param”：要求请求映射所匹配的请求必须不能携带 username 请求参数
“param=123”：要求请求映射所匹配的请求必须携带 param 请求参数且 param=123
“param!=123”：要求请求映射所匹配的请求必须携带 param 请求参数但是 param!=123

```java
@RequestMapping(value = "/testParams",  
        params = {"username", "password=123"},  
        headers = {"Host=localhost:8081"}  
)  
public String test1() {  
    return "succus";  
}
```
## RequestMapping 注解中 headers 属性（了解）
@RequestMapping 注解的 headers 属性通过请求的请求头信息匹配请求映射
@RequestMapping 注解的 headers 属性是一个字符串类型的数组，可以通过四种表达式设置请求头信息和请求映射的匹配关系
**headers 里面的参数只有都满足才会进行匹配映射**
“username”：要求请求映射所匹配的请求必须携带 username 请求参数
“! param”：要求请求映射所匹配的请求必须不能携带 username 请求参数
“param=123”：要求请求映射所匹配的请求必须携带 param 请求参数且 param=123
“param!=123”：要求请求映射所匹配的请求必须携带 param 请求参数但是 param!=123
若当前请求满足@RequestMapping 注解的 value 和 method 属性，但是不满足 headers 属性，此时页面显示 404 错误，即资源未找到。
```java
@RequestMapping(value = "/testParams",  
        params = {"username", "password=123"},  
        headers = {"Host=localhost:8081"}  
)  
public String test1() {  
    return "succus";  
}
```

## Spring 支持 ant 风格的路径
value 属性的值可以进行下面的匹配
Ant 路径就是模糊模糊路径
？：表示任意的单个字符
       * ：表示任意的 0 个或多个字符
** ：表示任意的一层或多层目录
注意：在使用 ** 时，只能使用/ ** /xxx 的方式，前后不能有字符串。
```java
@RequestMapping(value = {"/a*a/test2","/**/test2"})  
public String test2() {  
    return "succus";  
}
```
## SpringMVC 支持路径中的占位符（重点）
原始方式：/deleteUser? Id=1
rset 方式：/deleteUser/1
	SpringMVC 路径中的占位符常用于 RESTful 风格中，当请求路径中将某些数据通过路径的方式传输到服务器中，就可以在相应的@RequestMapping 注解的 value 属性中通过占位符{xxx}表示传输的数据，在通过@PathVariable 注解，将占位符所表示的数据赋值给控制器方法的形参
可以参考 JDBC 中的占位符去理解
```java
<a th:href="@{/testRest/1/admin}">测试路径中的占位符-->/testRest</a>
```

```java
@RequestMapping("/testRest/{id}；/{username}")
public String testRest(@PathVariable("id") String id, @PathVariable("username") String username){
    System.out.println("id:"+id+",username:"+username);
    return "success";
}
//最终输出的内容为-->id:1,username:admin

```
# SpringMVC 获取请求参数
## 通过 ServletAPI 获取
将 HttpServletRequest 作为控制器方法的形参，此时 HttpServletRequest 类型的参数表示封装了当前请求的请求报文的对象。
一般我们不使用原生 API 进行操作。
```java
@RequestMapping("/testParam")
public String testParam(HttpServletRequest request){
    String username = request.getParameter("username");
    String password = request.getParameter("password");
    System.out.println("username:"+username+",password:"+password);
    return "success";
}

```
## 通过控制器方法的形参获取请求参数
在控制器方法的形参位置，**设置和请求参数同名的形参**，当浏览器发送请求，匹配到请求映射时，在 DispatcherServlet 中就会将请求参数赋值给相应的形参。
在请求参数中如果出现多个重名请求参数，可以在控制器方法的形参位置，设置同名的字符串类型或者字符串数组。
若使用字符串类型的形参，最终结果为每个请求参数之间使用逗号拼接。
若使用字符串数组，会将请求参数全放到数组中。
```xml
<a th:href="@{/testParam(username='admin',password=123456)}">测试获取请求参数-->/testParam</a><br>
```

```java
1
@RequestMapping("/testParam")
public String testParam(String username, String password){
    System.out.println("username:"+username+",password:"+password);
    return "success";
}
```
## @RequestParam
@RequestParam 是将请求参数和控制器方法的形参创建映射关系
直接的位置是放在方法的形参上，用当前的 value 映射形参。
@RequestParam 注解一共有三个属性：
value：指定为形参赋值的请求参数的参数名
Name：用法和作用和 value 相同。
required：设置是否必须传输此请求参数，默认值为true
若设置为 true 时，则当前请求必须传输 value 所指定的请求参数，若没有传输该请求参数，且没有设置 defaultValue 属性，则页面报错 400：Required String parameter ‘xxx’ is not present；若设置为 false，则当前请求不是必须传输 value 所指定的请求参数，若没有传输，则注解所标识的形参的值为 null。
defaultValue：不管 required 属性值为 true 或 false，**当 value 所指定的请求参数没有传输或传输的值为""时，则使用默认值为形参赋值**
```java
@RequestMapping("/testParam")  
public String testParam(
@RequestParam(name = "user_name",defaultValue = "zhangminlei") String username,
@RequestParam("pass_word") String password, String hobby  
) {  
    System.out.println(username + " " + password + " " + hobby);  
    return "succus";  
}
```
## @RequestHeader
和请求头进行映射来操作数据。
@RequestHeader 是将请求头信息和控制器方法的形参创建映射关系
@RequestHeader 注解一共有三个属性：value、required、defaultValue，用法同@RequestParam 相同。
```java
@RequestMapping("/testParam")  
public String testParam(@RequestHeader("Host") String host) {  
    System.out.println(host);  
    return "succus";  
}
```

## @CookieValue
和 cookie 进行映射来操作数据
@CookieValue 是将 cookie 数据和控制器方法的形参创建映射关系
@CookieValue 注解一共有三个属性：value、required，defaultValue，用法同@RequestParam 相同。
```java
@RequestMapping("/testParam")  
public String testParam( @CookieValue(value = "JSESSIONID") String cookie) {  
    System.out.println(cookie);  
    return "succus";  
}
```

## 通过 POJO 获取请求参数
可以在控制器方法的形参位置设置一个实体类类型的形参，此时若浏览器传输的请求参数的参数名和实体类中的属性名一致，那么请求参数就会为此属性赋值。

```java
@RequestMapping("/testpojo")  
public String testBean(User user) {  //user为创建的实体类，字段名和接收名对应。框架便会帮我们自动获取存入到user对象中。
    System.out.println(user);  
    return "succus";  
}
```

## 解决获取请求参数的乱码问题
浏览器的乱码分为 GET 请求乱码和 PSOT 请求乱码，其中 GET 请求的乱码是由于 TOMCAT 造成的。因为我们在设置编码的时候，需要在服务器请求之前设置，否知会无效，所以我们把它放在过滤器中，因为在 Tomcat 中，先初始化过滤器，然后才是服务器。所以在 web. Xml 中进行过滤器配置，SpringMVC 已经帮我们封装好了。我们在通过源码可知，要首先将 encoding 设置为需要修改的编码，并且将 forceResponseEncoding 设置为 true。
```xml

<!--配置springMVC的编码过滤器-->
<filter>
    <filter-name>CharacterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>UTF-8</param-value>
    </init-param>
    <init-param>
        <param-name>forceResponseEncoding</param-name>
        <param-value>true</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>CharacterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```
# 域对象共享数据
ServletContext 对象会在服务器启动时创建，服务器关闭时销毁。
Session 在浏览器打开时，服务器关闭，Session 就会将信息序列化本地磁盘，如果浏览器没有关闭，服务器重新打开了，seeesion 就会将序列化在本地磁盘的文件发送给服务器。
只要 cookie 存在，我们创建的 session 都是同一个。

**PageContext（PageContextImpl 类）                   当前 jsp 页面范围内有效
request（HttpServletRequest 类）                        一次请求内有效
session（HttpSession 类）                   一整个会话中都有效 (打开浏览器到浏览器关闭)
applicantion（ServletContext 类） 整个 web 工程范围内都有效（web 工程不停止，数据都在）**

域对象是可以像Map 一样存取数据的对象。四个域对象的功能一致，但是他们对数据的存取范围不一样。
在能满足我们要求下选最小的域对象。
## 使用 ServletAPI 想 requsest 域对象中共享数据
在我们学习了 SSM 后不推荐使用原生 API 
```java
@RequestMapping("/testRequestByServletAPI")  
public String testServletAPI(HttpServletRequest request) {  
    request.setAttribute("username", "张民磊");  
    return "success";  
}
```
## 使用 ModelAndView 向 request 域对象共享数据
Model 指的是向请求域中共享数据；view 指的是视图。
我们使用 ModelAndView 的话，我们方法的返回值必须是 ModelAndView。
使用 ModelAndView 的 view 方法 API 来设置视图名称。
```java
 @RequestMapping(value = "/testModelAndView")  
    public ModelAndView testModelAndView() {  
        ModelAndView modelAndView = new ModelAndView();  
        modelAndView.addObject("username", "王林");  
//        使用modelandview中的addObject方法向请求共享数据  
        modelAndView.setViewName("success");  
//        使用modelandview设置视图名称 实现跳转  
        return modelAndView;  
    }
```
## 使用 Model 向 request 域对象中共享数据
使用方法直接在控制器方法的形参传入 Model 使用即可
```java
@RequestMapping(value = "testModel")  
public String testModel(Model model) {  
    model.addAttribute("username", "你好，Model");  
    return "success";  
}
```
## 使用 map 向 request 域对象共享数据
使用方法直接在控制器方法的形参传入 Map 使用即可，我们向 Map 传入的数据也就是向域对象共享的数据。
```java
@RequestMapping(value = "/testMap")  
public String testMap(Map<String, Object> map) {  
    map.put("username", "你好,Map");  
    return "success";  
}
```
## 使用 ModelMap 向 request 域对象共享数据
使用方法直接在控制器方法的形参传入 ModelMap 使用即可，我们向 ModelMap 传入的数据也就是向域对象共享的数据。
```java
@RequestMapping("/testModelMap")  
public String testModelMap(ModelMap modelMap) {  
    modelMap.addAttribute("username","你好，ModelMap");  
    return "success";  
}
```

## Model，ModelMap 和 Map 的之间的关系
Model、ModelMap、Map 类型的参数其实本质上都是 BindingAwareModelMap 类型的。查看类继承关系图可以得出。
![[图片/Pasted image 20221229231622.png]]

## 向 session 域共享数据
SpringMVC 也提供了注解帮我们封装，但是不太好用。自己查询 API 即可。
正常我们是用 HttpSession 原生 API。
```java
@RequestMapping("/testSession")  
public String testSession(HttpSession session) {  
    session.setAttribute("testSession", "你好Session");  
    return "success";  
}
```
## 向 ServletContext 域共享数据
向 application 域共享数据也就是 ServletContext 中共享数据。
获得 ServletContext 对象有很多种方法，我们这次使用会话获取。
```java
@RequestMapping("/testapplication")  
public String testApplication(HttpSession session) {  
    ServletContext servletContext = session.getServletContext();  
    servletContext.setAttribute("applicatoin", "你好，application");  
    return "success";  
}
```
# SpringMVC 的视图
SpringMVC 中的视图是 View 接口，视图的作用渲染数据，将模型 Model 中的数据展示给用户。
SpringMVC视图的种类很多，默认有转发视图和重定向视图
当工程引入jstl的依赖，转发视图会自动转换为JstlView
若使用的视图技术为 Thymeleaf，在 SpringMVC 的配置文件中配置了 Thymeleaf 的视图解析器，由此视图解析器解析之后所得到的是 ThymeleafView，
**只有当我们的视图名称没有任何前缀，创建的才是 ThymeleafView。**

## ThymeleafView
当控制器方法中所设置的视图名称没有任何前缀时，此时的视图名称会被 SpringMVC 配置文件中所配置的视图解析器解析，视图名称拼接视图前缀和视图后缀所得到的最终路径，会通过转发的方式实现跳转。查看源码即可解析。

## 转发视图
SpringMVC 中默认的转发视图是 **InternalResourceView**
转发可以获得前请求域中的数据。
转发不可以跨域，因为转发是发生在服务器内部的，他只能访问服务器内部的资源。
SpringMVC中创建转发视图的情况：
当控制器方法中所设置的视图名称以"forward:"为前缀时，创建InternalResourceView视图，此时的视图名称不会被SpringMVC配置文件中所配置的视图解析器解析，而是会将前缀"forward:"去掉，剩余部分作为最终路径通过转发的方式实现跳转
例如"forward:/"，“forward:/employee”
```java
@RequestMapping("/testsuccess")  
public String testSucess() {  
    return "success";  
}  
  
@RequestMapping("/testForward")  
public String testForward() {  
    return "forward:/testsuccess";  
}
//当我们跳转testForward页面时，会返回加上前缀的testsucess页面，当加上前缀就会解析为转发请求 InternalResourceView，再次跳转到/testsuccess控制器方法，方法返回解析ThymeleafViewResolver，然后跳转到success页面。
一般不使用这种方法，因为Thymeleaf帮我们提供的就是转发。
```

## 重定向视图
SpringMVC 中默认的重定向视图是 RedirectView。
重定向不可以获得请求域中的数据。
重定向可以跨域，因为是浏览器发送新的请求，所以可能访问任意资源。
重定向视图在解析时，会先将redirect:前缀去掉，然后会判断剩余部分是否以/开头，若是则会自动拼接上下文路径
当控制器方法中所设置的视图名称以"redirect:"为前缀时，创建RedirectView视图，此时的视图名称不会被SpringMVC配置文件中所配置的视图解析器解析，而是会将前缀"redirect:"去掉，剩余部分作为最终路径通过重定向的方式实现跳转
例如"redirect:/"，“redirect:/employee”
```java
@RequestMapping("/testsucess")  
public String testSucess() {  
    return "success";  
}

@RequestMapping("/testRedirect")  
public String testRedirect(){  
    return "redirect:/testsucess";  
}
// testRedirect方法返回redirect:/testsucess会被解析为重定向，然后传递到控制器方法 testSucess 返回success 页面；
```

## 视图控制器



当控制器方法中，仅仅用来实现页面跳转，即只需要设置视图名称时，可以将处理器方法使用 view-controller 标签进行表示。
当 SpringMVC 中设置任何一个 view-controller 时，其他控制器中的请求映射将全部失效，此时需要在 SpringMVC 的核心配置文件中设置开启 mvc 注解驱动的标 ^shitukongzhiqi
**<mvc: annotation-driven />** ^f2ff49

```xml
<!--
    path：设置处理的请求地址
    view-name：设置请求地址所对应的视图名称
-->
<mvc:view-controller path="/" view-name="index"></mvc:view-controller>
//这段注解的作用就是替代了下面这个控制器方法
```

```java
@RequestMapping("/")  
public String index() {  
      return "index";  
}
```
# RESTful
## RESTful 简介
REST：**Re**presentational **S**tate **T**ransfer，表现层资源状态转移。
RESTful 是一种架构风格。也就是一种格式。
### a>资源
资源是一种看待服务器的方式，即，将服务器看作是由很多离散的资源组成。每个资源是服务器上一个可命名的抽象概念。因为资源是一个抽象的概念，所以它不仅仅能代表服务器文件系统中的一个文件、数据库中的一张表等等具体的东西，可以将资源设计的要多抽象有多抽象，只要想象力允许而且客户端应用开发者能够理解。与面向对象设计类似，资源是以名词为核心来组织的，首先关注的是名词。一个资源可以由一个或多个URI来标识。URI既是资源的名称，也是资源在Web上的地址。对某个资源感兴趣的客户端应用，可以通过资源的URI与其进行交互。

### b>资源的表述
资源的表述是一段对于资源在某个特定时刻的状态的描述。可以在客户端-服务器端之间转移（交换）。资源的表述可以有多种格式，例如 HTML/XML/JSON/纯文本/图片/视频/音频等等。资源的表述格式可以通过协商机制来确定。请求-响应方向的表述通常使用不同的格式。

### c>状态转移
状态转移说的是：在客户端和服务器端之间转移（transfer）代表资源状态的表述。通过转移和操作资源的表述，来间接实现操作资源的目的。
## RESTful 的实现
具体说，就是 HTTP 协议里面，四个表示操作方式的动词：GET、POST、PUT、DELETE。

它们分别对应四种基本操作：**GET 用来获取资源，POST 用来新建资源，PUT 用来更新资源，DELETE 用来删除资源。**

REST 风格提倡 URL 地址使用统一的风格设计，从前到后各个单词使用斜杠分开，不使用问号键值对方式携带请求参数，而是将要发送给服务器的数据作为 URL 地址的一部分，以保证整体风格的一致性。
![[图片/Pasted image 20221230210204.png]]
## HiddenHttpMethodFilter
由于浏览器只支持发送 get 和 post 方式的请求，那么该如何发送 put 和 delete 请求呢？
SpringMVC 提供了 **HiddenHttpMethodFilter** 帮助我们**将 POST 请求转换为 DELETE 或 PUT 请求**

**HiddenHttpMethodFilter** 处理put和delete请求的条件：
a>当前请求的请求方式必须为post
b>当前请求必须传输请求参数_method
满足以上条件，**HiddenHttpMethodFilter** 过滤器就会将当前请求的请求方式转换为请求参数_method 的值，因此请求参数_method 的值才是最终的请求方式。

在 web. xml 中注册**HiddenHttpMethodFilter**
```xml
<filter>
    <filter-name>HiddenHttpMethodFilter</filter-name>
    <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>HiddenHttpMethodFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```
注：
**目前为止，SpringMVC 中提供了两个过滤器：CharacterEncodingFilter 和 HiddenHttpMethodFilter。**
在 web. xml 中注册时，必须先注册 CharacterEncodingFilter，再注册 HiddenHttpMethodFilter。
原因：
-   在 CharacterEncodingFilter 中通过 request. setCharacterEncoding (encoding) 方法设置字符集的。
-   request.setCharacterEncoding(encoding) 方法要求前面不能有任何获取请求参数的操作
-   而 HiddenHttpMethodFilter 恰恰有一个获取请求方式的操作：
```java
String paramValue = request.getParameter(this.methodParam);
```
## 简单的增删改查案例
![[图片/Pasted image 20221231195445.png]]

控制器方法
```java
@Controller  
public class TestController {  
  
    @Autowired  
    EmployeeDao employeeDao;  
  
    //跳转首页  
    @RequestMapping("/")  
    public String testindex() {  
        return "index";  
    }  
  
    //跳转添加页面  
    @RequestMapping(value = "/toAdd")  
    public String toAdd() {  
        return "addemployee";  
    }  
  
    //显示全部员工  
    @GetMapping("/employee")  
    public String getAllEmployee(Model model) {  
        Collection<Employee> employees = employeeDao.getAll();  
        model.addAttribute("employees", employees);  
        return "employeelist";  
    }  
  
    //删除  
    @DeleteMapping("/employee/{id}")  
    public String deleteEmployee(@PathVariable("id") Integer id) {  
        employeeDao.delete(id);  
        return "redirect:/employee";  
    }  
  
    //添加  
    @PostMapping("/employee")  
    public String addEmployee(Employee employee) {  
        employeeDao.save(employee);  
        return "redirect:employee";  
    }  
  
    //跳转修改的页面  
    @GetMapping("/employee/{id}")  
    public String toUpdate(@PathVariable("id") Integer id, Model model) {  
        Employee employee = employeeDao.get(id);  
        model.addAttribute("employee", employee);  
        return "updateemployee";  
    }  
  
    /*  
    修改  
     */    @PutMapping("/employee")  
    public String updateEmployee(Employee employee) {  
        employeeDao.save(employee);  
        return "redirect:employee";  
    }  
}
```
Bean 文件
```java
package com.zml.bean;  
  
/**  
 * @author 张民磊  
 * @version 1.0  
 */public class Employee {  
  
    private Integer id;  
    private String lastName;  
  
    private String email;  
    //1 male, 0 female  
    private Integer gender;  
  
    public Integer getId() {  
        return id;  
    }  
  
    public void setId(Integer id) {  
        this.id = id;  
    }  
  
    public String getLastName() {  
        return lastName;  
    }  
  
    public void setLastName(String lastName) {  
        this.lastName = lastName;  
    }  
  
    public String getEmail() {  
        return email;  
    }  
  
    public void setEmail(String email) {  
        this.email = email;  
    }  
  
    public Integer getGender() {  
        return gender;  
    }  
  
    public void setGender(Integer gender) {  
        this.gender = gender;  
    }  
  
    public Employee(Integer id, String lastName, String email, Integer gender) {  
        super();  
        this.id = id;  
        this.lastName = lastName;  
        this.email = email;  
        this.gender = gender;  
    }  
  
    public Employee() {  
    }  
    @Override  
    public String toString() {  
        return "Employee{" +  
                "id=" + id +  
                ", lastName='" + lastName + '\'' +  
                ", email='" + email + '\'' +  
                ", gender=" + gender +  
                '}';  
    }  
}
```
Dao 层
```java
package com.zml.dao;  
  
import com.zml.bean.Employee;  
import org.springframework.stereotype.Repository;  
  
import java.util.Map;  
import java.util.Collection;  
import java.util.HashMap;  
  
/**  
 * @author 张民磊  
 * @version 1.0  
 */@Repository  
public class EmployeeDao {  
  
    private static Map<Integer, Employee> employees = null;  
  
    static {  
        employees = new HashMap<Integer, Employee>();  
  
        employees.put(1001, new Employee(1001, "E-AA", "aa@163.com", 1));  
        employees.put(1002, new Employee(1002, "E-BB", "bb@163.com", 1));  
        employees.put(1003, new Employee(1003, "E-CC", "cc@163.com", 0));  
        employees.put(1004, new Employee(1004, "E-DD", "dd@163.com", 0));  
        employees.put(1005, new Employee(1005, "E-EE", "ee@163.com", 1));  
    }  
  
    private static Integer initId = 1006;  
  
    public void save(Employee employee) {  
        if (employee.getId() == null) {  
            employee.setId(initId++);  
        }  
        employees.put(employee.getId(), employee);  
    }  
  
    public Collection<Employee> getAll() {  
        return employees.values();  
    }  
  
    public Employee get(Integer id) {  
        return employees.get(id);  
    }  
  
    public void delete(Integer id) {  
        employees.remove(id);  
    }  
}
```

Html 文件
- 显示所有员工页面
```html
<!DOCTYPE html>  
<html lang="en" xmlns:th="http://www.thymeleaf.org">  
<head>  
    <meta charset="UTF-8">  
    <title>Title</title>  
</head>  
<body>  
<table id="dataTable">  
    <tr>  
        <th colspan="5">Employee Info</th>  
    </tr>  
    <tr>  
        <th>id</th>  
        <th>lastname</th>  
        <th>email</th>  
        <th>gender</th>  
        <th>options <a th:href="@{/toAdd}">添加</a></th>  
    </tr>  
    <tr th:each="employee : ${employees}">  
        <td th:text="${employee.id}"></td>  
        <td th:text="${employee.lastName}"></td>  
        <td th:text="${employee.email}"></td>  
        <td th:text="${employee.gender}"></td>  
        <td>  
            <!--            <a @cilck="deleteemployee" th:href="@{/employeelist}+${employee.id}">delete</a>-->  
            <a @click="deleteemployee" th:href="@{'/employee/'+${employee.id}}">delete</a>  
            <a th:href="@{'/employee/'+${employee.id}}">update</a>  
            <!--            delete功能跳转实现，因为在thymeleaf @{}会被解析为路径 ${}会被解析为代码，所以我们不能直接写，可以进行拼接或者加上单引号，  
                            上面两种实现都可以            -->  
  
        </td>  
    </tr>  
</table>  
<form id=deleteFrom method="post">  
    <input type="hidden" name="_method" value="delete">  
</form>  
<script type="text/javascript" th:src="@{/static/vue.js}"></script>  
<script type="text/javascript">  
    var vue = new Vue({  
        el: "#dataTable",  
        methods: {  
            deleteemployee: function (event) {  
                var deleteFrom = document.getElementById("deleteFrom")  
                //根据id获取表单  
                deleteFrom.action = event.target.href;  
                //    获取当前触发点击事件的超链接href属性赋值给表单的action  
                deleteFrom.submit();  
                //    提交表单  
                event.preventDefault();  
                //    取消超链接的默认行为  
            }  
        }  
    });  
</script>  
</body>  
</html>
```

- 修改员工页面
```html
<!DOCTYPE html>  
<html lang="en" xmlns:th="http://www.thymeleaf.org">  
<head>  
    <meta charset="UTF-8">  
    <title>Title</title>  
</head>  
<body>  
  
<table>  
    <tr>  
        <th colspan="5">Employee Info</th>  
    </tr>  
    <tr>  
        <th>id</th>  
        <th>lastname</th>  
        <th>email</th>  
        <th>gender</th>  
    </tr>  
    <tr th:each="emp : ${employee}">  
        <td th:text="${employee.id}"></td>  
        <td th:text="${employee.lastName}"></td>  
        <td th:text="${employee.email}"></td>  
        <td th:text="${employee.gender}"></td>  
    </tr>  
</table>  
<br>  
<form method="post" th:action="@{/employee}">  
    <input type="hidden" name="_method" value="PUT">  
    id <input type="text" name="id">  
    ListName:<input type="text" name="lastName">  
    email:<input type="text" name="email">  
    gender<input type="radio" name="gender" value="1">男  
    gender<input type="radio" name="gender" value="0">女<br>  
    <input type="submit" value="修改~">  
</form>  
</body>  
</html>
```

- 增加页面 html
```html
<!DOCTYPE html>  
<html lang="en" xmlns:th="http://www.thymeleaf.org">  
<head>  
    <meta charset="UTF-8">  
    <title>addemployee</title>  
</head>  
<body>  
<form method="post" th:action="@{/employee}">  
    ListName:<input type="text" name="lastName">  
    email:<input type="text" name="email">  
    gender<input type="radio" name="gender" value="1">男  
    gender<input type="radio" name="gender" value="0">女<br>  
    <input type="submit" value="增加~">  
</form>  
</body>  
</html>
```
## 删除功能的实现
因为 delete 请求需要用 PSOT 去发送，我们用到 jS 和 VUE，（vue 后面补习先试着理解），当我们导入 vue 静态资源，需要开放对静态资源的访问，在 SpringMVC. Xml 中进行配置，但前提是要配置注解驱动，否则无效。 [[SpringMVC访问静态资源]]
当我们的服务器的配置文件和 Tomcat 配置文件冲突的时候，就近原则为准。开启只有会有一些莫名奇妙的 bug，会导致 404。
```xml
<mvc:default-servlet-handler/>
<!--   开启静态资源访问权限，如果SpringMVC没有找到，会交给原生的Servlet去处理-->
<mvc:annotation-driven/>
<!--   开启注解驱动-->
```
# HttpMessageConverter
HttpMessageConverter，*报文信息转换器，将请求报文转换为 Java 对象，或将 Java 对象转换为响应报文.*
HttpMessageConverter 提供了两个注解和两个类型：
@RequestBody @ResponseBody，      
RequestEntity,   ResponseEntity
响应用的比较多。
**只有 POST 请求才有请求体。**

## @RequestBody 请求体注解
@RequestBody 可以获取**请求体**，**需要在控制器方法设置一个形参**，使用@RequestBody 进行标识，当前请求的请求体就会为当前注解所标识的形参赋值。
**只有 POST 请求才有请求体。**
```java
@RequestMapping("testRequestBody")  
public String testRequestBody(@RequestBody String requestBody) {  
    System.out.println(requestBody);  
    return "success";  
}
```

## RequestEntity 请求报文类型
RequestEntity 封装请求报文的一种类型，需要在控制器方法的形参中设置该类型的形参，当前请求的请求报文就会赋值给该形参，**可以通过 getHeaders () 获取请求头信息，通过 getBody () 获取请求体信息。**
```java
@RequestMapping("/testRequestEntity")  
public String testRequestEntity(RequestEntity<String> entity) {  
    System.out.println("请求头：" + entity.getHeaders());  
    System.out.println("请求体：" + entity.getBody());  
    return "success";  
}
```

## HttpServletRespones 使用
使用 Servlet 原生 API 来返回响应
```java
@RequestMapping("/testRespones")  
public void testResponees(HttpServletResponse response) throws IOException {  
    response.getWriter().write("你好，respones");  
}
```

## @ResponseBody 
@ResponseBody 用于标识一个控制器方法，可以将该方法的返回值直接作为响应报文的响应体响应到浏览器。**我们控制器的方法返回值直接作为响应报文响应到浏览器。**
```java
@RequestMapping("/testResponBody")  
@ResponseBody  
public String testRespones() {  
    return "hi";  
}
```
## SpringMVC 处理 json
@ResponseBody 处理 json 的步骤：
a>导入 jackson 的依赖
```xml
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.12.1</version>
</dependency>
```
b>在 SpringMVC 的核心配置文件中开启 mvc 的注解驱动，此时在 HandlerAdaptor 中会自动装配一个消息转换器：MappingJackson2HttpMessageConverter，可以将响应到浏览器的 Java 对象转换为 Json 格式的字符串
```xml
<mvc:annotation-driven />

```
c>在处理器方法上使用@ResponseBody 注解进行标识
d>将 Java 对象直接作为控制器方法的返回值返回，就会自动转换为 Json 格式的字符串
```java
@RequestMapping("/testResponseUser")
@ResponseBody
public User testResponseUser(){
    return new User(1001,"admin","123456",23,"男");
}
```
此时，浏览器的页面中展示的结果：
{“id”: 1001,“username”:“admin”,“password”:“123456”,“age”: 23,“sex”:“男”}。

## SpringMVC 处理 ajax
a>请求超链接：
```html
<div id="app">
    <a th:href="@{/testAjax}" @click="testAjax">testAjax</a><br>
</div>
123
```
b>通过vue和axios处理点击事件：
```html
<script type="text/javascript" th:src="@{/static/js/vue.js}"></script>
<script type="text/javascript" th:src="@{/static/js/axios.min.js}"></script>
<script type="text/javascript">
    var vue = new Vue({
        el:"#app",
        methods:{
            testAjax:function (event) {
                axios({
                    method:"post",
                    url:event.target.href,
                    params:{
                        username:"admin",
                        password:"123456"
                    }
                }).then(function (response) {
                    alert(response.data);
                });
                event.preventDefault();
            }
        }
    });
</script>
12345678910111213141516171819202122
```
c>控制器方法：
```java
@RequestMapping("/testAjax")
@ResponseBody
public String testAjax(String username, String password){
    System.out.println("username:"+username+",password:"+password);
    return "hello,ajax";
}

```
## @RestController 注解
@RestController 注解是 springMVC 提供的一个复合注解，标识在控制器的类上，**就相当于为类添加了@Controller 注解，并且为其中的每个方法添加了@ResponseBody 注解。**

## ResponeseEntity  
ResponseEntity 用于控制器方法的返回值类型，该控制器方法的返回值就是响应到浏览器的响应报文。可以由我们自定义响应报文发送给服务器。
创建响应报文，Respones 有三个构造函数有三个，一个是响应体，传输的文件，响应头，响应状态码。

# 文件的的上传和下载
## 文件下载

```java
  @RequestMapping("testfiledownload")  
    public ResponseEntity<byte[]> testFileDownLoad(HttpSession session) throws IOException {  
        ServletContext servletContext = session.getServletContext();  
//        获得servletContext对象  
        String realPath = servletContext.getRealPath("/static/imgs/one.JPG");  
//      获得资源路径在服务器中的路径  
        FileInputStream fileInputStream = new FileInputStream(realPath);  
//        获得流  
        byte[] bytes = new byte[fileInputStream.available()];  
//        创建字符数组存放字节流，大小为文件的大小， available方法是查看文件大小  
        fileInputStream.read(bytes);  
        MultiValueMap<String, String> headers = new HttpHeaders();  
//        因为响应头是键值对的形式，所以使用map数组的存起来  
        headers.add("Content-Disposition", "attachment;filename=touxiang.jpg");  
//设置响应头信息 利用附件下载，这是固定的不可以随便修改。  
        HttpStatus httpcode = HttpStatus.OK;  
//        这是响应状态码  
        ResponseEntity<byte[]> responseEntity = new ResponseEntity<>(bytes, headers, httpcode);  
//        创建响应报文，Respones有三个构造函数有三个，一个是响应体，传输的文件， 响应头，响应状态码。  
        return responseEntity;  
    }
```

## 文件上传
文件上传要求 form 表单的请求方式必须为 post，并且添加属 **enctype=“multipart/form-data”**
SpringMVC 中将上传的文件封装到 MultipartFile 对象中，通过此对象可以获取文件相关信息。
上传文件：
1.要先添加依赖
```xml
<!-- https://mvnrepository.com/artifact/commons-fileupload/commons-fileupload -->
<dependency>
    <groupId>commons-fileupload</groupId>
    <artifactId>commons-fileupload</artifactId>
    <version>1.3.1</version>
</dependency>
```
2. 在 SpringMVC。Xml 添加配置： 
因为 Spirngmvc 是通过 id 查找 bena 的，如果没有 id 的话是查找不到这个 bean 配置的
```xml
<!--必须通过文件解析器的解析才能将文件转换为MultipartFile对象-->
<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver"></bean>
```
3. 控制器方法

如果上传同名文件会导致将上一次的文件覆盖，我们使用 UUID 来替换文件名，获取一个 32 位不重复的名字。
```java
  @RequestMapping("/testUp")  
    public String testUp(MultipartFile photo, HttpSession session) throws IOException {  
        //SpringMVC把我们上传的文件类型和方法封装好了。为MultipartFile  
        String originalFilename = photo.getOriginalFilename();  
//        获得上传文件名  
        String substring = originalFilename.substring(originalFilename.lastIndexOf("."));  
        String uuid = UUID.randomUUID().toString().replaceAll("-", "");  
        originalFilename = uuid + substring;  
        //字符串进行拼接  
        ServletContext servletContext = session.getServletContext();  
        String photoPath = servletContext.getRealPath("photo");  
//        获得photo的路径  
        File file = new File(photoPath);  
        if (!file.exists()) {  
            file.mkdir();  
//            盘对文件是否存在，不存在就创建路径  
        }  
        String finalPath = photoPath + File.separator + originalFilename;  
//            将上传路径整合保存到finalpath变量，File.separator代表文件路径分隔符。  
        photo.transferTo(new File(finalPath));  
        return "success";  
    }
```
# 拦截器
## 拦截器的配置
SpringMVC 中的拦截器用于**拦截控制器方法的执行**
SpringMVC中的拦截器**需要实现HandlerInterceptor**
SpringMVC 的拦截器必须在 SpringMVC 的配置文件中进行配置：
```xml
<mvc:interceptors>
<bean class="com.atguigu.interceptor.FirstInterceptor"></bean>

<ref bean="firstInterceptor"></ref>
<!-- 以上两种配置方式都是对 DispatcherServlet 所处理的所有的请求进行拦截
第二种方式需要在类上面加上注解，就可以引用到id
-->
<mvc:interceptor>
    <mvc:mapping path="/**"/>
    <mvc:exclude-mapping path="/testRequestEntity"/>
    <ref bean="firstInterceptor"></ref>
</mvc:interceptor>
</mvc:interceptors>
<!-- 
    以上配置方式可以通过ref或bean标签设置拦截器，通过mvc:mapping设置需要拦截的请求。
    通过mvc:exclude-mapping设置需要排除的请求，即不需要拦截的请求。
    三种选择合适的即可。
-->
```

## 拦截器的三个抽象方法
SpringMVC中的拦截器有三个抽象方法：
preHandle：控制器方法执行之前执行preHandle()，**其boolean类型的返回值表示是否拦截或放行，返回true为放行，即调用控制器方法；返回false表示拦截，即不调用控制器方法**

postHandle：控制器方法执行之后执行postHandle()

afterComplation：处理完视图和模型数据，渲染视图完毕之后执行 afterComplation ()
![[图片/Pasted image 20230101203500.png]]

## 多个拦截器的执行顺序
a>若每个拦截器的 preHandle () 都返回 true
此时多个拦截器的**执行顺序和拦截器在SpringMVC的配置文件的配置顺序有关**：
preHandle () 会按照配置的顺序执行，而 postHandle () 和 afterComplation () 会按照配置的反序执行。

b>若某个拦截器的preHandle()返回了false
preHandle () 返回 false 和它之前的拦截器的 preHandle () 都会执行，postHandle () 都不执行，返回 false 的拦截器之前的拦截器的 afterComplation () 会执行

C>假如我我们有 5 个拦截器，第三个拦截器没有通过
1 2 3 的 preHandle () 会执行
都不执行 postHandle ()
1 2 会执行afterComplation ()

## 拦截器的使用
使用拦截器的话，新建类需要实现**HandlerInterceptor**接口。Ctrl+o 实现方法快捷键。
```java
public class testInterceptor implements HandlerInterceptor {  
    @Override  
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {  
        System.out.println(1);  
        return true;  
    }  
    @Override  
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {  
        System.out.println(2);  
    }  
    @Override  
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {  
        System.out.println(3);  
    }
    }
```
# 异常处理器
## 基于配置的异常处理器
SpringMVC 提供了一个处理控制器方法执行过程中所出现的异常的接口：HandlerExceptionResolver。

HandlerExceptionResolver接口的实现类有：DefaultHandlerExceptionResolver和SimpleMappingExceptionResolver

SpringMVC 提供了自定义的异常处理器 SimpleMappingExceptionResolver，使用方式：
exceptionAttribute 属性就相当于存储了这个异常信息，存储在请求域中的。value 就相当于键。
```xml
<bean class="org.springframework.web.servlet.handler.SimpleMappingExceptionResolver">
    <property name="exceptionMappings">
        <props>
            <!--
                properties的键表示处理器方法执行过程中出现的异常
                properties的值表示若出现指定异常时，设置一个新的视图名称，跳转到指定页面
            -->
            <prop key="java.lang.ArithmeticException">error</prop>
            error相当于跳转的页面名字
        </props>
    </property>
    <property name="exceptionAttribute" value="ex"></property>
    //当我们设置了这个属性，就相当于存储了这个错误信息，存储在请求域中的。value就相当于键。
</bean>
```

^805c13

##  基于注解的异常处理
@ControllerAdvice ：表示将当前类标示为异常处理的组件
@ExceptionHandler ：表示出现异常信息执行这个控制其方法，value 属性存放可能出现的异常。是一个数组。
形参位置的 Exeption 表示出现的异常信息。将错误信息存放到请求域对象中，显示到页面上面。 
```java
@ControllerAdvice  
public class ExceptionController {  
  
    @ExceptionHandler(value = {ArithmeticException.class, NullPointerException.class})  
    public String testerror(Exception exception, Model model) {  
        model.addAttribute("ex", exception);  
        return "error";  
    }//    ExceptionHandler标志代表出现异常执行这个控制器方法，value属性是一个数组，里面存放可能出现的异常。  
//    形参位置的Exeption表示出现的异常信息。将错误信息存放到请求域对象中，显示到页面上面。  
}
```

# 注解配置 SpringMVC
**使用配置类和注解代替 web. xml 和 SpringMVC 配置文件的功能**
## 1. 创建初始类，代替 web. Xml
在 Servlet3.0 环境中，容器会在类路径中查找实现 javax. servlet. ServletContainerInitializer 接口的类，如果找到的话就用它来配置 Servlet 容器。 Spring 提供了这个接口的实现，名为 SpringServletContainerInitializer，这个类反过来又会查找实现 WebApplicationInitializer 的类并将配置的任务交给它们来完成。Spring3.2 引入了一个便利的 WebApplicationInitializer 基础实现，名为 **AbstractAnnotationConfigDispatcherServletInitializer**，当我们的类扩展了 AbstractAnnotationConfigDispatcherServletInitializer 并将其部署到 Servlet3.0 容器的时候，容器会自动发现它，并用它来配置 Servlet 上下文。
该接口的三个实现方法：
getRootConfigClasses ()：获取当前根配置，也就是指定 Spring 的配置类。
getServletConfigClasses ()：指定 SpringMVC 的配置类。
getServletMappings ()： 指定 DispatcherServlet 的映射规则，即是 url-pattern
getServletFilters (): 注册过滤器；
```java
public class WebInit extends AbstractAnnotationConfigDispatcherServletInitializer {  
  
  
    @Override  
    protected Class<?>[] getRootConfigClasses() {  
        return new Class[]{SpringConfig.class};  
    }  
    @Override  
    protected Class<?>[] getServletConfigClasses() {  
        return new Class[]{WebConfig.class};  
    }  
    @Override  
    protected String[] getServletMappings() {  
        return new String[]{"/"};  
    }  
    @Override  
    protected Filter[] getServletFilters() {  
        CharacterEncodingFilter characterEncodingFilter = new CharacterEncodingFilter();  
        characterEncodingFilter.setEncoding("UTF-8");  
        characterEncodingFilter.setForceResponseEncoding(true);  
        HiddenHttpMethodFilter hiddenHttpMethodFilter = new HiddenHttpMethodFilter();  
        return new Filter[]{characterEncodingFilter, hiddenHttpMethodFilter};  
    }
    }
```

## 2. 创建 SpringConfig 配置类，代替 spring 的配置类
```java
@Configuration
public class SpringConfig {
    //ssm整合之后，spring的配置信息写在此类中
}
1234
```
## 创建 WebConfig 配置类，代替 SpringMVC 的配置文件
SprngMVC 实现的功能：扫描组件，视图解析器，[[SpringMVC#^f2ff49|view-controller]]，开启静态资源访问，文件上传解析器，异常处理，拦截器。
@ComponentScan：扫描组件
@EnableWebMvc：开启注解驱动
@Configuration：设置为配置类
方法上的形参是 Spring 自动装配。
```java
@Configuration  
//扫描组件  
@ComponentScan("com.zml")  
//开启MVC注解驱动  
@EnableWebMvc  
public class WebConfig implements WebMvcConfigurer {  
  
    //使用默认的servlet处理静态资源  
    @Override  
    public void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer) {  
        configurer.enable();  
    }  
    //配置文件上传解析器  
    @Bean  
    public CommonsMultipartResolver multipartResolver() {  
        return new CommonsMultipartResolver();  
    }  
    //配置拦截器  
    @Override  
    public void addInterceptors(InterceptorRegistry registry) {  
        testInterceptors testInterceptors = new testInterceptors();  
        registry.addInterceptor(testInterceptors).addPathPatterns("/**");  
    }  
    //配置视图控制  
  
    /*@Override  
    public void addViewControllers(ViewControllerRegistry registry) {        registry.addViewController("/").setViewName("index");    }*/  
    //配置异常映射  
    /*@Override  
    public void configureHandlerExceptionResolvers(List<HandlerExceptionResolver> resolvers) {        SimpleMappingExceptionResolver exceptionResolver = new SimpleMappingExceptionResolver();        Properties prop = new Properties();        prop.setProperty("java.lang.ArithmeticException", "error");        //设置异常映射  
        exceptionResolver.setExceptionMappings(prop);        //设置共享异常信息的键  
        exceptionResolver.setExceptionAttribute("ex");        resolvers.add(exceptionResolver);    }*/  
    //配置生成模板解析器  
    @Bean  
    public ITemplateResolver templateResolver() {  
        WebApplicationContext webApplicationContext = ContextLoader.getCurrentWebApplicationContext();  
        // ServletContextTemplateResolver需要一个ServletContext作为构造参数，可通过WebApplicationContext 的方法获得  
        ServletContextTemplateResolver templateResolver = new ServletContextTemplateResolver(  
                webApplicationContext.getServletContext());  
        templateResolver.setPrefix("/WEB-INF/templates/");  
        templateResolver.setSuffix(".html");  
        templateResolver.setCharacterEncoding("UTF-8");  
        templateResolver.setTemplateMode(TemplateMode.HTML);  
        return templateResolver;  
    }  
    //生成模板引擎并为模板引擎注入模板解析器  
    @Bean  
    public SpringTemplateEngine templateEngine(ITemplateResolver templateResolver) {  
        SpringTemplateEngine templateEngine = new SpringTemplateEngine();  
        templateEngine.setTemplateResolver(templateResolver);  
        return templateEngine;  
    }  
    //生成视图解析器并未解析器注入模板引擎  
    @Bean  
    public ViewResolver viewResolver(SpringTemplateEngine templateEngine) {  
        ThymeleafViewResolver viewResolver = new ThymeleafViewResolver();  
        viewResolver.setCharacterEncoding("UTF-8");  
        viewResolver.setTemplateEngine(templateEngine);  
        return viewResolver;  
    }  
  
}
```
# SpringMVC 执行流程
## SpringNVC 常用组件
-   DispatcherServlet：**前端控制器**，不需要工程师开发，由框架提供

作用：统一处理请求和响应，整个流程控制的中心，由它调用其它组件处理用户的请求

-   HandlerMapping：**处理器映射器**，不需要工程师开发，由框架提供

作用：根据请求的url、method等信息查找Handler，即控制器方法

-   Handler：**处理器**，需要工程师开发

作用：在DispatcherServlet的控制下Handler对具体的用户请求进行处理

-   HandlerAdapter：**处理器适配器**，不需要工程师开发，由框架提供

作用：通过HandlerAdapter对处理器（控制器方法）进行执行

-   ViewResolver：**视图解析器**，不需要工程师开发，由框架提供

作用：进行视图解析，得到相应的视图，例如：ThymeleafView、InternalResourceView、RedirectView

-   View：**视图**

作用：将模型数据通过页面展示给用户

## DispatcherServlet 初始化过程
DispatcherServlet 本质上是一个 Servlet，所以天然的遵循 Servlet 的生命周期。所以宏观上是 Servlet 生命周期来进行调度。
##### a>初始化WebApplicationContext

所在类：org.springframework.web.servlet.FrameworkServlet

```
protected WebApplicationContext initWebApplicationContext() {
    WebApplicationContext rootContext =
        WebApplicationContextUtils.getWebApplicationContext(getServletContext());
    WebApplicationContext wac = null;

    if (this.webApplicationContext != null) {
        // A context instance was injected at construction time -> use it
        wac = this.webApplicationContext;
        if (wac instanceof ConfigurableWebApplicationContext) {
            ConfigurableWebApplicationContext cwac = (ConfigurableWebApplicationContext) wac;
            if (!cwac.isActive()) {
                // The context has not yet been refreshed -> provide services such as
                // setting the parent context, setting the application context id, etc
                if (cwac.getParent() == null) {
                    // The context instance was injected without an explicit parent -> set
                    // the root application context (if any; may be null) as the parent
                    cwac.setParent(rootContext);
                }
                configureAndRefreshWebApplicationContext(cwac);
            }
        }
    }
    if (wac == null) {
        // No context instance was injected at construction time -> see if one
        // has been registered in the servlet context. If one exists, it is assumed
        // that the parent context (if any) has already been set and that the
        // user has performed any initialization such as setting the context id
        wac = findWebApplicationContext();
    }
    if (wac == null) {
        // No context instance is defined for this servlet -> create a local one
        // 创建WebApplicationContext
        wac = createWebApplicationContext(rootContext);
    }

    if (!this.refreshEventReceived) {
        // Either the context is not a ConfigurableApplicationContext with refresh
        // support or the context injected at construction time had already been
        // refreshed -> trigger initial onRefresh manually here.
        synchronized (this.onRefreshMonitor) {
            // 刷新WebApplicationContext
            onRefresh(wac);
        }
    }

    if (this.publishContext) {
        // Publish the context as a servlet context attribute.
        // 将IOC容器在应用域共享
        String attrName = getServletContextAttributeName();
        getServletContext().setAttribute(attrName, wac);
    }

    return wac;
}
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354
```

##### b>创建WebApplicationContext

所在类：org.springframework.web.servlet.FrameworkServlet

```
protected WebApplicationContext createWebApplicationContext(@Nullable ApplicationContext parent) {
    Class<?> contextClass = getContextClass();
    if (!ConfigurableWebApplicationContext.class.isAssignableFrom(contextClass)) {
        throw new ApplicationContextException(
            "Fatal initialization error in servlet with name '" + getServletName() +
            "': custom WebApplicationContext class [" + contextClass.getName() +
            "] is not of type ConfigurableWebApplicationContext");
    }
    // 通过反射创建 IOC 容器对象
    ConfigurableWebApplicationContext wac =
        (ConfigurableWebApplicationContext) BeanUtils.instantiateClass(contextClass);

    wac.setEnvironment(getEnvironment());
    // 设置父容器
    wac.setParent(parent);
    String configLocation = getContextConfigLocation();
    if (configLocation != null) {
        wac.setConfigLocation(configLocation);
    }
    configureAndRefreshWebApplicationContext(wac);

    return wac;
}
1234567891011121314151617181920212223
```

##### c>DispatcherServlet初始化策略

FrameworkServlet创建WebApplicationContext后，刷新容器，调用onRefresh(wac)，此方法在DispatcherServlet中进行了重写，调用了initStrategies(context)方法，初始化策略，即初始化DispatcherServlet的各个组件

所在类：org.springframework.web.servlet.DispatcherServlet

```
protected void initStrategies(ApplicationContext context) {
   initMultipartResolver(context);
   initLocaleResolver(context);
   initThemeResolver(context);
   initHandlerMappings(context);
   initHandlerAdapters(context);
   initHandlerExceptionResolvers(context);
   initRequestToViewNameTranslator(context);
   initViewResolvers(context);
   initFlashMapManager(context);
}
1234567891011
```

## DispatcherServlet 调用组件处理请求
##### a>processRequest()

FrameworkServlet重写HttpServlet中的service()和doXxx()，这些方法中调用了processRequest(request, response)

所在类：org.springframework.web.servlet.FrameworkServlet

```
protected final void processRequest(HttpServletRequest request, HttpServletResponse response)
    throws ServletException, IOException {

    long startTime = System.currentTimeMillis();
    Throwable failureCause = null;

    LocaleContext previousLocaleContext = LocaleContextHolder.getLocaleContext();
    LocaleContext localeContext = buildLocaleContext(request);

    RequestAttributes previousAttributes = RequestContextHolder.getRequestAttributes();
    ServletRequestAttributes requestAttributes = buildRequestAttributes(request, response, previousAttributes);

    WebAsyncManager asyncManager = WebAsyncUtils.getAsyncManager(request);
    asyncManager.registerCallableInterceptor(FrameworkServlet.class.getName(), new RequestBindingInterceptor());

    initContextHolders(request, localeContext, requestAttributes);

    try {
        // 执行服务，doService()是一个抽象方法，在DispatcherServlet中进行了重写
        doService(request, response);
    }
    catch (ServletException | IOException ex) {
        failureCause = ex;
        throw ex;
    }
    catch (Throwable ex) {
        failureCause = ex;
        throw new NestedServletException("Request processing failed", ex);
    }

    finally {
        resetContextHolders(request, previousLocaleContext, previousAttributes);
        if (requestAttributes != null) {
            requestAttributes.requestCompleted();
        }
        logResult(request, response, failureCause, asyncManager);
        publishRequestHandledEvent(request, response, startTime, failureCause);
    }
}
123456789101112131415161718192021222324252627282930313233343536373839
```

##### b>doService ()

所在类：org.springframework.web.servlet.DispatcherServlet

```
@Override
protected void doService(HttpServletRequest request, HttpServletResponse response) throws Exception {
    logRequest(request);

    // Keep a snapshot of the request attributes in case of an include,
    // to be able to restore the original attributes after the include.
    Map<String, Object> attributesSnapshot = null;
    if (WebUtils.isIncludeRequest(request)) {
        attributesSnapshot = new HashMap<>();
        Enumeration<?> attrNames = request.getAttributeNames();
        while (attrNames.hasMoreElements()) {
            String attrName = (String) attrNames.nextElement();
            if (this.cleanupAfterInclude || attrName.startsWith(DEFAULT_STRATEGIES_PREFIX)) {
                attributesSnapshot.put(attrName, request.getAttribute(attrName));
            }
        }
    }

    // Make framework objects available to handlers and view objects.
    request.setAttribute(WEB_APPLICATION_CONTEXT_ATTRIBUTE, getWebApplicationContext());
    request.setAttribute(LOCALE_RESOLVER_ATTRIBUTE, this.localeResolver);
    request.setAttribute(THEME_RESOLVER_ATTRIBUTE, this.themeResolver);
    request.setAttribute(THEME_SOURCE_ATTRIBUTE, getThemeSource());

    if (this.flashMapManager != null) {
        FlashMap inputFlashMap = this.flashMapManager.retrieveAndUpdate(request, response);
        if (inputFlashMap != null) {
            request.setAttribute(INPUT_FLASH_MAP_ATTRIBUTE, Collections.unmodifiableMap(inputFlashMap));
        }
        request.setAttribute(OUTPUT_FLASH_MAP_ATTRIBUTE, new FlashMap());
        request.setAttribute(FLASH_MAP_MANAGER_ATTRIBUTE, this.flashMapManager);
    }

    RequestPath requestPath = null;
    if (this.parseRequestPath && !ServletRequestPathUtils.hasParsedRequestPath(request)) {
        requestPath = ServletRequestPathUtils.parseAndCache(request);
    }

    try {
        // 处理请求和响应
        doDispatch(request, response);
    }
    finally {
        if (!WebAsyncUtils.getAsyncManager(request).isConcurrentHandlingStarted()) {
            // Restore the original attribute snapshot, in case of an include.
            if (attributesSnapshot != null) {
                restoreAttributesAfterInclude(request, attributesSnapshot);
            }
        }
        if (requestPath != null) {
            ServletRequestPathUtils.clearParsedRequestPath(request);
        }
    }
}
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354
```

##### c>doDispatch()

所在类：org.springframework.web.servlet.DispatcherServlet

```
protected void doDispatch(HttpServletRequest request, HttpServletResponse response) throws Exception {
    HttpServletRequest processedRequest = request;
    HandlerExecutionChain mappedHandler = null;
    boolean multipartRequestParsed = false;

    WebAsyncManager asyncManager = WebAsyncUtils.getAsyncManager(request);

    try {
        ModelAndView mv = null;
        Exception dispatchException = null;

        try {
            processedRequest = checkMultipart(request);
            multipartRequestParsed = (processedRequest != request);

            // Determine handler for the current request.
            /*
                mappedHandler：调用链
                包含handler、interceptorList、interceptorIndex
                handler：浏览器发送的请求所匹配的控制器方法
                interceptorList：处理控制器方法的所有拦截器集合
                interceptorIndex：拦截器索引，控制拦截器afterCompletion()的执行
            */
            mappedHandler = getHandler(processedRequest);
            if (mappedHandler == null) {
                noHandlerFound(processedRequest, response);
                return;
            }

            // Determine handler adapter for the current request.
               // 通过控制器方法创建相应的处理器适配器，调用所对应的控制器方法
            HandlerAdapter ha = getHandlerAdapter(mappedHandler.getHandler());

            // Process last-modified header, if supported by the handler.
            String method = request.getMethod();
            boolean isGet = "GET".equals(method);
            if (isGet || "HEAD".equals(method)) {
                long lastModified = ha.getLastModified(request, mappedHandler.getHandler());
                if (new ServletWebRequest(request, response).checkNotModified(lastModified) && isGet) {
                    return;
                }
            }
            
            // 调用拦截器的preHandle()
            if (!mappedHandler.applyPreHandle(processedRequest, response)) {
                return;
            }

            // Actually invoke the handler.
            // 由处理器适配器调用具体的控制器方法，最终获得ModelAndView对象
            mv = ha.handle(processedRequest, response, mappedHandler.getHandler());

            if (asyncManager.isConcurrentHandlingStarted()) {
                return;
            }

            applyDefaultViewName(processedRequest, mv);
            // 调用拦截器的postHandle()
            mappedHandler.applyPostHandle(processedRequest, response, mv);
        }
        catch (Exception ex) {
            dispatchException = ex;
        }
        catch (Throwable err) {
            // As of 4.3, we're processing Errors thrown from handler methods as well,
            // making them available for @ExceptionHandler methods and other scenarios.
            dispatchException = new NestedServletException("Handler dispatch failed", err);
        }
        // 后续处理：处理模型数据和渲染视图
        processDispatchResult(processedRequest, response, mappedHandler, mv, dispatchException);
    }
    catch (Exception ex) {
        triggerAfterCompletion(processedRequest, response, mappedHandler, ex);
    }
    catch (Throwable err) {
        triggerAfterCompletion(processedRequest, response, mappedHandler,
                               new NestedServletException("Handler processing failed", err));
    }
    finally {
        if (asyncManager.isConcurrentHandlingStarted()) {
            // Instead of postHandle and afterCompletion
            if (mappedHandler != null) {
                mappedHandler.applyAfterConcurrentHandlingStarted(processedRequest, response);
            }
        }
        else {
            // Clean up any resources used by a multipart request.
            if (multipartRequestParsed) {
                cleanupMultipart(processedRequest);
            }
        }
    }
}
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293
```

##### d>processDispatchResult()

```
private void processDispatchResult(HttpServletRequest request, HttpServletResponse response,
                                   @Nullable HandlerExecutionChain mappedHandler, @Nullable ModelAndView mv,
                                   @Nullable Exception exception) throws Exception {

    boolean errorView = false;

    if (exception != null) {
        if (exception instanceof ModelAndViewDefiningException) {
            logger.debug("ModelAndViewDefiningException encountered", exception);
            mv = ((ModelAndViewDefiningException) exception).getModelAndView();
        }
        else {
            Object handler = (mappedHandler != null ? mappedHandler.getHandler() : null);
            mv = processHandlerException(request, response, handler, exception);
            errorView = (mv != null);
        }
    }

    // Did the handler return a view to render?
    if (mv != null && !mv.wasCleared()) {
        // 处理模型数据和渲染视图
        render(mv, request, response);
        if (errorView) {
            WebUtils.clearErrorRequestAttributes(request);
        }
    }
    else {
        if (logger.isTraceEnabled()) {
            logger.trace("No view rendering, null ModelAndView returned.");
        }
    }

    if (WebAsyncUtils.getAsyncManager(request).isConcurrentHandlingStarted()) {
        // Concurrent handling started during a forward
        return;
    }

    if (mappedHandler != null) {
        // Exception (if any) is already handled..
        // 调用拦截器的afterCompletion()
        mappedHandler.triggerAfterCompletion(request, response, null);
    }
}
12345678910111213141516171819202122232425262728293031323334353637383940414243
```

## SpringMVC 的执行流程
1.  用户向服务器发送请求，请求被SpringMVC 前端控制器 DispatcherServlet捕获。
2.  DispatcherServlet对请求URL进行解析，得到请求资源标识符（URI），判断请求URI对应的映射：

a) 不存在

i. 再判断是否配置了mvc:default-servlet-handler

ii. 如果没配置，则控制台报映射查找不到，客户端展示 404 错误。
***
iii. 如果有配置，则访问目标资源（一般为静态资源，如：JS,CSS,HTML），找不到客户端也会展示404错误

b) 存在则执行下面的流程
1.  根据该URI，调用HandlerMapping获得该Handler配置的所有相关的对象（包括Handler对象以及Handler对象对应的拦截器），最后以HandlerExecutionChain执行链对象的形式返回。
2.  DispatcherServlet 根据获得的 Handler，选择一个合适的 HandlerAdapter。
3.  如果成功获得HandlerAdapter，此时将开始执行拦截器的preHandler(…)方法【正向】
4.  提取 Request 中的模型数据，填充 Handler 形参，开始执行 Handler（Controller) 方法，处理请求。在填充 Handler 的入参过程中，根据你的配置，Spring 将帮你做一些额外的工作：

a) HttpMessageConveter： 将请求消息（如 Json、xml 等数据）转换成一个对象，将对象转换为指定的响应信息

b) 数据转换：对请求消息进行数据转换。如 String 转换成 Integer、Double 等

c) 数据格式化：对请求消息进行数据格式化。如将字符串转换成格式化数字或格式化日期等

d) 数据验证： 验证数据的有效性（长度、格式等），验证结果存储到BindingResult或Error中

1.  Handler执行完成后，向DispatcherServlet 返回一个ModelAndView对象。
2.  此时将开始执行拦截器的postHandle(…)方法【逆向】。
3.  根据返回的ModelAndView（此时会判断是否存在异常：如果存在异常，则执行HandlerExceptionResolver进行异常处理）选择一个适合的ViewResolver进行视图解析，根据Model和View，来渲染视图。
4.  渲染视图完毕执行拦截器的afterCompletion(…)方法【逆向】。
5.  将渲染结果返回给客户端。